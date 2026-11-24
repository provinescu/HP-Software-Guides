# Robot GUI: Numeric Ranges, Algorithms, and Musical Behavior

**Analysis date:** October 5, 2026  
**Source basis:** `Robot - copie.txt`

This guide enables musicians and composers to configure Robot, understand the control families, build an instrument, operate the Chaos, Neurone, and Genetic algorithms, route effects, and prepare a reliable performance.

## 1. System Overview

Robot is a multi-instrument real-time creation environment. Every instrument has an input, memory, musical transformations, synthesizer, pre/post effects, and an algorithmic mode. Global transport aligns the system to a shared clock.

| Layer | Function |
|---|---|
| Source | Live audio, file, keyboard, or MIDI. |
| Analysis | Onsets, Pitch, Pitch2, or KeyTrack. |
| Memory | Stores frequency, amplitude, and duration. |
| Algorithm | Chaos, Neurone, or Genetic transforms data. |
| Instrument | SynthDef, sample, envelope, quantization, and scale. |
| Routing | Pre-FX, post-FX, pan, audio/MIDI/VST outputs. |
| Synchronization | Tempo, beats per bar, sequences, and scores. |

Safe diagnostic order: **source -> analysis -> memory -> algorithm -> synthesis -> effects -> MasterFX**.

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

## 3. Master, Level, and VST

| Control | Range | Behavior |
|---|---:|---|
| LimitOut | dB ControlSpec | Final limiter threshold. |
| PostAmp | dB ControlSpec | Global gain after mixing. |
| Volume | dB ControlSpec | Keyboard/module level. |
| VST xFade | 0–1 | Direct/processed blend. |
| VST Gain In | 0–1 | Normalized input gain. |
| VST Pan | -1 to +1, ranger | Pan bounds. |
| Run | On/Off | Enables VST processing. |
| Browse | Action | Selects a plug-in. |
| Editor / GUI | Action | Opens plug-in interface. |
| Close | Action | Closes and releases controller. |

The limiter protects output, but does not correct excessive accumulation inside instruments, FX sends, or VST.

## 4. Analysis and Memory

| Control | Exact range | Effect |
|---|---:|---|
| Thresh Analyse | 0.01–1, lin | Detection sensitivity. |
| Filter Analyse | 0.01–1, lin | Analyzer-specific confidence/filtering. |
| Dist Freq | 0–12 semitones, lin | Minimum pitch difference. |
| Dist Amp | 0–60, lin | Minimum amplitude difference. |
| Dist Dur | 0.01–16 s, exp | Minimum duration difference. |
| Memory Time | 0.01–3600 s, exp | Time before memory clearing/renewal. |
| Max Dur | 0.01–60 s, exp | Maximum accepted duration/continuity. |
| MaxDatas | 1–256, integer | Maximum stored events. |
| Chord | 0–12, integer | Maximum harmonic-group size. |
| ChordDur | 0.01–1 s, exp | Chord-grouping time threshold. |

Small distances retain detailed, reactive events. Large distances retain only major changes. Long Memory Time stabilizes material; short memory follows the present gesture.

## 5. Chaos Algorithm

| Control | Exact range | Musical behavior |
|---|---:|---|
| Chaos Fhz | 2.96–4, exp | Chaotic pitch regime; upper region is less stable. |
| Chaos Amp | 1–4, exp | Chaotic amplitude regime. |
| Chaos Dur | 3–4, exp | Chaotic duration regime. |
| ChaosIn Speed | 1–100, integer | Input/update cadence. |
| Algorithm Speed | 1–100, integer | Calculation/renewal cadence. |

Begin near the bottom of every range and increase one axis at a time.

## 6. Neurone Algorithm

| Control | Range | Behavior |
|---|---:|---|
| Neuron Learning | 0–1, lin | Learning rate: low is gradual, high is rapid. |
| Neuron Temperature | 0.1–1, lin | Selection diversity: low is deterministic, high exploratory. |
| NeuroneIn Speed | 1–100, integer | Model-input cadence. |
| Algorithm Speed | 1–100 | Output/update cadence. |

Recommended starting region: Learning 0.05–0.25, Temperature 0.1–0.3, Speed 5–20.

## 7. Genetic Algorithm

| Control | Range | Behavior |
|---|---:|---|
| Genetic Crossing | 0–1, lin | Crossover rate among solutions. |
| Genetic Mutation | 0–1, lin | Mutation rate. |
| GenetiqueIn Speed | 1–100, integer | Data-input cadence. |
| Algorithm Speed | 1–100 | Evolution cadence. |

| Goal | Crossing | Mutation |
|---|---:|---:|
| Stable | 0.1–0.3 | 0–0.05 |
| Evolving | 0.4–0.7 | 0.05–0.2 |
| Exploratory | 0.7–1 | 0.2–0.6 |

High Mutation plus high speed quickly reduces reproducibility. Save a preset first.

## 8. Instrument Pitch, Level, Duration, Chords, and Scale

| Control | Exact range | Musical effect |
|---|---:|---|
| Amp | dB ControlSpec, ranger | Instrument dynamic bounds. |
| Dur | 0–60 s, ranger | Duration bounds. |
| Quanta | 0.01–100, exp | Quantization/time division. |
| Root | 0–21, integer | Root for 12-TET and extended Indian tunings. |
| Scale | Dynamic menu | No Scale, chromatic, modes, pentatonics, ragas, shrutis. |
| Bands 1–12 | Individual On/Off | Enables/disables each frequency band. |
| All Bands | On/Off | Enables every band. |
| Chord | 0–12 | Maximum grouped notes. |
| ChordDur | 0.01–1 s | Simultaneity/grouping window. |

