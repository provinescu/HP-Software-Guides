# Matrix Music: Operating Principles and System Logic

**Musical-data architecture, algorithms, timing, and routing**  
Version: October 4, 2026

> **Scope:** Based on the supplied MatrixMusic source analysis. Available resources may vary with installed SynthDefs, presets, sounds, plug-ins, and extensions.

## 1. General Function

Matrix Music is a composition and processing matrix. It converts one or more sources into musical vectors, classifies them by input and pitch band, stores those vectors, and allows every Synth/FX cell to read them through an independent algorithm.

| Layer | Role |
|---|---|
| Acquisition | Audio, file, software keyboard, or MIDI. |
| Analysis | Pitch, amplitude, duration, tempo, and spectrum. |
| Data matrix | Inputs × frequency bands × events. |
| Interpretation | Default, Probability, Euclide, Genetic, Kohonen, and Neural algorithms. |
| Rendering | SynthDef, buffers, envelope, buses, and effects. |
| Macro-form | Automation, presets, freeze, and global control. |

> **Central principle:** material, memory, decision, timing, and sound rendering are separate layers.

## 2. Flow Architecture

1. A source is placed on an analysis bus.
2. An analysis SynthDef produces an OSC message.
3. `sclang` validates and normalizes the event.
4. The vector is stored in general memory and one pitch band.
5. Each cell reads a live memory or a frozen copy.
6. The selected algorithm chooses or transforms values.
7. The timing mode schedules the event.
8. The SynthDef synthesizes or processes audio.
9. Internal buses, FX, and MasterFX route the result.

| Flow | Contents |
|---|---|
| Audio | Samples in buses and buffers. |
| Control | GUI values and control buses. |
| OSC | Vectors among server, language, and external applications. |
| MIDI | Notes, velocities, and channel. |
| Time | `TempoClock`, quantum, and `Tdef`. |

## 3. Initialization

At launch, Matrix configures the server, devices, memory, channels, and floating-point AIFF recording; initializes MIDI and UDP; loads SynthDefs; creates buses, groups, buffers, and VST controllers; then builds the GUI.

| Structure | Function |
|---|---|
| `groupeAnalyse` | Acquisition and analysis. |
| `groupeSynth` | Ordered Synth/FX cells. |
| `groupeMasterFX` | VST and final processing. |
| `listeBusInOut` | 32 internal audio buses. |
| `listeBusFX` | 32 dedicated effect buses. |
| `busOSC` | 13 control buses: global plus twelve bands. |

Group order guarantees server calculation order. Any processing chain must preserve producer-before-consumer placement.

## 4. Musical Vector

Each stored event contains frequency, amplitude, duration, tempo, spectral centroid, flatness, energy, and flux, plus an input identifier.

| Descriptor | Interpretation |
|---|---|
| Frequency | Pitch in Hz, commonly converted to MIDI. |
| Amplitude | Normalized level. |
| Duration | Time between events. |
| Tempo | Rhythmic reference. |
| Centroid | Spectral brightness. |
| Flatness | Noise-versus-tonality ratio. |
| Energy | Region of strong spectral energy. |
| Flux | Amount of spectral change. |

The first four fields directly drive musical generation. Spectral descriptors primarily feed automation and adaptation.

## 5. Validation and Filtering

An event is retained only when pitch, amplitude, and duration differences exceed `FhzFilt`, `AmpFilt`, and `DurFilt`, and when duration remains below `MaxDur`. HiPass, LoPass, Thresh, and Filter add analyzer-specific constraints.

| Filter | Role |
|---|---|
| `FhzFilt` | Minimum pitch difference in semitones. |
| `AmpFilt` | Minimum amplitude difference in dB. |
| `DurFilt` | Minimum duration. |
| `MaxDur` | Maximum duration. |
| `Memory` | Memory expiration. |
| `Data` | Circular-memory capacity. |

## 6. Band Matrix

Memory is organized first by audio input and then by pitch band. Band 0 stores all data; subsequent bands store frequencies within their bounds. When capacity is reached, writing becomes circular.

| Concept | Consequence |
|---|---|
| `NumFhzBand` | Orchestration granularity. |
| `RangeBand` | Custom register partitioning. |
| `indexDataMusic` | Circular write position. |
| `lastTimeBand` | Detects an inactive band. |
| `FreezeDataOSC` | Disconnects a cell from the live stream. |

Freeze turns a living memory into reusable stable material.

## 7. A Synth/FX Window as a Cell

Each window is an autonomous matrix cell. It owns a node group, input and band selection, algorithm, timing mode, two buffers, SynthDef, routing, and automation.

| State | Responsibility |
|---|---|
| `startStop` | Enables playback and routines. |
| `audioInID` | Selects a matrix row/input. |
| `flagIndexBand` | Selects pitch-band columns. |
| `menuAlgorithm` | Selects the decision logic. |
| `flagModeSynth` | Selects Tdef, OnFly, or Stream timing. |
| `groupe` | Carries all DSP controls for the cell. |

