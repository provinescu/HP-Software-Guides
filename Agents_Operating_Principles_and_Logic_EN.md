# Agents: Operating Principles and System Logic

**Architecture, autonomous agents, genomes, and musical data flow**  
Version: October 4, 2026

> **Purpose:** This guide explains how Agents turns audio or MIDI input into a population of virtual musicians that can listen, remember, move, learn, reproduce, and generate music. It is based on analysis of the supplied Agents source code. Exact behavior may also depend on complementary classes, SynthDefs, resource lists, and presets installed in the Agents data directory.

## 1. What Agents Is

Agents is a real-time generative composition environment built in SuperCollider. It combines musical analysis, autonomous agents, a virtual world, parameter genomes, multiple synthesis engines, effects, a shared clock, presets, scores, and recording. The result is not determined by a fixed score. It emerges from the interaction of incoming data, memory, spatial proximity, individual parameters, and evolutionary rules.

| Layer | Role |
|---|---|
| Source | Live audio, file, MIDI, computer keyboard, or no new input. |
| Analysis | Extracts onsets, pitch, amplitude, duration, key information, and tempo. |
| Signals | Represents musical events inside the virtual space. |
| Agents | Listen, remember, select, transform, and play. |
| Genome | Stores values specific to each agent. |
| Population | Handles ageing, birth, mutation, crossover, and learning. |
| Synthesis | Converts decisions into sound. |
| Organization | Tempo, grid, scale, automation, presets, and scores. |

> **Central principle:** `perception -> memory -> decision -> action -> evolution`

## 2. General Architecture

1. A source produces audio or MIDI notes.
2. An analyzer converts the source into normalized events.
3. Each event becomes a signal positioned in the virtual space.
4. Nearby agents perceive the signal if their listening range allows it.
5. Each agent stores, filters, or transforms data in its musical memory.
6. A decision algorithm selects the material to play.
7. Global settings and the genome determine pitch, duration, amplitude, timbre, and routing.
8. A SynthDef creates sound; effects, reverb, and MasterFX process it.
9. In parallel, agents age, learn, mutate, share material, or reproduce.

| Flow | Contents |
|---|---|
| Audio | Continuous signals in buses and buffers. |
| Events | Frequency, amplitude, duration, identifier, and time. |
| Spatial | Positions, distances, attraction, and movement. |
| Genetic | Individual values that can be inherited or mutated. |
| Temporal | `TempoClock`, bar, quantum, and synchronization. |
| Control | GUI, MIDI, OSC, automation, presets, and scores. |

## 3. Initialization and Internal Startup

The normal startup sequence is:

1. Validate the Agents resource folder and SuperCollider recording folder.
2. Configure audio devices, input/output counts, memory, hardware buffer, and recording channels.
3. Open UDP ports and prepare OSC.
4. Load SynthDefs, sounds, effects, reverbs, and resource lists.
5. Create buses, groups, buffers, and state variables.
6. Create `TempoClock` and the musical grid.
7. Install MIDI handlers and keyboard shortcuts.
8. Build the GUI panels.
9. Initialize the population and start runtime routines when **System** is enabled.

| Creation parameter | Meaning |
|---|---|
| `path` | Agents resource directory. |
| `ni / o / r` | Input, output, and recording-channel counts. |
| `devIn / devOut` | Audio devices. |
| `size` | Hardware buffer size; 256 in the analyzed code. |
| `wid / ori / flag` | Multichannel output parameters. |
| `scPort` | SuperCollider server port. |

A device or channel-count change normally requires a server restart.

## 4. From Source to Musical Signal

| Analyzer | Extracted information | Typical use |
|---|---|---|
| Onsets | Attacks and transients | Rhythm, percussion, articulated gestures. |
| Pitch | Fundamental frequency | Monodic lines. |
| Pitch2 | Alternative pitch method | Comparison for different timbres. |
| KeyTrack | Tonal region or spectral pitch class | Harmonic context. |
| Keyboard | Note, velocity, and duration | Deterministic keyboard/MIDI control. |
| Tempo | Estimated pulse | Clock adaptation. |

Values are converted into a common representation so that microphone, file, keyboard, and MIDI events can be processed consistently. The analysis threshold determines whether an event is created. Register limits and filters reject unwanted data.

