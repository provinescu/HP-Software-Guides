# Agents GUI Reference: Controls, Ranges, and Expected Behavior

**Numerical ranges and intended function of each control**  
Version: October 5, 2026

> `lin` = linear scale; `exp` = exponential scale with increased precision at small values; `db` = logarithmic amplitude; `ranger` = minimum/maximum handles. Values written as `-1 / +1` often encode direction, signed rate, or state. Dynamic limits depend on output count, SynthDef lists, algorithm lists, world dimensions, or loaded files.

## 1. Startup Principles

1. Select the source and analyzer.
2. Keep the population moderate.
3. Set BPM, bar length, root, and scale.
4. Enable only a few genes.
5. Start at a reduced output level.
6. Observe the virtual space and adjust distance, vision, and hearing.
7. Save a reference preset.

## 2. MasterFX and Virtual-Space View

| Control | Range / scale | Expected behavior |
|---|---|---|
| LimitOut | SuperCollider dB specification | Safety ceiling on final output. |
| PostAmp | dB specification | Post-processing gain; also increases clipping risk. |
| ZoomX / ZoomY | 0–10, lin | Display scale only. |
| TranslateX / Y | ±(dimension × zoom), lin | Moves the world view. |
| ShearingX / Y | -2 to +2, lin | Visual shearing. |
| AngleX / Y / Z | -360° to +360°, lin | Rotates the representation. |
| Focus | 0–500, lin | Viewing focal distance. |

Camera controls affect visualization, not internal musical coordinates.

## 3. Behavioral Genes

| Gene | Range | Engine function and musical consequence |
|---|---|---|
| Ageing | 0–1, ranger | Bounds each agent's ageing factor. High values accelerate population turnover. |
| Moving | 0–1, ranger | Movement probability/intensity. Changes encounters and interaction frequency. |
| Vision | 0–1, ranger | Amount of spatial neighborhood available to an agent. Low values favor independence; high values favor collective reactions. |
| Hearing | 0–1, ranger | Ability to consider nearby musical events. High values increase imitation or response. |
| %Distance Agent | 0.001–1, exp | Relative proximity threshold between agents. Small values produce local interaction; large values create a collective network. |
| %Distance Signal | 0.001–1, exp | Relative signal-perception threshold. Small values are selective; large values expose agents to more events. |
| %Deviance | 0.001–1, exp | Adds tolerated divergence to movement or selection. Low values are regular; high values are less predictable. |

`Ageing`, `Moving`, `Vision`, and `Hearing` are `EZRanger` controls. Their two handles define a distribution range for individual agents; they do not directly control one specific agent.

## 4. Core Musical Genes

| Control | Range / unit | Expected behavior | Precaution |
|---|---|---|---|
| Frequency | 0–127 MIDI, ranger | Allowed agent register. | Start with a restricted range. |
| Translate Frequency | -127 to +127 semitones, ranger | Signed pitch transposition. | Extreme values may leave the useful register. |
| Amplitude | -∞ to 0 dB, ranger | Per-agent dynamic range before final mixing. | Many agents sum together even below 0 dB. |
| Duration | 0–60 s, ranger | Minimum and maximum event duration. | Near-zero values can click or create excessive density. |
| Stretch Duration | -16 to +64, ranger | Contracts or expands duration; negative behavior depends on the active engine branch. | Test negative values with few agents. |
| Pan | -1 to +1, ranger | Available stereo area. | A narrow range stabilizes location. |
| Root | 0–21, integer | Root used by scales and tuning tables. | Use 0–11 for 12-TET; extended values may address microtonal tables. |
| Scale | menu | Constrains generated pitches to a scale, mode, raga, or shruti table. | `No Scale` leaves pitches unconstrained. |

## 5. Samples, Buffers, and Envelopes

| Control | Range | Behavior |
|---|---|---|
| Time Buffer Sound | 0.01 s to maximum signal duration, exp | Available time span of live or loaded material. |
| Reverse Sample | -1 to +1, ranger | Signed direction/rate supplied to compatible readers. |
| Loop Sample | -1 to +1, ranger | Loop or repetition control supplied to compatible readers. |
| Offset Sample | 0–1, ranger | Normalized read position from beginning to end. |
| Envelope Level | 0–1, ranger | Bounds envelope point amplitudes. |
| Envelope Duration | 0–1, ranger | Bounds normalized envelope segment times. |
| Loop Buffer Music | -1 to +1, ranger | Musical-buffer loop/direction control. |
| Buffer Music | 1–256, exp, integer | Musical memory size or item count. |
| Chord Max | 1–12, exp, integer | Maximum simultaneous chord notes. |
| Chord Dur | 0.01–1 s, exp | Event-grouping window for chord formation. |

