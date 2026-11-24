# Robot GUI: Complete Numeric Range and Behavior Reference

**Expected function of each control**  
Version: October 5, 2026

## 1. System Overview

Robot is a multi-instrument real-time environment. Every instrument has a source, memory, musical transformations, synthesizer, pre/post effects, and an algorithmic mode. Global transport aligns the system to one clock.

| Layer | Function |
|---|---|
| Source | Live audio, file, keyboard, or MIDI. |
| Analysis | Onsets, Pitch, Pitch2, or KeyTrack. |
| Memory | Frequency, amplitude, and duration. |
| Algorithm | Chaos, Neurone, or Genetic transforms data. |
| Instrument | SynthDef, sample, envelope, quantization, scale. |
| Routing | Pre-FX, post-FX, pan, audio/MIDI/VST outputs. |
| Synchronization | Tempo, beats per bar, sequences, scores. |

Safe order: **source -> analysis -> memory -> algorithm -> synthesis -> effects -> MasterFX**.

## 2. Reading Widgets

| Widget / scale | Meaning |
|---|---|
| EZSlider / EZKnob | One value. |
| EZRanger | Two minimum/maximum bounds. |
| `lin` | Uniform distribution. |
| `exp` | Increased precision at small values. |
| `db` | Logarithmic amplitude scale. |
| `unipolar` | 0 to 1. |
| `bipolar` | -1 to +1. |
| step 1 | Integer value. |
| -∞ dB | Digital silence/zero gain. |

## 3. Master and VST

| Control | Range | Behavior |
|---|---:|---|
| LimitOut | Standard dB ControlSpec | Final limiter threshold. |
| PostAmp | Standard dB ControlSpec | Global gain after mixing. |
| Volume | Standard dB ControlSpec | Keyboard/module level. |
| VST xFade | 0–1 | Direct/processed blend. |
| VST Gain In | 0–1 | Normalized input gain. |
| VST Pan | -1 to +1, ranger | Pan bounds. |
| Run | On/Off | Enables VST processing. |
| Browse | Action | Selects a plug-in. |
| Editor / GUI | Action | Opens plug-in interface. |
| Close | Action | Closes and releases the controller. |

The limiter protects output but does not correct excessive gain accumulation in instruments, sends, or VST.

## 4. Analysis and Memory

| Control | Exact range | Expected effect |
|---|---:|---|
| Thresh Analyse | 0.01–1, linear | Detection sensitivity; interpretation depends on analyzer. |
| Filter Analyse | 0.01–1, linear | Analyzer confidence/stability filter. |
| Dist Freq | 0–12 semitones, linear | Minimum pitch difference for a new datum. |
| Dist Amp | 0–60, linear | Minimum amplitude difference. |
| Dist Dur | 0.01–16 s, exponential | Minimum duration/time difference. |
| Memory Time | 0.01–3600 s, exponential | Retention time before clearing/renewal. |
| Max Dur | 0.01–60 s, exponential | Maximum admitted duration/continuity. |
| MaxDatas | 1–256, integer | Maximum remembered events. |
| Chord | 0–12, integer | Maximum harmonic-group size. |
| ChordDur | 0.01–1 s, exponential | Chord-grouping time window. |

Set Thresh and Filter first. Then adjust Dist Freq/Amp/Dur to remove duplicates. Finally choose Memory Time and MaxDatas for the intended musical form.

## 5. Chaos Algorithm

| Control | Exact range | Musical behavior |
|---|---:|---|
| Chaos Fhz | 2.96–4, exponential | Chaotic regime applied to pitch; higher values tend toward instability. |
| Chaos Amp | 1–4, exponential | Chaotic amplitude regime. |
| Chaos Dur | 3–4, exponential | Chaotic duration regime. |
| ChaosIn Speed | 1–100, integer | Data-feed/update cadence. |
| Algorithm Speed | 1–100, integer | Global calculation/renewal cadence. |

These values are not percentages. Start near the bottom of each range and change one axis at a time.

## 6. Neurone Algorithm

| Control | Range | Behavior |
|---|---:|---|
| Neuron Learning | 0–1, linear | Learning rate: low = slow adaptation; high = fast adaptation. |
| Neuron Temperature | 0.1–1, linear | Choice diversity: low = deterministic; high = exploratory. |
| NeuroneIn Speed | 1–100, integer | Model-input cadence. |
| Algorithm Speed | 1–100 | Output/update cadence. |

Suggested starting region: Learning 0.05–0.25, Temperature 0.1–0.35, speed 5–20.

## 7. Genetic Algorithm

| Control | Range | Behavior |
|---|---:|---|
| Genetic Crossing | 0–1, linear | Crossover rate among solutions/genomes. |
| Genetic Mutation | 0–1, linear | Random variation rate. |
| GenetiqueIn Speed | 1–100, integer | Input-feed cadence. |
| Algorithm Speed | 1–100 | Evolution cadence. |

