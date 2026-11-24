# Density GUI: Complete Control, Range, and Behavior Reference

**Expected function of every major slider, button, menu, and XY surface**  
Version: October 5, 2026

> **Recommended first settings:** Onsets, 2–4 voices, Density 20–35%, Default algorithm, zero jitter, moderate amplitude, and a limiter with headroom.

## 1. Application Windows

| Window | Musical function |
|---|---|
| Density | Main analysis, memory, transformation, and orchestration panel. |
| Reverb | Global space and dry/wet mix. |
| Analyze Freq/Amp/Duration | Musical-event visualization. |
| Analyze Flux/Flatness/Centroid/Energy/BPM | Spectral-descriptor visualization. |
| Keyboard | Direct playing, transposition, and level. |
| VST | External instrument/effect hosting. |

The analysis windows are diagnostic displays, not direct sound generators. `Display Analyze Music` and `Display Analyze FFT` confirm that data is circulating.

## 2. AUDIO Zone and Sources

| Control | Function | Recommendation |
|---|---|---|
| Start/Stop | Starts or stops analysis and generation. | Check hardware level before Start. |
| Audio/File | Selects live input or file. | File mode supports repeatable tests. |
| Analyze Bus | Input channel used for analysis. | Choose the relevant microphone or source mix. |
| Recording Bus | Channel captured by internal samplers. | May differ from Analyze Bus. |
| Offset | Normalized file position. | Target a specific passage. |
| File dB | File gain. | Avoid compensating a quiet file with excessive gain. |
| Limiter | Final output ceiling. | Preserve safety headroom. |

## 3. Analyzer Modes

| Analyzer | Recommended source | Limitation |
|---|---|---|
| Onsets | Percussion, attacks, articulated playing | Pitch can be unstable in dense mixtures. |
| Pitch | Voice or monodic instrument | Not designed for polyphony. |
| Pitch2 | Mixed or noisy sources | More complex and potentially slower. |
| KeyTrack | Tonal context or pitch-class control | Reduces melodic detail. |
| Keyboard | Software keyboard | Depends on key layout and transposition. |
| MIDI | External controller | Verify connection and channel. |

## 4. Analysis Controls

| Control | Exact range | Function and musical consequence |
|---|---:|---|
| Threshold | 0–1, unipolar | Active-analyzer sensitivity. Low values usually retain more events and noise; high values are more selective. |
| Filter | 0–1, unipolar | Pitch confidence/stability filtering. High values stabilize pitches but may lose events. |
| Prefilter | Off / LoPass / HiPass | Inserts or removes a filter before analysis. |
| FhzPass | Analyzer-dependent frequency | Cutoff for the selected prefilter. |
| Fhz | 0–12 semitones, linear, 0.01 step | Minimum pitch difference required for a new event. |
| Amp | 0–12 dB, linear, 0.01 step | Minimum amplitude difference required for a new event. |
| Duration | 0.01–60 s, exponential | Minimum accepted time between events. |
| Max Duration | 0.01666–3600 s, exponential | Maximum continuity gap before reset/segmentation. |
| MemoryTime | 0.01666–3600 s, exponential | Silence time before memory clearing when Infinite Memory is OFF. |
| Infinite Memory | On/Off | Prevents automatic memory clearing. |

Recommended adjustment order: set Threshold and Filter with one source; add Fhz, Amp, and Duration to remove duplicates; finally choose MemoryTime for the intended formal timescale.

## 5. Memory and Frequency Bands

| Control | Role |
|---|---|
| Max Data | Events retained in each circular memory. |
| MemoryTime | Delay before clearing after silence. |
| Infinite Memory | Retains material after input stops. |
| FhzBand | Number of pitch-register bands. |
| Range Band | MIDI limits of each band. |
| All / 1…12 | Bands authorized to generate voices. |

Small memories closely follow live playing. Large memories create a more stable texture that is less tied to the immediate gesture. Changing the number of bands resets memories and voices, so do it while stopped.

## 6. Generation Controls

| Control | Exact range | Expected function |
|---|---:|---|
| Density | Percentage/control-panel range | Probability and renewal frequency of voices. At 0, no voices; near 1, maximum renewal subject to Max Synth. |
| Max Synth | Integer, panel-defined | Maximum simultaneous voices and a direct CPU ceiling. |
| Range FHZ | 0–127 MIDI, ranger, 0.1 step | Output pitch range. |
| Trans FHZ | -127 to +127 semitones, 0.1 step | Transposition after remapping. |
| Range Amp | dB specification, ranger | Dynamic range of generated voices. |
| Range Duration | 0–60 s, ranger | Minimum/maximum generated durations. |
| Trans Duration | -60 to +60 s, 0.01 step | Adds or removes time after transformation. |
| Stretch | 0.0167–60, exponential | Multiplies the temporal behavior: low contracts, high stretches. |
| Quantization | 1–100, integer | Grid resolution; practical subdivision is `1 / value`. |
| BPM System | Panel-defined tempo | Reference clock; AutoBPM may override it. |
| FX | Dry/effect blend | Per-voice wet/dry mix. |

