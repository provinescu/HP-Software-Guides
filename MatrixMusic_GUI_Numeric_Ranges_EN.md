# Matrix Music GUI: Numeric Ranges and Control Behavior

**For musicians, composers, performers, and technicians**  
Version: October 5, 2026

## 1. System Levels

Matrix Music receives audio, file, keyboard, or MIDI input, creates musical/spectral data, classifies it by input and pitch band, then feeds independent Synth/FX cells.

| Level | Function |
|---|---|
| Matrix Control | Source, analysis, tempo, automation, global state. |
| Matrix | Cell organization and input/band relationships. |
| Synth/FX cell | Sound, memory, algorithm, timing, filter, effect, routing. |
| Master | Gain, limiter, automation, shared parameters. |
| External interfaces | Keyboard, MIDI, VST, OSC. |

Start with one cell, a simple synth, Default, low gain, and no advanced routing/automation.

## 2. Control Types

| Type | Meaning |
|---|---|
| EZSlider / EZKnob | One value. |
| EZRanger | Two handles: minimum and maximum. |
| `lin` | Uniform variation. |
| `exp` | Increased precision at low values. |
| `db` | Logarithmic amplitude. |
| `unipolar` | 0 to 1. |
| `bipolar` | -1 to +1. |
| step 1 | Integer value. |

A GUI range defines allowed values, not necessarily a musically safe zone. Test positive gain, extreme transposition, and high speed at low level.

## 3. Global and Master Controls

| Control | Range | Behavior |
|---|---:|---|
| Auto% | 0–100%, lin | Amount/probability of global automation. |
| BPM | 1–960, exp | System tempo. |
| Volume | dB ControlSpec | Global or keyboard level. |
| LimitOut | dB ControlSpec | Output limiter threshold. |
| PostAmp | dB ControlSpec | Gain after many sums/processes. |
| VST xFade | 0–1 | 0 direct, 1 processed. |
| VST Gain In | 0–1 | Normalized VST input. |
| VST Pan | -1 to +1, ranger | VST spatial range. |

Begin with neutral/reduced PostAmp, low cell levels, and limiter headroom. Send Bus, Local In, and cell summation can quickly increase total level.

## 4. File Source, Buffers, and Feedback

| Control | Exact range | Behavior |
|---|---:|---|
| FileAmp | -∞ to +12 dB | File gain before reading/analysis. |
| FileOffset | 0–1, lin | Normalized file position. |
| Offset | 0–1, lin | Normalized cell-buffer position. |
| Pre | 0–1, lin | Previous content retained during write/feedback. |
| Post | 0–1, lin | Amount applied after the relevant stage. |
| Local In | dB ControlSpec | Local return/feedback level. |
| Send Bus | dB ControlSpec | Audio-bus send level. |
| Send Bus FX | dB ControlSpec | Effect-network send level. |

Pre near 0 replaces old content quickly. Pre near 1 favors accumulation. Local In plus Send Bus may create unstable feedback, so begin at -∞ dB or very low levels.

## 5. Musical Data per Cell

| Control | Exact range | Musical effect |
|---|---:|---|
| Freq | 0–127 MIDI, ranger | Register bounds. |
| Transpose | -127 to +127 semitones | Transposition after selection/remapping. |
| Amp | dB ControlSpec, ranger | Dynamic bounds. |
| Dur | 0–60 s, ranger | Event durations. |
| Quant | 1–100, integer | Quantization resolution. |
| Time | 1/16–100 s, exp, ranger | Timing window in one section. |
| Time, alternate | 1/60–100 s, exp, ranger | Finer timing variant. |
| Jitter | 0–1, lin | Normalized random variation. |

An EZRanger defines a choice/remapping zone, not one fixed value. Narrow ranges stabilize the cell; wide ranges increase diversity.

## 6. Chords and Memory

| Control | Range | Behavior |
|---|---:|---|
| ChdT | 0.01–60 s, exp, step 0.01 | Time threshold for building/recognizing a chord. |
| ChdS | 0–12, integer | Chord size; 0 may disable grouping depending on mode. |
| Buffer I / II | Selection/actions | Two memories or sources per cell. |
| Loop | Button | Repeats buffer content. |
| Reverse | Button/signed speed | Reverses playback if supported. |
| Clear / Reset | Action | Clears memory or resets the cell. |

Very short ChdT groups nearly simultaneous attacks. Long ChdT creates broader but less articulated harmony. High ChdS increases harmonic density and level.

## 7. Algorithms and Timing Modes

| Element | Type | Behavior |
|---|---|---|
| Default | Algorithm | Direct data remapping. |
| Probability | Algorithm | Statistics-guided probabilistic decisions. |
| Euclide | Algorithm | Distance-based combination of frequency/amplitude/duration. |
| Genetic | Algorithm | Evolution and mutation of solutions. |
| Kohonen | Algorithm | Similarity-based organization. |
| Neural | Algorithm | Prediction from preceding events. |
| Tdef | Timing mode | Clock-scheduled pattern. |
| OnFly | Timing mode | Reaction to incoming events. |
| Stream | Timing mode | Continuous synth updated without full recreation. |

