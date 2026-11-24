# Matrix Music GUI: Complete Control and Numeric Range Reference

**Expected function of each control**  
Version: October 5, 2026

> **Audience:** musicians, composers, performers, and technicians using Matrix Music without reading the SuperCollider source.

## 1. Matrix Music in One Minute

Matrix Music receives audio, file, keyboard, or MIDI sources, builds musical and spectral data, classifies them in a matrix of inputs and pitch bands, then feeds independent Synth/FX cells. Each cell can memorize, transform, sequence, and route its own material.

| Level | Function |
|---|---|
| Matrix Control | Source, analysis, tempo, automation, and global state. |
| Matrix | Cell organization and input/band relationships. |
| Synth/FX cell | Sound, memory, algorithm, timing mode, filter, effect, routing. |
| Master | Gain, limiter, automation, and shared parameters. |
| External interfaces | Keyboard, MIDI, VST, and OSC. |

Create one cell, choose a simple synth, select Default, keep gain low, and add routing/automation only after validating the signal.

## 2. Reading Control Types

| Type | Meaning |
|---|---|
| EZSlider / EZKnob | One value. |
| EZRanger | Two handles: minimum and maximum. |
| `lin` | Uniform variation across the travel. |
| `exp` | Increased precision at low values. |
| `db` | Logarithmic amplitude scale. |
| `unipolar` | Normalized 0–1 value. |
| `bipolar` | Normalized -1 to +1 value. |
| step 1 | Integer value for MIDI, quantization, or indices. |

A GUI range describes allowed values, not necessarily a musically safe region. Test positive gain, extreme transposition, and very high speed at low listening level.

## 3. Global and Master Controls

| Control | Exact range | Expected behavior |
|---|---:|---|
| Auto% | 0–100%, linear | Proportion/probability of global automation. |
| BPM | 1–960, exponential | System tempo; conventional ranges create pulse, extremes create textures. |
| Volume | Standard dB ControlSpec | Global or keyboard level depending on the window. |
| LimitOut | Standard dB ControlSpec | Output limiter threshold. |
| PostAmp | Standard dB ControlSpec | Gain after many processing and summing stages. |
| VST xFade | 0–1 | 0 = direct; 1 = processed. |
| VST Gain In | 0–1 | Normalized VST input level. |
| VST Pan | -1 to +1, ranger | VST spatial range. |

Start with neutral or reduced PostAmp, low cell levels, and limiter headroom. Send Bus, Local In, and cell summation can quickly increase total level.

## 4. File Source, Input, Buffers, and Feedback

| Control | Exact range | Expected function |
|---|---:|---|
| FileAmp | -∞ to +12 dB | File gain before reading and analysis. |
| FileOffset | 0–1, linear | Normalized file position: start to end. |
| Offset | 0–1, linear | Normalized cell-buffer start position. |
| Pre | 0–1, linear | Amount of previous content retained during writing/feedback. |
| Post | 0–1, linear | Amount applied after the relevant stage. |
| Local In | dB ControlSpec | Local return/feedback level. |
| Send Bus | dB ControlSpec | Level sent to an audio bus. |
| Send Bus FX | dB ControlSpec | Level sent to the effect network. |
| Loop | On/Off | Repeats a buffer when supported by the SynthDef. |
| Reverse | On/Off or signed rate | Reverses playback when negative rate is supported. |
| Clear / Reset | Action | Clears buffer, memory, or algorithmic state. |

Pre near 0 replaces previous content quickly; Pre near 1 favors accumulation. A Local In + Send Bus loop may self-amplify, so start at -∞ dB or very low levels.

## 5. Musical Data for Each Cell

| Control | Exact range | Expected musical effect |
|---|---:|---|
| Freq | 0–127 MIDI, ranger | Minimum and maximum register. |
| Transpose | -127 to +127 semitones | Transposition after selection/remapping. |
| Amp | dB ControlSpec, ranger | Dynamic bounds for the cell. |
| Dur | 0–60 s, ranger | Event-duration range. |
| Quant | 1–100, integer | Quantization resolution. |
| Time | 1/16–100 s, exponential, ranger | Timing window in one cell section. |
| Time, alternate | 1/60–100 s, exponential, ranger | Finer timing variant in another section. |
| Jitter | 0–1, linear | Normalized random variation. |

An EZRanger defines a choice/remapping zone rather than one fixed value. Narrow ranges stabilize the cell; wide ranges increase diversity.

## 6. Chords and Memory