- Threshold too low: false or excessive events fill the world.
- Threshold too high: agents receive too little material.

The full system depends on the quality of this first conversion.

## 5. The Virtual Space

The virtual space is not only a visualization. It is an interaction model. Signals and agents have positions; an event or neighbor is perceived only when distance and attraction rules allow it.

| Element | Function |
|---|---|
| Distance Signals | Range at which an agent perceives events. |
| Distance Agents | Interaction range between neighbors. |
| Attraction Signals | Motion toward musical material. |
| Attraction Agents | Motion toward other population members. |
| Flock | Tendency to follow a collective direction. |
| Deviance | Irregularity added to motion or decisions. |
| Torus | Joins opposite boundaries, creating a continuous world. |

### Display Colors

- Red: incoming signal.
- White: agent currently playing.
- Blue: young agent.
- Green: adult agent.
- Yellow: old agent.
- Magenta: attraction toward neighbors.
- Cyan: attraction toward signals.

## 6. Agent Life Cycle

| Phase | Behavior |
|---|---|
| Birth | An agent is created with a genome and position. |
| Youth | It begins perceiving and building memory. |
| Adulthood | It plays, learns, shares, and may reproduce. |
| Ageing | Age progresses according to generation speed. |
| Reproduction | Genomes cross over and may mutate. |
| Death | The agent is removed when its lifetime is reached. |

`Maximum Agents` limits population size. `Maximum Children` limits individual reproduction. `Birth` sets birth probability; `Crossing` combines inherited values; `Mutation` generates differences; `Learning` copies traits from neighbors; `Elitist` favors agents considered better adapted.

Short lifetimes plus strong mutation produce rapid stylistic change. Long lifetimes plus low mutation stabilize the ecosystem.

## 7. Genome and Global Parameters

Every musical dimension can be global or individual. When a gene is disabled, agents share the GUI value. When enabled, each agent receives its own value within the selected range.

| Gene family | Individualized property |
|---|---|
| Frequency / Translate | Register and transposition. |
| Amplitude | Dynamics. |
| Duration / Stretch | Event duration and time scale. |
| Pan | Spatial position. |
| Sample / Synthesizer | Sample and sound engine. |
| Envelope / Controls | Articulation and SynthDef macros. |
| Loop / Reverse / Offset | Buffer playback. |
| Buffer / Chord | Memory size and harmonic grouping. |
| Audio/MIDI Out | Routing. |
| Algorithm | Musical decision rule. |

The genome is heritable. Mutation changes values, crossing combines two parents, and learning may copy a neighbor. Sound parameters therefore become evolutionary traits.

Enable genes gradually. If too many parameters become individual at once, musical causality becomes hard to hear.

## 8. Memory, Chords, and Musical Sharing

| Mechanism | Role |
|---|---|
| Buffer Music | Musical events available to the agent. |
| MaxDatas | Maximum number of stored data items. |
| Chord | Maximum simultaneous events. |
| ChordDur | Time window used to group events into a chord. |
| Shared Music | Transfers fragments between agents. |
| Loop Music | Repeats remembered structures. |
| Root / Scale | Rebuilds pitches around a root and scale. |

Memory links perception and composition. An agent may select older events, combine them, transpose them, share them, or repeat them. Short memory closely follows the input. Long memory introduces reminiscence and gradually separates the output from the source.

## 9. Decision Algorithms

| Family | General logic | Expected result |
|---|---|---|
| Default | Baseline selection rules | Reference behavior. |
| Probability | Weighted or random selection | Controlled variation. |
| Euclide | Distributes events over a grid | Regular and asymmetric rhythms. |
| Genetic | Selection and evolution of solutions | Generational transformation. |
| Kohonen | Similarity-based organization | Clusters of related data. |
| Neural | Learning and prediction | Material-dependent responses. |

The algorithm does not directly create sound. It selects content or a transformation. The genome supplies parameter values, and the SynthDef produces the timbre.

## 10. From Decision to Sound

| Stage | Function |
|---|---|
| Selection | The agent chooses one or more data items. |
| Transformation | Register, scale, duration, and genes are applied. |
| SynthDef | Sound is instantiated on the audio server. |
| Envelope | Shapes articulation and reduces discontinuities. |
| Routing | Selects stereo, multichannel, MIDI, or VST output. |
| FX / Reverb | Parallel or spatial processing. |
| MasterFX | Final gain and limiting. |