For click-free articulation, keep the first and final envelope levels at zero, avoid zero-length segments, and initially test Reverse/Loop with a small population. Reverse and Loop are audible only when the selected SynthDef reads those parameters.

## 6. Synthesizer and Outputs

| Control | Range | Behavior |
|---|---|---|
| Control Synthesizer | 0–1, ranger | Generic macro bounds sent to compatible SynthDefs. |
| Audio Out | 1 to available output count, integer | Output channel or group. |
| MIDI Out | 0–16, integer | 0 disables output; 1–16 selects a MIDI channel. |
| Algorithm | 0 to list size - 1, integer | Algorithm selection by index. |
| Fhz | 0–127 MIDI | Global signal/event pitch. |
| Dur | 0–60 s | Global or signal duration. |
| Amp | -∞ to +12 dB, step 0.001 | Amplitude with positive gain reserve. |
| Pan | -1 to +1, step 0.001 | Fine stereo position. |
| Speed | 0.01–100, exp, step 0.01 | Very broad speed control; low values are extremely slow. |

## 7. Analysis, Tempo, and Frequency Bands

| Control | Range | Behavior |
|---|---|---|
| Thresh In | 0–1 | Input-analysis sensitivity. Exact direction depends on analyzer; excessive threshold may suppress weak events. |
| Filter In | 0–1 | Analysis filtering or confidence threshold. |
| FhzBand | 1–12, integer | Number of frequency bands. |
| Level FileIn | -120 to +12 dB, lin | File level before analysis. |
| Offset FileIn | 0–1, lin | Normalized playback position in the file. |
| BPM | 1–960, exp | System tempo. Start between 30 and 240 BPM for conventional musical control. |
| Bar | 1–64, exp | Bar or cycle length. |
| Automation Time | 1/3600 to 60 s, exp, ranger | Minimum/maximum interval between automatic changes. The theoretical minimum is about 0.000278 s and may be computationally impractical. |
| Keyboard Translate | -36 to +31 semitones | Computer-keyboard transposition. |
| Keyboard Volume | dB specification | Keyboard level. |

Band buttons `All` and `1...12` keep the same text in both states. Green means active; red means inactive.

## 8. Population Evolution

| Control | Range | Behavior |
|---|---|---|
| %Crossover | 0–1, lin | Amount or probability of genome crossover. |
| %Mutation | 0.001–1, exp | Mutation rate/intensity; highly sensitive below 0.1. |
| %Shared | 0–1, lin | Proportion of shared knowledge or memory. |
| Activate Genes | On/Off buttons | Allows each selected gene to influence behavior. |
| Scale menu | No Scale, 12-TET, modes, pentatonics, ragas, shrutis | Constrains pitch generation. |
| Degrees | Editable list | Defines available scale degrees. |

High crossover mixes profiles quickly. High mutation reduces lineage stability. High Shared values homogenize the population; low values preserve individual identity.

## 9. Essential Buttons and States

| Button / menu | States | Effect |
|---|---|---|
| System Start/Stop | On/Off | Starts or stops population and runtime processes. |
| Source | AudioIn / FileIn / Keyboard / MIDI | Selects event origin. |
| Analyzer | Onsets / Pitch / Pitch2 / KeyTrack | Selects musical extraction method. |
| Genes | On/Off per gene | Enables genetic parameter influence. |
| Mute/Solo | On/Off | Removes or isolates a voice/agent as defined by the panel. |
| AutoRoot | On/Off | Adapts the root. |
| Automation | On/Off | Enables programmed changes. |
| Record | Start/Pause/Stop | Controls audio or score recording. |
| Preset/Genome/Sequence/Score | Load/Save | Reads or writes the corresponding state layer. |

## 10. VST Controls

| Control | Range / states | Behavior |
|---|---|---|
| Run | On/Off | Activates the VST engine. |
| Browse | Action | Selects a plug-in. |
| Editor / GUI | Action | Opens plug-in interfaces. |
| Close | Action | Closes and resets the controller. |
| Out | 1–32 | Output channel. |
| xFade | 0–1 | Dry/wet blend. |
| Gain In | 0–1 | Normalized input gain. |
| Pan | -1 to +1, ranger | Panning range. |