With Rangers, Robot selects/remaps inside the interval. Narrow ranges yield precise behavior; wide ranges increase variety.

## 9. File, Offset, Tempo, and Bar

| Control | Range | Behavior |
|---|---:|---|
| File LevelOut | -∞ to +6 dB | Source-file level. |
| File Offset | 0–1 | Normalized file position. |
| OffSet | 0–1 | Normalized buffer/sample position. |
| Tempo | 1–960 BPM, exp | Global tempo. |
| Beats / Bar | 1–64, exp | Beats in a measure/cycle. |
| Automation Speed | 1–100, integer | Automation cadence. |

For a readable pulse, begin around 30–240 BPM. Extreme values are better suited to textures and modulation.

## 10. Pre/Post Effects, Pan, and Gain

| Control | Range | Behavior |
|---|---:|---|
| FXpre | -∞ to +12 dB | Effect level before the main stage. |
| FXpost | -∞ to +12 dB | Effect level after the main stage. |
| Amp_pre | -∞ to +12 dB | Pre-chain gain. |
| Amp_post | -∞ to +12 dB | Post-chain gain. |
| Pan_pre | -1 to +1 | Pan before processing. |
| Pan_post | -1 to +1 | Pan after processing. |
| Bypass | On/Off | Bypasses a section without necessarily deleting the node. |
| Prefilter | Off / LoPass / HiPass | Filter before analysis/processing. |

Pre-FX affects what enters synthesis or analysis. Post-FX transforms the produced result. Compare at equal perceived level.

## 11. Sources, Outputs, and Routing

| Element | Choice / range | Behavior |
|---|---|---|
| AudioIn Rec | 1–32 | Recording/analysis input channel. |
| Audio Out | Dynamic menu | Physical output. |
| MIDI In / Out | Port and channel | Input or generated-note output. |
| VST Out | Available outputs | Plug-in output. |
| Instrument routing | Menus/buttons | Source, synth, FX, output assignment. |
| Mute / Solo | On/Off | Silences or isolates an instrument. |

Validate direct audio before enabling VST, and avoid external MIDI return loops.

## 12. Main Buttons and Menus

| Command | State / choice | Consequence |
|---|---|---|
| Start/Stop | Two states | Starts/stops transport and processes. |
| Analyze | Onsets/Pitch/Pitch2/KeyTrack | Changes extraction method. |
| Source | Audio/File/Keyboard/MIDI | Changes data origin. |
| Algorithm | Default/Chaos/Neurone/Genetic | Changes transformation. |
| Bands 1–12 | Green/red | Enables/disables each band. |
| Scale On/Off | On/Off | Constrains or frees pitch. |
| Bypass | On/Off | Bypasses a section. |
| VST Run | On/Off | Enables plug-in. |
| Preset/Sequence/Score | Load/Save/Play/Record | Restores, organizes, or records form. |

## 13. Speeds and Automation

| Control | Range | Interpretation |
|---|---:|---|
| ChaosIn Speed | 1–100 | Chaos acquisition cadence. |
| NeuroneIn Speed | 1–100 | Neurone acquisition cadence. |
| GenetiqueIn Speed | 1–100 | Genetic acquisition cadence. |
| Algorithm Speed | 1–100 | Decision cadence. |
| Automation Speed | 1–100 | Automated-change cadence. |

- 1–10: slow, readable evolution.
- 10–40: active musical animation.
- 40–100: fast texture, higher CPU demand, lower predictability.

## 14. Recommended Starting Configurations

| Goal | Suggested values |
|---|---|
| Articulated response | Onsets; Dist Dur 0.05–0.2; Dur 0.1–1.5; Quanta 4–16. |
| Slow texture | Pitch2; Memory 30–300 s; Dur 3–20 s; Tempo 20–60. |
| Controlled Chaos | Fhz 3.0–3.3; Amp 1–1.8; Dur 3.0–3.3; Speed 5–20. |
| Stable Neurone | Learning 0.1–0.25; Temperature 0.15–0.35; Speed 5–20. |
| Evolving Genetic | Crossing 0.4–0.7; Mutation 0.05–0.15; Speed 5–25. |
| Harmonic counterpoint | Chord 2–5; ChordDur 0.03–0.2; Scale On; separated bands. |

## 15. Troubleshooting and Safety

| Symptom | Check |
|---|---|
| No sound | Transport, instrument Run, Amp, output, Mute/Solo, MasterFX. |
| No analysis | Source, channel, Threshold, Filter, prefilter. |
| Too many events | Raise Threshold and Dist Freq/Amp/Dur. |
| Empty memory | MaxDatas, Memory Time, analyzer. |
| Saturation | Amp, FXpre/post, Amp_pre/post, PostAmp, VST Gain. |
| High CPU | Instrument count, speeds, effects, VST, automation. |
| Unstable algorithm | Lower speed, Chaos, Temperature, or Mutation. |
| Incomplete preset | Verify sounds, SynthDefs, VST, and paths. |

## 16. Exact Declared Control Specifications

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
| Volume, alternate | `-inf, 12, db` | EZSlider |
| Tempo | `1, 960, exp, 0` | EZSlider |
| Beats / Bar | `1, 64, exp, 0` | EZSlider |
| ChaosIn / NeuroneIn / GenetiqueIn Speed | `1, 100, lin, 1` | EZSlider |
| Algorithm Speed | `1, 100, lin, 1` | EZSlider |
| Automation Speed | `1, 100, lin, 1` | EZSlider |

SynthDef, sound, effect, output, MIDI-port, and plug-in lists are dynamic and depend on startup resources.