## 8. Algorithms

| Algorithm | Logic |
|---|---|
| Default | Direct or sequential reading. |
| Probability | Uses distributions, quartiles, and weighted draws. |
| Euclide | Distributes or compares events through a Euclidean rule/grid. |
| Genetic | Evolves frequency, amplitude, and duration. |
| Kohonen | Classifies by proximity in a self-organizing map. |
| Neural | Learns relationships among frequency, amplitude, and duration. |

Algorithms operate on musical data, not raw audio. The SynthDef remains independent from the decision rule.

## 9. Timing Modes

| Mode | Behavior |
|---|---|
| Tdef | Repetitive routine with calculated waits and clock synchronization. |
| OnFly1 | Plays when new data arrives. |
| OnFly2 | Alternative arrival-time transformation. |
| Stream1 | Continuously applies one calculated dimension. |
| Stream2 | Continuously applies another dimension or combination. |

`Quant` defines rhythmic resolution; `Stretch` multiplies duration. BPM may be neutral, analyzed, manually set, or received through OSC.

> Algorithm and timing mode are independent axes: **what to choose** and **when/how to play it**.

## 10. Synthesis and Processing

The library includes non-sample synthesis, samplers, granulation, one- and two-buffer FFT, pianos, time-domain effects, filters, and spectral processing. Each engine uses a subset of `ctrl1...ctrl12` plus two anti-click controls.

| Stage | Role |
|---|---|
| Buffers I/II | Recorded or loaded material. |
| Envelope | Articulation. |
| Musical Data | Event constraints. |
| Synthesizer Controls | Engine-specific parameters. |
| Audio buses | Generator chaining. |
| FX buses | Processing chaining. |
| MasterFX | Final gain and limiting. |

## 11. Matrix Routing

Thirty-two audio buses and thirty-two FX buses support serial or parallel networks. A cell can read Bus In, send through Send Bus, receive FX In, send through Send FX, and write to a physical output.

| Control | Effect |
|---|---|
| MoveBefore / MoveAfter | Changes calculation order. |
| Local In | Local re-injection. |
| Send Bus | Audio-network send level. |
| Send Bus FX | Effect-network send level. |
| Bypass | Removes physical output without necessarily deleting the node. |

A cyclic route or strong feedback can become unstable. Begin sends at very low levels and keep the limiter active.

## 12. Adaptive Automation

Matrix computes quartiles, median, dispersion, and skewness over memory. These statistics may move global pitch, duration, amplitude, pan, quantization, and root ranges.

| Automation | Source |
|---|---|
| Preset Probability | Detects atypical Flux and Flatness values. |
| Preset OnFly | Detects a change above a threshold. |
| Freq / Dur | Median and semi-interquartile range. |
| Transpose | Distribution skewness. |
| Root | Centroid converted to pitch class. |
| Auto% | Probability that adaptation is applied. |

Automation adapts configuration to the morphology of incoming material.

## 13. Presets and Reproducibility

A complete preset stores Synth/FX cells, the global panel, Master Sliders, OSC memory, frozen data, and node order. A Synthesizer file stores one cell and the state required to restore it.

| Object | Restored content |
|---|---|
| Preset | Complete orchestration. |
| Synthesizer | One cell. |
| OSCmusicData | Analyzed memory. |
| Freeze data | Stable cell-local copy. |
| Node order | Processing-chain order. |

The result remains partly variable when Probability, Genetic, Neural, or automation is active.

## 14. MIDI, OSC, and Synchronization

MIDI provides discrete notes and may also act as output. OSC synchronizes tempo, startup, and recording between master and slave applications. In master mode, Matrix can control Ardour transport.

| BPM mode | Source |
|---|---|
| BPM Nil | Neutral tempo. |
| BPM Algo | Tempo estimated by analysis. |
| BPM On | Manual setting. |
| BPM OSC | Tempo received from the master. |

## 15. System Analysis Method

1. Validate source and Display OSC.
2. Inspect the musical vector.
3. Check validation filters.
4. Verify distribution among bands.
5. Freeze a memory and compare it with the live stream.
6. Test Default in Tdef.
7. Change only the algorithm.
8. Change only the SynthDef.
9. Build routing and automation last.

This method separates perception, memory, decision, time, timbre, and routing.

## 16. Logical Diagnostics

| Symptom | Likely layer |
|---|---|
| No OSC message | Acquisition or analysis. |
| Messages but empty memory | Filters, duration, or `flagDataOSC`. |
| Memory but silence | Bands, Start state, timing mode, or routing. |
| Unchanged sound | Algorithm has no new data or Freeze is active. |
| Erratic automation | Memory too small or threshold too low. |
| Incomplete preset | Missing resources or sample paths. |
| Inconsistent tempo | Wrong BPM or OSC mode. |

Matrix Music can be summarized as: **analyze, filter, classify, memorize, choose, schedule, synthesize, route, and automate**.