| Control | Range | Behavior |
|---|---:|---|
| ChdT | 0.01–60 s, exponential, 0.01 step | Time window used to build or recognize a chord. |
| ChdS | 0–12, integer | Chord size; 0 may disable grouping depending on mode. |
| Buffer I / II | Selection/actions | Two memories or sources available to the cell. |
| Loop | Button | Repeats buffer content. |
| Reverse | Button/signed speed | Reverses playback if supported. |
| Clear / Reset | Action | Clears memory or resets the cell. |

Short ChdT groups nearly simultaneous attacks. Long ChdT creates broader but less articulated harmonic aggregation. Large ChdS increases harmonic density and output level.

## 7. Algorithms

| Algorithm | Intended behavior | Evaluation method |
|---|---|---|
| Default | Passes or remaps data without a complex adaptive model | Use first to validate source, memory, and synthesis. |
| Probability | Chooses events from distributions or calculated probabilities | Medium memory, low Jitter, narrow pitch range. |
| Euclide | Compares events by distance among pitch, amplitude, and duration | Vary one dimension at a time. |
| Genetic | Evolves solutions through selection, crossover, and mutation | Slow speed, automation off, save beforehand. |
| Kohonen | Organizes events by similarity in a self-organizing map | Provide varied memory and learning time. |
| Neural | Predicts/transforms from previous events and a learned model | Begin with a repetitive source, then add variation. |

An algorithm changes selection logic, not necessarily timbre. Compare algorithms with the same SynthDef, ranges, and timing mode.

## 8. Timing Modes

| Mode | Intended behavior | Musical use |
|---|---|---|
| Tdef | Schedules events on a clock and recreates actions as a pattern | Quantized sequences, polyrhythm, reproducible form. |
| OnFly | Creates a response when new data arrives | Reactive live transformation. |
| Stream | Maintains a continuous synth/stream and updates parameters | Drones, continuous morphology, spectral control. |

Validate Default with Tdef first. Change only one axis at a time: algorithm or timing mode.

## 9. Matrix Routing

| Control | Range / state | Effect |
|---|---|---|
| Audio Out | Dynamic menu | Physical or logical output destination. |
| Cell Link | On/Off or menu | Serial, parallel, or bus relationship between cells. |
| Send Bus | dB | Sends a copy to another destination. |
| Send Bus FX | dB | Sends to an effect chain. |
| Local In | dB | Receives a local return and enables recursive structures. |
| Pre / Post | 0–1 | Placement/proportion before or after processing. |
| Mute | On/Off | Silences the cell without necessarily stopping computation. |
| Solo | On/Off | Isolates the cell. |
| Bypass / Off | On/Off | Bypasses selected processing. |
| MoveBefore / MoveAfter | Action | Changes server calculation order. |

Any loop involving Local In, Send Bus, or linked cells can self-amplify. Start very low, enable the limiter, and increase gradually.

## 10. SynthDef, Filter, and FX Controls

| Zone | Behavior |
|---|---|
| Synth menu | Selects the synthesis or playback engine. |
| Filter menu | Inserts a filter or spectral process. |
| FX menu | Inserts a cell effect. |
| Engine-specific controls | Appear according to the SynthDef and use the ranges declared by that engine. |
| Envelope | Controls articulation and click prevention. |
| Dry/Wet or xFade | Blends direct and processed signals when available. |
| Bypass/Off | Removes a treatment without necessarily deleting the cell. |

The `Synthesizer Controls` MultiSlider sends up to twelve macros, `ctrl1...ctrl12`. Meaning changes with the engine. Adjust one macro at a time and compare at equal loudness.

FFT processing usually costs more CPU and may require valid Buffer I and Buffer II material.

## 11. Automation

| Control | Range | Behavior |
|---|---:|---|
| Auto% | 0–100 | Probability/amount of automated parameters. |
| Time | 1/16–100 or 1/60–100 s | Change interval/window. |
| Jitter | 0–1 | Variation amplitude. |
| Automation On/Off | Button | Enables or suspends the routine. |
| Targets | Menu/buttons | Selects parameters allowed to move. |
| Reset | Action | Restores a stable or initial state. |

For performance, automate a small number of parameters with Auto% around 5–20 and Jitter around 0.05–0.2. Do not automate gains, routing, and feedback simultaneously.

## 12. VST, MIDI, Keyboard, and OSC

| System | Controls | Behavior |
|---|---|---|
| Keyboard | dB volume, transposition, keys | Generates musical data directly. |
| MIDI In | Port/channel | Feeds cells or analysis. |
| MIDI Out | Port/channel | Duplicates/exports generated events. |
| VST Run | On/Off | Enables the plug-in. |
| VST Browse | Action | Selects a plug-in. |
| VST Editor / GUI | Action | Opens generic/native UI. |
| VST Out | Output menu | Routes plug-in audio. |
| OSC | Address/port | External control and synchronization. |

