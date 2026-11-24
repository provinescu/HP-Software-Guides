# Agents GUI: Numeric Ranges, Behavior, and Musical Use

**Analysis date:** October 5, 2026  
**Source basis:** `Agents - copie.txt`

This reference describes each major control by range, scale, behavior, musical effect, and useful precautions. Dynamic values that depend on output count, SynthDef lists, algorithm lists, signal duration, or world dimensions are identified as such.

## 1. Reading the Ranges

- `lin`: uniform progression across the control.
- `exp`: increased precision at small values.
- `db`: logarithmic amplitude scale.
- `ranger`: two handles defining minimum and maximum.
- `-1 / +1`: often direction, signed rate, or state.

Recommended startup: choose source and analyzer, keep population moderate, set BPM/Bar/scale, enable only a few genes, start at low output level, observe the virtual space, then save a reference preset.

## 2. MasterFX and Virtual Space

| Control | Range / scale | Behavior |
|---|---|---|
| LimitOut | SuperCollider dB spec | Final safety ceiling. |
| PostAmp | dB spec | Post-processing gain; also increases clipping risk. |
| ZoomX / ZoomY | 0–10, lin | Display scale only. |
| TranslateX / Y | ±(dimension × zoom), lin | Moves the world view. |
| ShearingX / Y | -2 to +2, lin | Visual shearing. |
| AngleX / Y / Z | -360° to +360°, lin | Rotates the representation. |
| Focus | 0–500, lin | View focal distance. |

Camera controls affect visualization, not internal musical coordinates.

## 3. Behavioral Genes

| Gene | Range | Effect |
|---|---|---|
| Ageing | 0–1, ranger | Bounds ageing influence on the life cycle. |
| Moving | 0–1, ranger | Movement probability/intensity. |
| Vision | 0–1, ranger | Spatial-perception capacity. |
| Hearing | 0–1, ranger | Musical perception/sharing capacity. |
| %Distance Agent | 0.001–1, exp | Relative interaction distance among agents. |
| %Distance Signal | 0.001–1, exp | Relative signal-perception distance. |
| %Deviance | 0.001–1, exp | Tolerance/divergence in decisions or movement. |

A narrow range homogenizes the population. A wide range produces contrasting individuals. Exponential controls are particularly sensitive below 0.1.

## 4. Core Musical Genes

| Control | Range / unit | Behavior |
|---|---|---|
| Frequency | 0–127 MIDI, ranger | Agent register. |
| Translate Frequency | -127 to +127 semitones, ranger | Very wide transposition; use cautiously. |
| Amplitude | -∞ to 0 dB, ranger | Agent dynamic range before later gains. |
| Duration | 0–60 s, ranger | Event duration. |
| Stretch Duration | -16 to +64, ranger | Time contraction/expansion; negative behavior depends on the active engine. |
| Pan | -1 to +1, ranger | Left-to-right position. |
| Root | 0–21, step 1 | Root: 0–11 for 12-TET; extended values for shrutis. |

## 5. Samples, Buffers, and Envelope

| Control | Range | Behavior |
|---|---|---|
| Time Buffer Sound | 0.01 s to maximum signal duration, exp | Available source duration. |
| Reverse Sample | -1 to +1, ranger | Relative playback direction/rate around zero. |
| Loop Sample | -1 to +1, ranger | Loop command sent to compatible readers. |
| Offset Sample | 0–1, ranger | Normalized buffer position. |
| Envelope Level | 0–1, ranger | Envelope-level bounds. |
| Envelope Duration | 0–1, ranger | Normalized segment-duration bounds. |
| Loop Buffer Music | -1 to +1, ranger | Musical-buffer loop/direction control. |
| Buffer Music | 1–256, exp, integer | Musical-memory size or item count. |
| Chord Max | 1–12, exp, integer | Maximum chord size. |
| Chord Dur | 0.01–1 s, exp | Chord-grouping time threshold. |

For click prevention, keep envelope endpoints at zero, avoid null segment durations, and test Reverse/Loop with a small population.

## 6. Synthesizer and Outputs

