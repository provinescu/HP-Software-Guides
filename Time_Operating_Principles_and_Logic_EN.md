# Time: Operating Principles and System Logic

**Sequencer architecture, data flow, synthesis, and spatialization**  
Version: October 4, 2026

> **Scope:** Based on the supplied Time source analysis. Available resources may vary with installed SynthDefs, presets, sounds, and extensions.

## 1. General Function

Time is a four-voice musical organizer. One central routine selects voices, constructs musical parameters, creates a Synth for every event, and routes the signal through a filter, effect, and spatializer.

| Layer | Role |
|---|---|
| Input | Audio, file, computer keyboard, or MIDI. |
| Analysis | Onset, pitch, amplitude, tempo, and spectrum. |
| Memory | Chords organized by pitch band. |
| Sequencer | Temporal voice selection. |
| Voices | Sample, synthesis, envelope, and jitter. |
| Processing | Filter, FX, panner, and MasterFX. |
| Organization | Presets, automation, MIDI, OSC, and VST. |

```text
input -> analysis -> band memory -> voice selection
      -> synthesis -> filter -> FX -> spatialization -> MasterFX
```

## 2. Initialization

1. Create or verify the Time data directory.
2. Configure server devices, channels, memory, and hardware buffer.
3. Open UDP ports and initialize Safety.
4. Load SynthDefs.
5. Create AudioRec, Synth, and Limiter groups.
6. Create audio buses and eight OSC data buses.
7. Build four Synth/Filter/FX/Panner chains.
8. Load the default sample.
9. Build windows and install shortcuts.

| Parameter | Role |
|---|---|
| `ni` | Number of audio inputs. |
| `numberOut` | Number of audio outputs. |
| `numberRec` | Recorded channels. |
| `format` | Stereo, Rotate2, MultiSpeaker, Ambisonic, or Dolby 5.1. |
| `size` | Hardware buffer size. |
| `scPort` | SuperCollider server port. |

## 3. Groups and Buses

| Structure | Function |
|---|---|
| `groupeAudioRec` | Sources, analysis, and live buffers. |
| `groupeSynth` | Parent of the four voices. |
| `listeGroupSynth` | Sound generators for each voice. |
| `listeGroupFilter` | Filters after synthesis. |
| `listeGroupFX` | Effects after filtering. |
| `listeGroupDolby` | Final spatialization. |
| `groupeLimiter` | VST and MasterFX. |
| `busOSC...` | Frequency, amplitude, duration, tempo, and spectral descriptors. |

Group order materializes the DSP chain and prevents an effect from being calculated before its source.

## 4. Analysis and OSC Messages

The Time Onset, Pitch, Pitch2, KeyTrack, and Keyboard SynthDefs send event replies. Time FFT periodically transmits centroid, flatness, energy, flux, and tempo.

| Data | Use |
|---|---|
| Frequency | Pitch and frequency-band placement. |
| Amplitude | Dynamics or Amp control. |
| Duration | Inter-onset time or Keyboard duration. |
| Tempo | Speed sent to mapped modules. |
| Flux / Flatness | Preset-automation triggers. |
| Energy | Auto Root and adaptive synthesis. |
| Centroid | Spectral and timbral parameters. |

## 5. Event Validation

An audio event enters memory only when frequency, amplitude, and duration differences exceed configured thresholds. Duration is bounded by the maximum memory time.

| Condition | Role |
|---|---|
| `fhzFilter` | Minimum pitch change in semitones. |
| `ampFilter` | Minimum amplitude change in dB. |
| `durFilter` | Minimum duration. |
| `maxDureeOSC` | Maximum duration and expiration. |
| `chordDureeOSC` | Chord-grouping interval. |
| `flagOSC / flagKeyboard / flagMIDI` | Active data-channel selection. |

This validation prevents small fluctuations from filling bands and triggering voices unnecessarily.

## 6. Harmonic Memory by Band

Band 0 receives every event. Other bands receive only frequencies within their boundaries. Each stored event includes MIDI note, octave, semitone, and cent information.

| Variable | Logic |
|---|---|
| `bandFHZ` | Band limits in Hz. |
| `listeDataBand` | Current chords per band. |
| `lastTimeBand` | Time of the last event. |
| `flagIndexBand` | Bands allowed for each voice. |
| `rangeNumFhzBand` | Bands from which a voice may choose. |

Chord size is limited by the four available synthesizer voices.

## 7. Central Sequencer

A single `Tdef` named `Sequencer` drives all four voices. At each loop, Time selects active voices, calculates duration, prepares parameters, creates Synth nodes, and waits before the next iteration.

| Mode | Selection logic |
|---|---|
| Rand | One active voice selected randomly. |
| Seq | Voices enabled on the current grid step. |
| WeightS | One voice selected from normalized weights. |
| WeightP | Several voices may pass a shared random threshold. |

Rand and WeightS favor one voice. Seq and WeightP may create polyphony.

## 8. Event Construction

For each selected voice, Time:

1. chooses a frequency band;
2. chooses sample or live buffer;
3. calculates read position and waveform jitter;
4. reads the envelope;
5. applies octave, semitone, cent, and pitch jitter;
6. applies amplitude and amplitude jitter;
7. prepares filter, effect, and spatial parameters;
8. adds analyzed pitch when Translate is enabled;
9. applies Ambitus, scale, Root, and pitch bend;
10. calculates playback rate, node lifetime, and MIDI output;
11. creates the SynthDef and maps OSC control buses.