Validate Default with Tdef first. Change only algorithm or timing mode, not both simultaneously.

## 8. Matrix Routing

| Control | Range / state | Effect |
|---|---|---|
| Send Bus | dB | Sends the cell to another bus. |
| Send Bus FX | dB | Sends to an effect chain. |
| Local In | dB | Returns a bus into the cell; may create feedback. |
| Pre / Post | 0–1 | Position/proportion before or after processing. |
| Audio Out | Dynamic menu | Physical-output assignment. |
| Cell Link | On/Off or menu | Serial/parallel relation among cells. |
| Mute | On/Off | Silences the cell. |
| Solo | On/Off | Isolates the cell. |

Start sends very low, enable the limiter, and increase gradually.

## 9. SynthDef, Filters, Effects, and Automation

Synth, Filter, and FX menus are dynamic. Engine-specific parameters inherit ranges from the selected SynthDef. Envelope controls articulation and click prevention; Dry/Wet/xFade blends direct and processed paths.

| Automation control | Range | Behavior |
|---|---:|---|
| Auto% | 0–100 | Amount/probability of automated parameters. |
| Time | 1/16–100 or 1/60–100 s | Change interval/window. |
| Jitter | 0–1 | Variation amplitude. |
| Automation On/Off | Button | Enables/suspends routine. |
| Targets | Menu/buttons | Selects parameters allowed to move. |
| Reset | Action | Restores a stable/initial state. |

For stage use, start with Auto% 5–20 and Jitter 0.05–0.2. Avoid automating gain, routing, and feedback together.

## 10. MIDI, Keyboard, VST, and OSC

| System | Controls | Behavior |
|---|---|---|
| Keyboard | dB volume and transposition | Generates musical data directly. |
| MIDI In | Port/channel | Feeds cells or analysis. |
| MIDI Out | Port/channel | Exports generated events. |
| VST Run | On/Off | Enables the plug-in. |
| VST Browse | Action | Selects a plug-in. |
| VST Editor / GUI | Action | Opens editor. |
| VST Out | Output menu | Routes plug-in audio. |
| OSC | Address/port | External control and synchronization. |

Validate the direct signal before enabling VST. Avoid MIDI loops in which an external instrument returns to Matrix Music and retriggers itself.

## 11. Main Buttons and State Changes

| Command | State | Consequence |
|---|---|---|
| Start/Stop | Two states | Starts/stops global routines. |
| Add Synth/FX | Action | Creates a cell and buses. |
| Delete | Action | Releases cell, synths, effects, and routes. |
| Cell Run | On/Off | Enables timing engine. |
| Source | Audio/File/Keyboard/MIDI | Changes data origin. |
| Analyzer | Onsets/Pitch/Pitch2/KeyTrack | Changes detection. |
| Buffer select | I/II | Selects active memory. |
| Loop/Reverse | On/Off | Changes buffer playback. |
| Algorithm | Menu | Changes musical transformation. |
| Mode | Tdef/OnFly/Stream | Changes timing behavior. |
| Preset Load/Save | Action | Restores/writes configuration. |
| Reset | Action | Resets relevant data/algorithm. |

## 12. Recommended Starting Configurations

| Goal | Configuration |
|---|---|
| First texture | 1 cell, Default, Tdef, Freq 48–72, low Amp, Dur 0.2–2 s, Quant 4–8. |
| Live transformation | Onsets, Time 0.1–2 s, Jitter 0.05–0.15, one FX. |
| Slow cloud | Pitch2, Freq 36–96, Dur 3–20 s, Quant 1–4, Time 2–20 s. |
| Counterpoint | 2–4 cells, different bands, Quant 4–16, ChdS 2–5. |
| Algorithmic texture | Probability or Euclide, Jitter 0.1–0.3, moderate ranges. |
| Adaptive evolution | Genetic/Kohonen/Neural, sufficient memory, weak automation. |

## 13. Troubleshooting

| Symptom | Check |
|---|---|
| No sound | Global/cell Run, Synth menu, Amp, Volume, Out, Mute/Solo. |
| No data | Source, analyzer, input bus, thresholds, FileAmp, Offset. |
| Frozen cell | Timing mode, Time, memory/buffer, Cell Run. |
| Too dense | Dur, Quant, Time, cell count, ChdS. |
| Saturation | Amp, Send Bus, Local In, PostAmp, FX, VST Gain. |
| High CPU | Cell count, FFT, Stream, effects, automation. |
| Extreme pitch | Freq, Transpose, scale/root, algorithm. |
| Incomplete preset | SynthDefs, sounds, plug-ins, external paths. |

## 14. Exact Declared Specifications

| GUI label | Exact specification | Widget |
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
| Send Bus / Send Bus FX / Local In | `db` | EZSlider |
| Offset / Pre / Post | `0, 1, lin, 0` | EZSlider |
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

`db`, `unipolar`, and `bipolar` use SuperCollider's standard ControlSpecs. Output, SynthDef, filter, and effect menus depend on loaded resources.