| Control | Range | Behavior |
|---|---|---|
| Control Synthesizer | 0–1, ranger | Two macro-control bounds sent to the SynthDef. |
| Audio Out | 1 to available output count, integer | Output channel/group. |
| MIDI Out | 0–16, integer | 0 disables; 1–16 selects channel. |
| Algorithm | 0 to list size - 1, integer | Algorithm selection by index. |
| Fhz | 0–127 MIDI | Global signal/event pitch. |
| Dur | 0–60 s | Global/signal duration. |
| Amp | -∞ to +12 dB, step 0.001 | Gain with positive headroom. |
| Pan | -1 to +1, step 0.001 | Fine stereo position. |
| Speed | 0.01–100, exp, step 0.01 | Very broad speed range. |

## 7. Analysis, Tempo, and Bands

| Control | Range | Behavior |
|---|---|---|
| FhzBand | 1–12, integer | Number of frequency bands. |
| Level FileIn | -120 to +12 dB, lin | Analyzed-file level. |
| Offset FileIn | 0–1, lin | Normalized file position. |
| BPM | 1–960, exp | System tempo. |
| Bar | 1–64, exp | Bar/cycle length. |
| Automation Time | 1/3600 to 60 s, exp, ranger | Automation interval; theoretical minimum ≈ 0.000278 s. |
| Keyboard Translate | -36 to +31 semitones, integer | Keyboard transposition. |
| Keyboard Volume | dB spec | Keyboard level. |

For readable musical timing, begin between 30 and 240 BPM. Extreme values are better suited to textures or modulation.

## 8. Population Evolution

| Control | Range | Behavior |
|---|---|---|
| %Crossover | 0–1, lin | Genome-crossover rate/intensity. |
| %Mutation | 0.001–1, exp | Mutation rate; highly sensitive below 0.1. |
| %Shared | 0–1, lin | Shared knowledge/memory proportion. |
| Activate Genes | On/Off buttons | Allows each selected gene to influence behavior. |
| Scale menu | No Scale, 12-TET, modes, pentatonics, ragas, shrutis | Constrains generated pitches. |
| Degrees | Editable list | Defines available scale degrees. |

High crossover mixes profiles quickly. High mutation reduces lineage stability. High Shared values homogenize the population; low values preserve individual identity.

## 9. Essential Buttons and States

| Button / menu | States | Effect |
|---|---|---|
| System Start/Stop | On/Off | Starts/stops population and processes. |
| Source | AudioIn / FileIn / Keyboard / MIDI | Selects event origin. |
| Analyzer | Onsets / Pitch / Pitch2 / KeyTrack | Selects extraction method. |
| Genes | On/Off per gene | Enables genetic influence. |
| Mute/Solo | On/Off | Removes or isolates a voice/agent. |
| AutoRoot | On/Off | Adapts the root. |
| Automation | On/Off | Enables programmed changes. |
| Record | Start/Pause/Stop | Controls audio or score recording. |
| Preset/Genome/Sequence/Score | Load/Save | Reads or writes the corresponding state layer. |

## 10. VST

| Control | Range / states | Behavior |
|---|---|---|
| Run | On/Off | Enables the VST engine. |
| Browse | Action | Selects a plug-in. |
| Editor / GUI | Action | Opens the plug-in editor. |
| Close | Action | Closes and resets the controller. |
| Out | 1–32 | Output channel. |
| xFade | 0–1, unipolar | Direct/processed blend. |
| Gain In | 0–1, unipolar | Normalized input gain. |
| Pan | -1 to +1, ranger | Pan range. |

## 11. Recommended Musical Starting Points

| Goal | Starting values |
|---|---|
| Stable texture | Mutation 0.001–0.03; Crossover 0.1–0.3; Shared 0.4–0.8; durations 1–8 s. |
| Contrasting population | Mutation 0.03–0.15; Crossover 0.3–0.7; Shared 0.1–0.4. |
| Evolving ecosystem | Mutation 0.05–0.25; Crossover 0.5–0.9; distances 0.05–0.4. |
| Slow cloud | BPM 20–60; durations 5–30 s; Speed 0.05–0.5. |
| Articulated counterpoint | BPM 80–160; Chord Dur 0.03–0.2 s; Chord Max 2–5. |
| Granular texture | Speed 2–30; broad offsets; short envelopes; limited population. |