## 9. Seq, Pitch, and Grain Duration Modes

| Mode | Synth lifetime |
|---|---|
| Seq | Directly from sequencer or analyzed duration. |
| Pitch | Selection duration divided or multiplied by playback rate. |
| Grain | Physical selection duration without rate correction. |

These modes determine node lifetime, not note choice.

## 10. Jitter

Jitter is recalculated around a preset value for every event. It does not modify the stored preset.

| Jitter | Domain |
|---|---|
| Waveform | Buffer-selection position. |
| Octave / Semitone / Cent | Pitch perturbation. |
| Amp | Variation in dB. |
| Filter / FX | Exponential variation of controls. |
| PanX / PanY | Spatial movement. |

## 11. Buffers and Live Audio Memory

Every voice owns a sample buffer and a live buffer. AudioRec or FileRec writes to the live buffer with `RecLevel` and `PreLevel`. `BufRdLive` uses two phase-shifted readers and crossfades between them.

| Parameter | Effect |
|---|---|
| RecLevel | Amount of new signal written. |
| PreLevel | Amount of old content retained. |
| Loop | Circular recording. |
| Trigger | One-shot restart. |
| `hp1 / hp2` | Anti-click controls for HP readers. |
| Safety | Dynamic margin between writing and reading. |

The dual-reader design maintains continuity when playback speed changes.

## 12. Synthesis Library

| Family | Examples |
|---|---|
| Buffers | TGrains, Warp1, BufRd, BufRdLive, LoopBuf, PlayBuf. |
| Spectral | HPshiftDown, MagShift, BinShift, BrickWall, MagSmooth. |
| Oscillators | SinOsc, SawSynth, FM, Ring, Formant, Klang. |
| Instruments | MdaPiano, Guitare, StringSynth. |
| Percussion | AnalogKick/Snare/HiHat, SOSkick/snare/hats/tom. |

OSC descriptors may become timbral parameters, linking incoming analysis to synthesis.

## 13. Filter, FX, and Panner Chain

Each Synth writes to `busInFilter`; the filter writes to `busInFX`; the effect writes to `busInDolby`; the panner writes to physical outputs.

| Stage | Controls |
|---|---|
| Filter | Three parameters, Dry/Wet, and jitter. |
| FX | Five parameters, Dry/Wet, and jitter. |
| Panner | PanX/PanY and jitter. |
| MasterFX | Limiter followed by PostAmp. |
| VST | Inserted in `groupeLimiter` before MasterFX. |

ByPassFilter and ByPassFX preserve chain continuity when processing is disabled.

## 14. Scale and Harmonic Adaptation

Incoming frequency is first remapped by Ambitus. If Scaling is enabled, Time selects the nearest available scale degree, then applies Root. Tunings include 12-TET and 22 shrutis. Auto Root converts spectral energy into a pitch class and updates the GUI root.

## 15. Preset Automation

Time FFT normalizes flux and flatness through Range FFT. When Automation is enabled and both variations exceed Threshold after the minimum interval, another preset is selected.

| Element | Effect |
|---|---|
| Speed FFT | Observation cadence. |
| Range FFT | Descriptor normalization. |
| Automation Threshold | Required variation. |
| Time Memory | Minimum interval between changes. |
| `foldersToScanPreset` | Available preset population. |

Automation changes the complete configuration and therefore acts on macro-form.

## 16. Presets and Serialization

Saving traverses GUI views and stores type, value, text, range, envelope, SoundFileView selection, and Slider2D position. Loading rebuilds buffers and reapplies values through `valueAction`.

| Object | Scope |
|---|---|
| Preset + Control | Main GUI and external controls. |
| Preset | Main GUI. |
| Synthesizer | Ninety-three controls for one voice. |
| Init Preset | Startup-generated state. |
| Copy Source/Target | Serialized voice copy. |

Restoration depends on fixed GUI-view order; changing GUI structure may break older presets.

## 17. OSC, MIDI, and VST

OSC provides master/slave synchronization for tempo, start, and recording, and accepts Score commands for loading a preset or starting/stopping. MIDI handles notes, velocity, pitch bend, and output. `VSTPluginController` hosts a plug-in and may receive generated MIDI notes. In master mode, Start and Stop are also sent to Ardour transport.

## 18. Layered Diagnostics

| Symptom | Likely layer |
|---|---|
| No data | Source, analysis SynthDef, or OSC. |
| Data but no band | Boundaries, filters, or expiration. |
| Bands but no voice | Mute/Solo, sequencer mode, or Step. |
| Voice but silence | Buffer, SynthDef, Amp, or routing. |
| Unstable pitch | Pitch analyzer, Ambitus, scale, or jitter. |
| Clicks | Envelope, buffer reader, or HP controls. |
| Excessive effect | Dry/Wet, feedback, or jitter. |
| Different preset result | Sample path or changed GUI structure. |

Time can be summarized as: **analyze, memorize by bands, select voices, generate parameters, synthesize, process, and spatialize**.