| Goal | Crossing | Mutation |
|---|---:|---:|
| Stable | 0.1–0.3 | 0–0.05 |
| Evolving | 0.4–0.7 | 0.05–0.2 |
| Exploratory | 0.7–1 | 0.2–0.6 |

High Mutation plus high speed quickly reduces reproducibility. Save a preset first.

## 8. Instrument: Pitch, Level, Duration, Chords, and Scale

| Control | Exact range | Function |
|---|---:|---|
| Amp | dB ControlSpec, ranger | Instrument dynamic bounds. |
| Dur | 0–60 s, ranger | Duration bounds. |
| Quanta | 0.01–100, exponential | Time-quantization value/division. |
| Root | 0–21, integer | Root for 12-TET and extended Indian tunings. |
| Scale | Dynamic menu | No Scale, chromatic, modes, pentatonics, ragas, shrutis. |
| Bands 1–12 | Individual On/Off | Enables/disables each frequency band. |
| All Bands | On/Off | Enables all bands. |
| Chord | 0–12 | Maximum grouped notes. |
| ChordDur | 0.01–1 s | Time interval treated as simultaneity. |

No Scale leaves pitches unconstrained. For stable harmony, set Root and Scale before increasing Chord.

## 9. File, Offset, Tempo, and Bar

| Control | Range | Behavior |
|---|---:|---|
| File LevelOut | -∞ to +6 dB | Source-file level. |
| File Offset | 0–1 | Normalized file position. |
| OffSet | 0–1 | Normalized position in a buffer/sample. |
| Tempo | 1–960 BPM, exponential | Global tempo. |
| Beats / Bar | 1–64, exponential | Beats in a bar/cycle. |
| Automation Speed | 1–100, integer | Automation cadence. |

The clearest conventional tempo range is roughly 30–240 BPM. Extreme values are useful for texture and modulation.

## 10. Instrument, Sample, and Envelope

| Command | State / range | Intended function |
|---|---|---|
| OffSet | 0–1 | Playback start between buffer beginning/end. |
| Loop | On/Off | Repeats the buffer when supported. |
| Reverse | On/Off or signed speed | Reverses playback when negative rate is supported. |
| Envelope | Dynamic curve/parameters | Attack, sustain, and release. |
| Synth menu | Dynamic list | Selects synthesis/sample engine. |
| Sound menu | Dynamic list | Selects available sound resource. |
| Record buffer | On/Off/action | Captures input into an instrument buffer. |
| Clear / Reset | Action | Clears buffer or module state. |

With changing offsets, short loops, or reverse playback, use non-zero attack/release to reduce clicks.

## 11. Pre/Post Effects, Pan, and Gain

| Control | Range | Function |
|---|---:|---|
| FXpre | -∞ to +12 dB | Level of treatment before the main stage. |
| FXpost | -∞ to +12 dB | Level of treatment after the main stage. |
| Amp_pre | -∞ to +12 dB | Pre-chain gain. |
| Amp_post | -∞ to +12 dB | Post-chain gain. |
| Pan_pre | -1 to +1 | Stereo position before processing. |
| Pan_post | -1 to +1 | Stereo position after processing. |
| Bypass | On/Off | Bypasses a section without necessarily deleting nodes. |
| Prefilter | Off / LoPass / HiPass | Filter before analysis/processing. |

Compare pre and post at equal perceived level. Multiple instruments and FX sum even when every fader is below 0 dB.

## 12. Routing, MIDI, and VST

| Element | Range / choice | Behavior |
|---|---|---|
| AudioIn Rec | 1–32 | Recording/analysis input channel. |
| Audio Out | Dynamic menu | Physical/logical instrument output. |
| MIDI In / Out | Port and channel | Input or generated-note output. |
| VST Out | Available outputs | Plug-in output channel. |
| Instrument routing | Menus/buttons | Source, synth, FX, and output assignment. |
| Mute / Solo | On/Off | Silences or isolates an instrument. |

Validate direct sound before enabling VST. Begin with low Gain In and avoid return loops from external MIDI instruments.

## 13. Main Buttons and Menus

| Command | State / choice | Consequence |
|---|---|---|
| Start / Stop | On/Off | Starts/stops transport, analysis, and generation. |
| Analyzer | Onsets/Pitch/Pitch2/KeyTrack | Changes extraction method. |
| Source | Audio/File/Keyboard/MIDI | Changes data origin. |
| Algorithm | Default/Chaos/Neurone/Genetic… | Changes transformation logic. |
| Bands All / 1–12 | Green/red | Enables or disables each band. |
| Scale On/Off | On/Off | Constrains or frees pitch. |
| Bypass | On/Off | Bypasses the designated section. |
| VST Run | On/Off | Enables the plug-in. |
| Preset/Sequence/Score | Load/Save/Play/Record | Restores, organizes, or records form. |

When text remains unchanged, green normally means active and red inactive.

## 14. Speed and Automation