## 11. Recommended Starting Values

| Goal | Starting values |
|---|---|
| Stable texture | Mutation 0.001–0.03; Crossover 0.1–0.3; Shared 0.4–0.8; durations 1–8 s. |
| Contrasting population | Mutation 0.03–0.15; Crossover 0.3–0.7; Shared 0.1–0.4. |
| Evolutionary ecosystem | Mutation 0.05–0.25; Crossover 0.5–0.9; distances 0.05–0.4. |
| Slow cloud | BPM 20–60; durations 5–30 s; Speed 0.05–0.5. |
| Articulated counterpoint | BPM 80–160; Chord Dur 0.03–0.2 s; Chord Max 2–5. |
| Granular texture | Speed 2–30; broad offsets; short envelopes; limited population. |
| Local interaction | Agent/Signal distance 0.01–0.1; low Vision/Hearing. |
| Collective behavior | Distance 0.2–0.6; high Vision/Hearing; high Shared. |

These are starting points. Results depend on SynthDefs, loaded sounds, source material, routing, and population size.

## 12. Troubleshooting

| Symptom | Check |
|---|---|
| No sound | System, Mute/Solo, Audio Out, Amp, MasterFX, source, and SynthDef. |
| Immobile population | Moving, distances, deviance, and gene state. |
| No interaction | Vision/Hearing and distances may be too low. |
| Extreme pitches | Frequency, Translate Frequency, Root, and Scale. |
| Saturation | Amp up to +12 dB, PostAmp, population, FX, and VST Gain. |
| High CPU | Population, Speed, effects, VST, and excessively fast automation. |
| Chaotic evolution | Mutation/Crossover too high or Shared too low. |
| Inconsistent presets | External resources, SynthDef/sound lists, and paths. |

## 13. Exhaustive Declared GUI Specifications

| GUI label | Exact ControlSpec-style declaration | Widget |
|---|---|---|
| ZoomX | `0, 10, lin, 0` | EZKnob |
| ZoomY | `0, 10, lin, 0` | EZKnob |
| TranslateX | `~mx.neg*~zoomXSlider.value, ~mx*~zoomXSlider.value, lin, 0` | EZKnob |
| TranslateY | `~my.neg*~zoomYSlider.value, ~my*~zoomYSlider.value, lin, 0` | EZKnob |
| ShearingX | `-2, 2, lin, 0` | EZKnob |
| ShearingY | `-2, 2, lin, 0` | EZKnob |
| Ageing / Moving / Vision / Hearing | `0, 1, lin, 0` | EZRanger |
| Frequency | `0, 127, lin, 0` | EZRanger |
| Translate Frequency | `-127, 127, lin, 0` | EZRanger |
| Amplitude | `-inf, 0, db, 0` | EZRanger |
| Duration | `0, 60, lin, 0` | EZRanger |
| Stretch Duration | `-16, 64, lin, 0` | EZRanger |
| Pan (gene) | `-1, 1, lin, 0` | EZRanger |
| Time Buffer Sound | `0.01, ~tempsmaxsignal, exp, 0` | EZRanger |
| Reverse / Loop Sample | `-1, 1, lin, 0` | EZRanger |
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
| Pan (global) | `-1, 1, lin, 0.001` | EZKnob |
| Speed | `0.01, 100, exp, 0.01` | EZKnob |
| FhzBand | `1, 12, lin, 1` | EZKnob |
| Level FileIn | `-120, 12, lin, 0` | EZKnob |
| Offset FileIn | `0, 1, lin, 0` | EZKnob |
| BPM | `1, 960, exp, 0` | EZKnob |
| Bar | `1, 64, exp, 0` | EZKnob |
| %Distance Agent / Signal / Deviance | `0.001, 1, exp, 0` | EZKnob |
| Root | `0, 21, lin, 1` | EZKnob |
| Time | `3600.reciprocal, 60, exp, 0` | EZRanger |

Dynamic values: `~tempsmaxsignal` depends on the signal or file; `~sourceOutAgents.size` depends on routing; `~listeAlgorithm.size` depends on the installed list; TranslateX/Y depends on world dimensions and zoom.
