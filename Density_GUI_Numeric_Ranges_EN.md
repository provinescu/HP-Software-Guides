# Density GUI: Complete Numeric Range Reference

**Sliders, buttons, menus, XY surfaces, and expected behavior**  
Version: October 5, 2026

> Recommended first setup: Onsets, 2–4 voices, Density 20–35%, Default algorithm, zero jitter, conservative output level.

## 1. Signal Chain

```text
listen -> analyze -> remember -> transform -> orchestrate -> spatialize/output
```

| Stage | GUI controls |
|---|---|
| Listen | Audio, File, Keyboard, MIDI. |
| Analyze | Onsets, Pitch, Pitch2, KeyTrack. |
| Remember | Max Data, MemoryTime, Infinite Memory, bands. |
| Transform | Register, duration, level, scale, algorithm. |
| Orchestrate | Instrument, Sound, and FX XY surfaces. |
| Output | Pan, reverb, channels, VST, limiter. |

## 2. AUDIO and Sources

| Control | Function | Recommendation |
|---|---|---|
| Start/Stop | Starts/stops analysis and generation. | Check hardware level first. |
| Audio/File | Selects live or file source. | File mode supports repeatable tests. |
| Analyze Bus | Input channel used for analysis. | Choose the relevant mic/mix. |
| Recording Bus | Channel recorded by samplers. | May receive another source. |
| Offset | File position. | Target a useful passage. |
| File dB | File level. | Avoid excessive compensation gain. |
| Limiter | Final ceiling. | Preserve safety headroom. |

## 3. Analysis and Sensitivity

| Analyzer | Recommended use | Limitation |
|---|---|---|
| Onsets | Percussion and articulated attacks | Pitch may be unstable in dense mixtures. |
| Pitch | Monophonic voice/instrument | Poorer for polyphony. |
| Pitch2 | Mixed/noisy sources | More complex and potentially slower. |
| KeyTrack | Tonal context or pitch class | Reduces melodic detail. |
| Keyboard | Software keyboard | Depends on key layout/transposition. |
| MIDI | External controller | Verify connection and channel. |

| Control | Exact range | Behavior |
|---|---:|---|
| Threshold | 0–1, unipolar | Detection sensitivity; lower commonly retains more events. |
| Filter | 0–1, unipolar | Analyzer-specific pitch confidence/filter. |
| Fhz | 0–12 semitones, lin, step 0.01 | Minimum stored pitch difference. |
| Amp | 0–12 dB, lin, step 0.01 | Minimum stored level difference. |
| Duree | 0.01–60 s, exp | Minimum accepted interval. |
| Max Duree | 0.01666–3600 s, exp | Maximum gap before continuity reset. |
| MemoryTime | 0.01666–3600 s, exp | Silence before clearing when Infinite Memory is off. |

## 4. Memory and Bands

| Control | Role |
|---|---|
| Max Data | Events retained in circular memory. |
| MemoryTime | Delay before clearing after silence. |
| Infinite Memory | Keeps material when input stops. |
| FhzBand | Number of register bands. |
| Range Band | MIDI limits for each band. |
| All / 1–12 | Bands allowed to generate voices. |

A small memory closely follows live input. A large memory produces a more stable texture. Changing band count resets memories and voices.

## 5. Density and Musical Generation

| Control | Exact range | Result |
|---|---:|---|
| Density | GUI percentage/control range | Voice-creation probability and renewal rate. |
| Max Synth | Integer, panel-defined | Maximum simultaneous voices and CPU ceiling. |
| Range FHZ | 0–127 MIDI, ranger, step 0.1 | Output register. |
| Trans FHZ | -127 to +127 semitones, step 0.1 | Added transposition. |
| Range Amp | dB spec, ranger | Generated dynamic range. |
| Range Duree | 0–60 s, ranger | Generated-duration bounds. |
| Trans Duree | -60 to +60 s, step 0.01 | Duration offset. |
| Stretch | 0.0167–60, exp | Pattern time-scale factor. |
| Quantization | 1–100, integer | Grid resolution; practical subdivision = `1/value`. |
| BPM System | Panel-defined | Reference tempo. |
| FX | Dry/effect blend | Per-voice mix. |

Raise Density before Max Synth so you can evaluate musical density before increasing CPU and output summation.

## 6. Envelope, Buffers, and Sampler

| Control | Range / behavior |
|---|---|
| Envelope | Eight levels and seven segments. |
| Threshold/Sensitivity Synth | Two macros sent to compatible SynthDefs. |
| Offset | Normalized read point. |
| DurSample | 0.0001–8 s, exp, step 0.0001. |
| Rec / Pre | New signal and retained old content. |
| Ploop | Playback loop. |
| Rloop | Recording loop/retrigger behavior. |
| Reverse | Forward/reverse playback. |
| Sampler+Sound | Random live/library selection. |
| Sampler | Live capture only. |
| Sound | Library only. |
| RecSnd | Records input into a copy of a sound buffer. |

High Pre turns the buffer into cumulative memory. Test Reverse and Offset at low Density. Keep envelope endpoints at zero and use non-zero attack/release.

## 7. Instrument, Sound, and FX XY Surfaces

| Element | Numeric behavior | Interpretation |
|---|---|---|
| Instrument X / Y | 0–1 / 0–1 | SynthDef family and item. |
| Sound X / Y | 0–1 / 0–1 | Sound family and sample. |
| FX X / Y | 0–1 / 0–1 | Effect family and effect. |
| JitX / JitY | 0–100% | Horizontal/vertical selection dispersion. |
| JitterCtrl | 0–1 | Global perturbation of selected controls/descriptors. |

The surfaces are discretized by actual list size. They do not continuously morph between sounds. Set all jitter to zero when identifying families.

## 8. Algorithms, Chords, and Tonality