Synthesis families include PlayBuf/BufRd/LoopBuf readers, live buffers, TGrains and GrainBuf granulation, sinusoidal synthesis, resonators, piano models, PV spectral processing, convolution, filters, pitch shifting, freeze, live effects, VST hosting, and multichannel output.

> The event describes **what** to play. The genome describes **with which values**. The SynthDef describes **how** the sound is produced.

## 11. Tempo, Grid, and Organization

| Concept | Function |
|---|---|
| TempoClock | Shared time reference. |
| BPM | Global speed. |
| Beats per Bar | Pulses per measure. |
| Quanta Music | Quantization resolution. |
| Synchro | Delays a command until the next grid point. |
| Tempo Analyze | Allows the source to influence tempo. |
| Metronome | Shows the current grid position. |

Agents may generate independently while sharing the same clock. Synchronization does not remove autonomy: it defines when actions may occur. An apparently late action may be intentionally waiting for the next quantum or bar.

## 12. Automation and Large-Scale Evolution

Automation operates above individual agents and creates macro-form.

| Module | Automated behavior |
|---|---|
| Init | Rebuilds the population when required. |
| Preset / Universe | Changes global state according to analysis. |
| ControlA / ControlM | Evolves agent-world or musical-world parameters. |
| GenesA / GenesM | Switches gene families. |
| SynthA / SynthM | Changes SynthDefs or musical ranges. |
| Root | Changes the root and harmonic adaptation. |
| Density | Controls decision interval or frequency range. |
| Source SoundIn / Agents | Selects the monitored statistical material. |

Agents create local activity; automation creates large-scale form. Start with one automation family because multiple automated layers can mask each other.

## 13. Presets, Scores, and Reproducibility

| Object | Preserved material | Limitation |
|---|---|---|
| Preset | Musical state and parameters | Depends on external resources. |
| Genome | Individual traits | May evolve after loading. |
| Sequence | Agent musical memory | Rendering depends on the SynthDef. |
| GUI Score | Actions and timing | Randomness may alter the result. |
| Audio | Final rendered sound | Session cannot be edited from it. |
| Buffer | Reusable sound material | Must be saved explicitly. |

Archive presets, genomes, sequences, sounds, SynthDefs, scores, and configuration data together. Agents offers controlled reproducibility, not necessarily sample-identical repetition.

## 14. Recommended Exploration Method

1. Begin with Audio, File, or MIDI and validate analysis.
2. Create four to six agents.
3. Select a simple SynthDef and restrict the register.
4. Observe the link between signal distance and activity.
5. Set memory, chord grouping, and event duration.
6. Enable one gene, such as Pan or Duration.
7. Introduce low mutation and learning.
8. Add Shared Music or Flock.
9. Enable one automation family.
10. Save the preset, genomes, and sequences before major changes.

| Musical regime | Dominant settings |
|---|---|
| Reactive imitation | Short memory, low mutation, high signal proximity. |
| Stable ecosystem | Long lifetime, moderate learning, few active genes. |
| Rapid evolution | Short lifetime, high mutation/crossing, active automation. |

## 15. Layer-by-Layer Diagnostics

| Symptom | Likely layer | Test |
|---|---|---|
| No signal | Source/device | Check input, channel, and analyzer. |
| Signals but inactive agents | Distance/population | Increase range or reinitialize agents. |
| Active agents but silence | Synthesis/routing | Test a simple SynthDef and Out1. |
| Texture too dense | Threshold/population/chord | Raise threshold; reduce agents and Chord. |
| Unpredictable result | Genes/mutation/automation | Disable one layer at a time. |
| Saturated sound | Levels/FX/MasterFX | Reduce upstream levels and monitor the limiter. |
| Clicks | Envelope/buffer/SynthDef | Lengthen edge times and adjust NoClic. |
| Preset sounds different | Evolving population/resources | Reload genome and sequences; verify sounds. |

## Summary

Agents listens to a source, converts it to signals, lets a population perceive those signals according to distance, stores and transforms events, assigns parameters either globally or genetically, creates sound through SynthDefs, and evolves both the population and the macro-form.