| Control | Range | Interpretation |
|---|---:|---|
| ChaosIn Speed | 1–100 | Chaos acquisition cadence. |
| NeuroneIn Speed | 1–100 | Neurone acquisition cadence. |
| GenetiqueIn Speed | 1–100 | Genetic acquisition cadence. |
| Algorithm Speed | 1–100 | Decision cadence. |
| Automation Speed | 1–100 | Automated-change cadence. |

- 1–10: slow and readable evolution.
- 10–40: active musical animation.
- 40–100: fast texture, higher CPU demand, reduced predictability.

For performance, begin between 1 and 10. Do not automate gain, feedback, routing, and algorithmic parameters simultaneously.

## 15. Recommended Starting Configurations

| Goal | Starting values |
|---|---|
| Articulated response | Onsets; Dist Dur 0.05–0.2; Dur 0.1–1.5; Quanta 4–16. |
| Slow texture | Pitch2; Memory 30–300 s; Dur 3–20 s; Tempo 20–60. |
| Controlled Chaos | Fhz 3.0–3.3; Amp 1–1.8; Dur 3.0–3.3; speed 5–20. |
| Stable Neurone | Learning 0.1–0.25; Temperature 0.15–0.35; speed 5–20. |
| Evolving Genetic | Crossing 0.4–0.7; Mutation 0.05–0.15; speed 5–25. |
| Harmonic counterpoint | Chord 2–5; ChordDur 0.03–0.2; Scale On; separated bands. |

## 16. Troubleshooting and Safety

| Symptom | Check |
|---|---|
| No sound | Transport, instrument Run, Amp, output, Mute/Solo, MasterFX. |
| No analysis | Source, channel, threshold, filter, prefilter. |
| Too many events | Raise Threshold and Dist Freq/Amp/Dur. |
| Empty memory | MaxDatas, Memory Time, analyzer. |
| Saturation | Amp, FXpre/post, Amp_pre/post, PostAmp, VST Gain. |
| High CPU | Instrument count, speeds, effects, VST, automation. |
| Unstable algorithm | Lower speed, Chaos, Temperature, or Mutation. |
| Incomplete preset | Verify sounds, SynthDefs, VST, and external paths. |

## 17. Declared GUI ControlSpecs

| GUI label | Exact specification | Widget |
|---|---|---|
| xFade | `unipolar` | EZKnob |
| Gain In | `unipolar` | EZKnob |
| Pan | `bipolar` | EZRanger |
| Volume | `db` | EZKnob |
| LimitOut | `db` | EZSlider |
| PostAmp | `db` | EZSlider |
| Chaos Fhz | `2.96, 4, exp` | EZSlider |
| Chaos Amp | `1, 4, exp` | EZSlider |
| Chaos Dur | `3, 4, exp, 0` | EZSlider |
| Dist Freq | `0.0, 12.0, lin, 0.0` | EZSlider |
| Dist Amp | `0.0, 60.0, lin, 0.0` | EZSlider |
| Dist Dur | `0.01, 16, exp, 0` | EZSlider |
| Memory Time | `0.01, 3600, exp` | EZSlider |
| Max Dur | `0.01, 60, exp` | EZSlider |
| Chord | `0, 12, lin, 1` | EZSlider |
| ChordDur | `0.01, 1.0, exp` | EZSlider |
| MaxDatas | `1, 256, lin, 1` | EZSlider |
| Neuron Learning | `0, 1, lin, 0` | EZSlider |
| Neuron Temperature | `0.1, 1, lin, 0` | EZSlider |
| Genetic Crossing | `0, 1, lin, 0` | EZSlider |
| Genetic Mutation | `0, 1, lin, 0` | EZSlider |
| OffSet | `0, 1, lin, 0` | EZSlider |
| Pan_pre / Pan_post | `bipolar` | EZSlider |
| Amp_pre / Amp_post | `-inf, 12, db` | EZSlider |
| Amp | `db` | EZRanger |
| FXpre / FXpost | `-inf, 12, db` | EZSlider |
| Dur | `0, 60, lin, 0` | EZRanger |
| Quanta | `0.01, 100, exp, 0` | EZSlider |
| Root | `0, 21, lin, 1` | EZKnob |
| File LevelOut | `-inf, 6, db, 0` | EZSlider |
| File Offset | `0, 1, lin, 0` | EZSlider |
| Thresh Analyse | `0.01, 1, lin` | EZSlider |
| Filter Analyse | `0.01, 1, lin` | EZSlider |
| Tempo | `1, 960, exp, 0` | EZSlider |
| Beats / Bar | `1, 64, exp, 0` | EZSlider |
| ChaosIn / NeuroneIn / GenetiqueIn Speed | `1, 100, lin, 1` | EZSlider |
| Algorithm Speed | `1, 100, lin, 1` | EZSlider |
| Automation Speed | `1, 100, lin, 1` | EZSlider |

SynthDef, sound, effect, output, MIDI-port, and plug-in lists are dynamic and depend on startup resources.