| Control / algorithm | Function |
|---|---|
| Algorithm | Range from which one algorithm is drawn. |
| Default | Direct memory remapping. |
| Probability | Statistics-guided probabilistic transformations. |
| Euclide | Combines frequency, amplitude, and duration. |
| Genetic | Evolves musical solutions. |
| Kohonen | Similarity-based organization. |
| Neural | Prediction from successive events. |
| Chord | Groups short events into chords. |
| Tuning/Scale | Constrains pitches to degree structure. |
| Root | Root transposition. |
| AutoRoot | Derives root from spectral energy. |
| AutoBPM | Adapts tempo from analyzed BPM. |

Begin with Default, then Probability, then adaptive families one at a time.

## 9. Reverb, FFT, and VST Exact Ranges

| Control | Exact range | Behavior |
|---|---:|---|
| FFT Speed | -100 to +100, lin, step 0.01 | Signed speed/refresh parameter. |
| Range FFT | 0–1, ranger | Descriptor normalization/display window. |
| RoomSize | 0–300 | GVerb room size. |
| RevTime | 0–150 s | Reverberation time. |
| Damping | 0–1 | High-frequency damping. |
| InputBW | 0–1 | Reverb input bandwidth. |
| EarlyLevel | dB spec | Early-reflection level. |
| TailLevel | dB spec | Late-tail level. |
| Dry/Wet | 0–1 | 0 dry, 1 processed. |
| Limiter | dB spec | Output ceiling. |
| File Offset | 0–1 | Normalized file position. |
| File dB | dB spec | File gain. |
| VST xFade | 0–1 | Direct/processed blend. |
| VST Gain In | 0–1 | Normalized input gain. |
| VST Pan | -1 to +1, ranger | Pan range. |
| Keyboard Volume | dB spec | Software-keyboard level. |

Large RoomSize and RevTime can accumulate substantial energy. Reduce TailLevel or Dry/Wet and use proper gain staging.

## 10. Buttons, Menus, and State Changes

| Command | Choices | Immediate effect |
|---|---|---|
| Start / Stop | Two states | Starts/stops source, analyzers, Player, silence monitor. |
| Audio / File | Two choices | Changes source; File enables Offset/gain. |
| Analyzer | Onsets, Pitch, Pitch2, KeyTrack, Keyboard, MIDI | Changes detection method and relevant controls. |
| Prefilter | Off, LoPass, HiPass | Inserts/removes pre-analysis filter. |
| Infinite Memory | On/Off | Prevents/allows clearing after MemoryTime. |
| FhzBand | On/Off | Enables selected-band generation rotation. |
| Band All / 1–12 | Individual On/Off | Selects memories allowed to create voices. |
| Ploop / Rloop / Reverse | Playback/recording states | Changes looping, retriggering, and direction. |
| Sampler+Sound / Sampler / Sound | Three choices | Mixed, live-only, or library-only buffers. |
| RecSnd | On/Off | Captures input into associated sound buffer. |
| AutoRoot / AutoBPM / Chord | On/Off | Automates root/tempo or chord construction. |
| Scale/Tuning | Menu | Replaces degree/tuning table. |
| Reverb | Off + models | Replaces/disables reverb engine. |
| VST actions | Run/Browse/Editor/GUI/Close | Enables, selects, opens, or closes plug-in. |
| Preset Load/Save | File action | Restores/serializes GUI, orchestras, and memories. |

## 11. Performance Starting Points

| Strategy | Starting values |
|---|---|
| Reactive texture | Onsets; short memory; 2–4 voices; Default; low jitter; room reverb. |
| Spectral cloud | Pitch2; Infinite Memory; 5–8 voices; bands; long durations; large reverb. |
| MIDI/VST counterpoint | MIDI; Chord; Scale/Root; Quantization 4–16; per-band channels; VST. |
| Cautious concert | Max Synth 2–4; Density 20–40%; JitX/Y 0–15%; JitterCtrl 0–0.15; moderate reverb. |

## 12. Troubleshooting

| Symptom | Action |
|---|---|
| No data | Check source, bus, analyzer, Threshold, filters. |
| Data but no voice | Check Density, Max Synth, bands, Start. |
| Too many voices | Lower Density or Max Synth. |
| Unstable pitch | Try Pitch2/KeyTrack or prefilter. |
| Saturation | Lower Range Amp, Max Synth, FX, reverb. |
| Clicks | Lengthen envelope; inspect buffer/Reverse. |
| Silent MIDI | Check channel, output, and mode. |
| Incomplete preset | Verify sound paths and external resources. |

## 13. Exact Slider and Knob Reference

| Control | Exact range | Scale / step |
|---|---:|---|
| Threshold | 0–1 | unipolar |
| Filter | 0–1 | unipolar |
| Fhz | 0–12 | lin, 0.01 |
| Amp | 0–12 | lin, 0.01 |
| Duree | 0.01–60 s | exp |
| Max Duree | 0.01666–3600 s | exp |
| MemoryTime | 0.01666–3600 s | exp |
| Range FHZ | 0–127 MIDI | lin, 0.1 |
| Trans FHZ | -127 to +127 | lin, 0.1 |
| Range Amp | dB spec | logarithmic |
| Range Duree | 0–60 s | lin |
| Trans Duree | -60 to +60 s | lin, 0.01 |
| Stretch | 0.0167–60 | exp |
| Quantization | 1–100 | lin, integer |
| DurSample | 0.0001–8 s | exp, 0.0001 |
| JitX / JitY | 0–100% | lin |
| JitterCtrl | 0–1 | unipolar |

SuperCollider symbolic specs: `db` is logarithmic amplitude; `unipolar` is 0–1; `bipolar` is -1 to +1. Exact visible dB limits may depend on the installed SuperCollider version.