Increase Density before Max Synth so you can hear whether a denser renewal rate is musically useful before increasing polyphony and CPU cost.

> **Implementation detail:** Density is used as creation probability, approximate interval (`1 - Density`), and reciprocal lifetime factor. Avoid zero in any forced-build path.

## 7. Envelope and Synth Macros

| Control | Function |
|---|---|
| Envelope | Eight levels and seven articulation segments. |
| Threshold/Sensitivity Synth | Generic `ctrlHP1`/`ctrlHP2` values sent to compatible SynthDefs. |
| Offset | Starting point in the sound or buffer. |

For click reduction, use non-zero attack/release times and keep the first and final envelope levels at zero.

## 8. Live Sampler and Buffer Controls

| Control | State / range | Function |
|---|---:|---|
| DurSample | 0.0001–8 s, exponential, 0.0001 step | Length of the live-captured fragment. |
| Rec / Pre | SynthDef-dependent | New recording level and retained previous-buffer level. |
| Ploop | On/Off | Playback looping. |
| Rloop | On/Off | Recording loop/retrigger behavior. |
| Reverse | On/Off | Reverse playback when the SynthDef accepts negative rate. |
| Sampler+Sound | Mode | Randomly mixes captured and library buffers. |
| Sampler | Mode | Uses live capture only. |
| Sound | Mode | Uses the library only. |
| RecSnd | On/Off | Records input into a buffer associated with a library sound. |
| Clear | Action | Clears the designated buffer or memory. |

With high Pre values, the buffer becomes cumulative memory. Test Reverse, Offset, and very short DurSample values at low Density.

## 9. Instrument, Sound, and FX XY Surfaces

Each XY surface indexes a discrete two-level list. X selects a family; Y selects an item inside that family. The displayed label is the most recently chosen resource when a voice was created.

| Element | Numeric behavior | Musical interpretation |
|---|---|---|
| Instrument X | 0–1 normalized | Selects a SynthDef family. |
| Instrument Y | 0–1 normalized | Selects a SynthDef in that family. |
| Sound X/Y | 0–1 / 0–1 | Selects a sound family and sample. |
| FX X/Y | 0–1 / 0–1 | Selects an effect family and effect. |
| JitX | 0–100% | Broadens horizontal family selection. |
| JitY | 0–100% | Broadens vertical item selection. |
| JitterCtrl | 0–1 | Adds variation to several controls/descriptors. |
| Lock/Fixed | On/Off, where available | Prevents or allows automatic resource renewal. |

These surfaces do not morph continuously between sounds. They are quantized by the actual list sizes. Set every jitter to zero when identifying zones.

## 10. Algorithms, Chords, and Tonality

| Control | Function |
|---|---|
| Algorithm | Range from which an algorithm is randomly selected. |
| Default | Direct memory remapping. |
| Probability | Statistics-guided probabilistic transformations. |
| Euclide | Combines frequency, amplitude, and duration dimensions. |
| Genetic | Evolves musical solutions. |
| Kohonen | Similarity-based organization. |
| Neural | Predicts from successive events. |
| Chord | Groups short events into chords. |
| Chord Max | Limits chord polyphony. |
| Chord Dur | Maximum time gap for simultaneous grouping. |
| Tuning/Scale | Constrains pitches to a degree structure. |
| Root | Manual root. |
| AutoRoot | Derives root from spectral energy. |
| AutoBPM | Derives tempo from analyzed BPM. |

Begin with Default only. Then include Probability. Test adaptive families one at a time.

## 11. Reverb Controls

| Control | Exact range | Function |
|---|---:|---|
| Model | Off, BathRoom, Living Room, Church, Cathedral, Canyon, FreeVerb, Allpass, JPverb | Selects or disables the global reverb engine. |
| RoomSize | 0–300 | GVerb virtual room size. |
| RevTime | 0–150 s | Reverberation decay time. |
| Damping | 0–1 | High-frequency damping. |
| InputBW | 0–1 | Input bandwidth. |
| EarlyLevel | dB specification | Early-reflection level. |
| TailLevel | dB specification | Late-tail level. |
| Dry/Wet | 0–1 | 0 = dry, 1 = fully processed. |
| Pan | -1 to +1, ranger | Reverb placement range. |

Large RoomSize plus long RevTime can accumulate substantial energy. Reduce TailLevel or Dry/Wet and do not rely on the limiter as gain control.

## 12. FFT Display and Control

| Control | Exact range | Behavior |
|---|---:|---|
| FFT Speed | -100 to +100, linear, 0.01 step | Signed speed/refresh parameter used by the FFT mechanism. |
| Range FFT | 0–1, ranger | Descriptor normalization/display window. |