Validate the direct signal before enabling VST, with Gain In low. Avoid unintended doubling when MIDI Out drives an instrument whose return re-enters Matrix Music.

## 13. Buttons and Operational States

| Button / menu | State | Consequence |
|---|---|---|
| Start/Stop | Two states | Starts/stops global routines. |
| Add Synth/FX | Action | Creates a cell and its buses. |
| Delete | Action | Releases cell, synths, effects, and routes. |
| Cell Run | On/Off | Enables the cell timing engine. |
| Source | Audio/File/Keyboard/MIDI | Changes data origin. |
| Analyzer | Onsets/Pitch/Pitch2/KeyTrack | Changes extraction method. |
| Buffer select | I/II | Selects the active memory. |
| Loop / Reverse | On/Off | Changes buffer playback. |
| Algorithm | Menu | Changes musical transformation. |
| Mode | Tdef/OnFly/Stream | Changes timing behavior. |
| Preset Load/Save | Action | Restores or writes configuration. |
| Reset | Action | Resets the relevant memory/algorithm. |

When a label remains unchanged in both states, color usually indicates active/inactive. Before performance, verify Cell Run, Mute, Solo, automation, and routing visually.

## 14. Recommended Starting Configurations

| Goal | Initial configuration |
|---|---|
| First texture | 1 cell, Default, Tdef, Freq 48–72, low Amp, Dur 0.2–2 s, Quant 4–8. |
| Live transformation | Onsets, Time 0.1–2 s, Jitter 0.05–0.15, one FX. |
| Slow cloud | Pitch2, Freq 36–96, Dur 3–20 s, Quant 1–4, Time 2–20 s. |
| Counterpoint | 2–4 cells, different bands, Quant 4–16, ChdS 2–5. |
| Algorithmic texture | Probability or Euclide, Jitter 0.1–0.3, moderate ranges. |
| Adaptive evolution | Genetic/Kohonen/Neural, sufficient memory, weak automation. |

## 15. Troubleshooting

| Symptom | Controls to check |
|---|---|
| No sound | Global/cell Run, Synth menu, Amp, Volume, Out, Mute/Solo. |
| No data | Source, analyzer, input bus, thresholds, FileAmp, and Offset. |
| Frozen cell | Timing mode, Time, memory/buffer, and Cell Run. |
| Texture too dense | Dur, Quant, Time, cell count, and ChdS. |
| Saturation | Amp, Send Bus, Local In, PostAmp, FX, VST Gain. |
| High CPU | Cell count, FFT, Stream, effects, automation. |
| Extreme pitches | Freq, Transpose, Scale/Root, algorithm. |
| Incomplete preset | SynthDefs, sounds, plug-ins, external paths. |

## 16. Declared GUI Specifications

| GUI label | Exact ControlSpec-style declaration | Widget |
|---|---|---|
| xFade | `unipolar` | EZKnob |
| Gain In | `unipolar` | EZKnob |
| Pan | `bipolar` | EZRanger |
| Volume | `db` | EZSlider |
| LimitOut | `db` | EZSlider |
| PostAmp | `db` | EZSlider |
| Auto% | `0, 100, lin, 0` | EZKnob |
| BPM | `1, 960, exp, 0` | EZSlider |
| FileAmp | `-inf, 12, db, 0` | EZSlider |
| FileOffset | `0, 1, lin, 0` | EZSlider |
| Send Bus | `db` | EZSlider |
| Send Bus FX | `db` | EZSlider |
| Local In | `db` | EZSlider |
| Offset | `0, 1, lin, 0` | EZSlider |
| Pre | `0, 1, lin, 0` | EZSlider |
| Post | `0, 1, lin, 0` | EZSlider |
| Freq | `0, 127, lin, 0` | EZRanger |
| Transpose | `-127, 127, lin, 0` | EZSlider |
| Amp | `db` | EZRanger |
| Dur | `0, 60, lin, 0` | EZRanger |
| Quant | `1, 100, lin, 1` | EZSlider |
| Time | `16.reciprocal, 100.0, exp, 0` | EZRanger |
| Jitter | `0.0, 1.0, lin, 0` | EZSlider |
| Time, alternate | `60.reciprocal, 100.0, exp, 0` | EZRanger |
| ChdT | `0.01, 60, exp, 0.01` | EZSlider |
| ChdS | `0, 12, lin, 1` | EZSlider |

`db`, `unipolar`, and `bipolar` use SuperCollider's standard ControlSpecs. Output, SynthDef, filter, and effect menus are dynamic and depend on loaded resources.