These are starting points. Results depend on SynthDefs, sounds, population size, and analyzed source.

## 12. Troubleshooting

| Symptom | Check |
|---|---|
| No sound | System, Mute/Solo, Audio Out, Amp, MasterFX, source, synth. |
| Immobile population | Moving, distances, deviance, and gene state. |
| No interaction | Vision/Hearing and distances may be too low. |
| Extreme pitches | Frequency, Translate Frequency, Root, Scale. |
| Saturation | Amp up to +12 dB, PostAmp, population, FX, VST Gain. |
| High CPU | Population, Speed, effects, VST, rapid automation. |
| Chaotic evolution | Mutation/Crossover too high or Shared too low. |
| Inconsistent presets | External resources, SynthDef/sound lists, and paths. |

## 13. Exact Declared Control Specifications

| GUI label | Exact specification | Widget |
|---|---|---|
| ZoomX / ZoomY | `0, 10, lin, 0` | EZKnob |
| TranslateX | `~mx.neg*~zoomXSlider.value, ~mx*~zoomXSlider.value, lin, 0` | EZKnob |
| TranslateY | `~my.neg*~zoomYSlider.value, ~my*~zoomYSlider.value, lin, 0` | EZKnob |
| ShearingX / ShearingY | `-2, 2, lin, 0` | EZKnob |
| Ageing / Moving / Vision / Hearing | `0, 1, lin, 0` | EZRanger |
| Frequency | `0, 127, lin, 0` | EZRanger |
| Translate Frequency | `-127, 127, lin, 0` | EZRanger |
| Amplitude | `-inf, 0, db, 0` | EZRanger |
| Duration | `0, 60, lin, 0` | EZRanger |
| Stretch Duration | `-16, 64, lin, 0` | EZRanger |
| Pan, gene | `-1, 1, lin, 0` | EZRanger |
| Time Buffer Sound | `0.01, ~tempsmaxsignal, exp, 0` | EZRanger |
| Reverse Sample / Loop Sample | `-1, 1, lin, 0` | EZRanger |
| Offset Sample | `0, 1, lin, 0` | EZRanger |
| Envelope Level / Duration | `0, 1, lin, 0` | EZRanger |
| Control Synthesizer | `0, 1, lin, 0` | EZRanger |
| Audio Out | `1, ~sourceOutAgents.size, lin, 1` | EZRanger |
| MIDI Out | `0, 16, lin, 1` | EZRanger |
| Loop Buffer Music | `-1, 1, lin, 0` | EZRanger |
| Buffer Music | `1, 256, exp, 1` | EZRanger |
| Chord Max | `1, 12, exp, 1` | EZRanger |
| Chord Dur | `0.01, 1, exp, 0` | EZRanger |
| Algorithm | `0, ~listeAlgorithm.size - 1, lin, 1` | EZRanger |
| Fhz | `0, 127, lin, 0` | EZRanger |
| Dur | `0, 60, lin, 0` | EZRanger |
| Amp | `-inf, 12, db, 0.001` | EZKnob |
| Pan, global | `-1, 1, lin, 0.001` | EZKnob |
| Speed | `0.01, 100, exp, 0.01` | EZKnob |
| FhzBand | `1, 12, lin, 1` | EZKnob |
| Level FileIn | `-120, 12, lin, 0` | EZKnob |
| Offset FileIn | `0, 1, lin, 0` | EZKnob |
| BPM | `1, 960, exp, 0` | EZKnob |
| Bar | `1, 64, exp, 0` | EZKnob |
| %Distance Agent / Signal / Deviance | `0.001, 1, exp, 0` | EZKnob |
| Root | `0, 21, lin, 1` | EZKnob |
| Time | `3600.reciprocal, 60, exp, 0` | EZRanger |

Dynamic values: `~tempsmaxsignal` depends on the loaded signal; `~sourceOutAgents.size` depends on routing; `~listeAlgorithm.size` depends on the installed list; TranslateX/Y depends on world dimensions and zoom.