Descriptors include Flux, Flatness, Centroid, Energy percentile frequency, and BPM.

## 13. MIDI, Keyboard, VST, and OSC

### MIDI and Keyboard

- MIDI IN selects the channel feeding MIDI analysis.
- MIDI Out enables generated-note duplication to an external output and optionally VST.
- The Channels field contains 13 values: one global channel and twelve band channels, shown as 1–16.
- Keyboard Shortcut mode uses `y s x d c v g b h n j m , l . é -`; `<` and `>` transpose.

### VST

| Control | Range / state | Function |
|---|---:|---|
| Run | On/Off | Enables or suspends VST processing. |
| Browse | Action | Selects a plug-in. |
| Editor / GUI | Action | Opens generic or native plug-in UI. |
| Close | Action | Closes the plug-in and frees its controller. |
| Out | Panel-defined channel | Output assignment. |
| xFade | 0–1 | Dry/plug-in blend. |
| Gain In | 0–1 | Normalized input level. |
| Pan | -1 to +1, ranger | Return pan range. |

Test MIDI and VST separately before integrating them with the generator. External OSC `/score` messages can control preset, start, and stop operations.

## 14. Main Buttons and State Changes

| Command | States / choices | Immediate effect |
|---|---|---|
| Start / Stop | Two states | Starts/stops source, analyzers, Player, and silence monitoring. |
| Audio / File | Two choices | Changes source; File enables file Offset and gain. |
| Analyzer | Onsets, Pitch, Pitch2, KeyTrack, Keyboard, MIDI | Changes event-creation method and relevant controls. |
| Prefilter | Off, LoPass, HiPass | Inserts/removes filtering before analysis. |
| Infinite Memory | On/Off | Prevents/allows clearing after MemoryTime. |
| FhzBand | On/Off | Enables selected-band generation rotation. |
| Band All / 1…12 | Individual On/Off | Authorizes memories to produce voices. |
| Ploop / Rloop / Reverse | Playback/recording states | Changes looping, retriggering, and direction. |
| Sampler+Sound / Sampler / Sound | Three choices | Mixed, live-only, or library-only buffers. |
| RecSnd | On/Off | Captures input into an associated sound buffer. |
| AutoRoot / AutoBPM / Chord | On/Off | Automates root/tempo or chord building. |
| Scale/Tuning | Menu | Changes degree table and tuning. |
| Reverb | Off + models | Replaces or disables reverb. |
| Preset Load/Save | File action | Restores or serializes GUI, orchestras, and memories. |

When the label remains unchanged in both states, green normally means active and red inactive.

## 15. Presets, Shortcuts, and Recording

| Action | Command |
|---|---|
| Start/Stop | Space or Escape. |
| Next/previous analyzer | `q` / `Ctrl+q`. |
| Audio/File | `h`. |
| End voices quickly | `i`. |
| Reset voices and memories | `Ctrl+i`. |
| Full reset and Init Preset | `Alt+i`. |
| Next/previous window | `w` / `Ctrl+w`. |
| Load/save preset | `l` or `s`, then a digit. |
| Random preset | `z`. |
| Load analysis file | `Ctrl+f`. |
| Record/pause/stop | `Alt+r` / `Shift+Alt+r` / `Ctrl+Alt+r`. |

Before performance, prepare a rescue preset, verify sound paths and audio devices, limit Max Synth, and record a short rehearsal test.

## 16. Practical Composition Starting Points

| Strategy | Starting values |
|---|---|
| Reactive texture | Onsets; short memory; 2–4 voices; Default; low jitter; room reverb. |
| Spectral cloud | Pitch2; Infinite Memory; 5–8 voices; bands; long duration; spacious reverb. |
| MIDI/VST counterpoint | MIDI; Chord; Scale/Root; Quantization 4–16; per-band channels; VST ON. |
| Cautious concert setup | Max Synth 2–4; Density 20–40%; JitX/JitY 0–15%; JitterCtrl 0–0.15; limited Range Amp; moderate reverb. |

## 17. Troubleshooting and Clean Shutdown

| Symptom | Action |
|---|---|
| No data | Check source, bus, analyzer, Threshold, and filters. |
| Data but no voice | Check Density, Max Synth, selected bands, and Start. |
| Too many voices | Lower Density or Max Synth. |
| Unstable pitch | Try Pitch2/KeyTrack or prefilter. |
| Saturation | Reduce Range Amp, Max Synth, FX, and reverb. |
| Clicks | Lengthen envelope and inspect buffer/Reverse behavior. |
| Silent MIDI | Verify channel, output, and MIDI mode. |
| Incomplete preset | Verify sound paths and external resources. |

Clean shutdown: stop recording, disable automation/jitter if needed, press Stop, wait for voice release, close VST and secondary windows, then close the main window and quit SuperCollider.
