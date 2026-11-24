# Agents: Complete User Guide

**Getting started, GUI, musical logic, and system architecture**  
Edition: October 4, 2026

This manual combines practical instructions for musicians with an explanation of how the autonomous-agent system works. For numeric details, use the companion [GUI Reference](Agents_GUI_Complete_Control_Reference_EN.md). For system architecture, use [Operating Principles and System Logic](Agents_Operating_Principles_and_Logic_EN.md).

## 1. Agents in Two Minutes

Agents is an interactive generative-composition environment. It analyzes live audio, a file, MIDI, or keyboard input; converts detected events into pitch, amplitude, and duration data; and distributes them to autonomous agents. Each agent transforms and replays material according to its memory, genome, neighbors, position, and decision algorithm.

| Stage | Musical role | Main controls |
|---|---|---|
| Input | Microphone, file, MIDI, keyboard, or Off | Source, channel, threshold, analyzer. |
| Population | Agents listen, remember, learn, and play | Count, lifetime, proximity, mutation, sharing. |
| Instrument | Synthesis, sampling, granulation, or spectral processing | SynthDef, sample, register, duration, envelope, pan. |
| Processing | FX, reverb, and master output | Activation, macros, level, pan, automation. |
| Organization | Tempo, scale, presets, scores, and recording | BPM, bar, quantization, save/load, automation. |

Most two-state controls use **green for active** and **red for inactive**. When the label is ambiguous, trust the color and audible result.

### Recommended First Workflow

1. Choose a source in Control Panel.
2. Set a cautious MasterFX output level.
3. Initialize the population.
4. Start System, then Music.
5. Set Synthesizer before enabling Genes or Automation.
6. Save a preset as soon as the result becomes musically useful.

## 2. Quick Start Recipes

### A. Improvise with a Microphone

1. Launch Agents and wait for sounds and SynthDefs to load.
2. Select **Audio** as source.
3. Select the input channel used for analysis and the correct **Rec In** channel for live buffers.
4. In MasterFX, start around `LimitOut = -3 dB` and `PostAmp = 0 dB`.
5. Click **Init Agents**, then enable **System** and **Music**. Common shortcuts are `i`, `Space/Escape`, and `m`.
6. Play clearly articulated sounds. If too few events appear, lower the analysis threshold gradually.
7. Begin with a simple SynthDef, restricted register, and short durations.
8. Save a preset before enabling automation.

### B. Transform an Audio File

1. Choose **AgentsTools > File for Analyze > Load**.
2. Select **File** as input source.
3. Configure looping through **File for Analyze > Loop**.
4. Initialize and start Agents.
5. To restart on the temporal grid, use **Synchro on Temporal Grid** or `Ctrl+F`.

### C. Control Agents with MIDI

1. Connect the keyboard before launching the software.
2. Choose **AgentsTools > MIDI > Init**, then select the input port if required.
3. Select **MIDI** as source.
4. Play several notes, initialize the agents, and start the system.
5. To drive external hardware or software, enable MIDI Out and select a channel.

Start without Genes and Automation so that basic parameter effects remain audible.

## 3. Window Map

| Window | Immediate use | When to open |
|---|---|---|
| Control Panel | Start, source, analysis, tempo, population, global state | Always. |
| Synthesizer | Instrument, sample, register, duration, envelope, output | Sound design. |
| Effects / Reverb | Parallel processing and depth | After stabilizing the dry sound. |
| Activate Genes | Gives selected parameters to individual agents | Population diversity. |
| Automation | Evolves groups of settings automatically | Long forms. |
| Virtual Space | Displays signals and agents | Understanding interaction. |
| Control Virtual Space | Changes viewing camera only | Visual readability. |
| MasterFX | Limiter and final gain | Keep visible in performance. |
| Keyboard / VST | Computer-keyboard performance and VST hosting | As required. |

Shortcuts: `w` shows main windows; `Ctrl+W` uses an alternate layout; `v` toggles Virtual Space; `Alt+V` resets the space. The full shortcut panel is under **AgentsTools > ShortCuts**.

## 4. Control Panel

### Core Transport

| Control | Effect | Recommended use |
|---|---|---|
| System | Starts/stops the global process | First control on, last control off. |
| Music | Enables/disables agent sound production | Mute music without necessarily stopping analysis. |
| Init Agents | Rebuilds the population | Use after prolonged silence or major changes. |
| Loop Music | Repeats remembered sequences | Stabilize a pattern. |
| Virtual / Data | Shows space or internal data | Diagnostics. |
| Automation | Starts automatic evolution | Only after manual setup. |

### Source and Analysis

- **Audio:** analyzes live input.
- **File:** reads and analyzes the loaded file.
- **MIDI:** converts note and velocity messages into agent data.
- **Off:** stops feeding new data.

Analyzers include Onsets, Pitch, Pitch2, KeyTrack, and Keyboard. Threshold controls sensitivity. A setting that is too high produces few events; a setting that is too low creates over-detection and excessive density.

### Tempo and Grid

- **Tempo/BPM:** musical clock speed.
- **Beats per Bar:** measure length.
- **Quanta Music:** duration quantization resolution.
- **Tempo Analyze:** allows input to influence tempo.
- Metronome and counter indicate grid position and elapsed time.

## 5. Synthesizer Panel

This panel has the largest direct effect on sound while corresponding genes remain disabled.

| Control | Musical function | Safe starting point |
|---|---|---|
| SynthDef | Sampler, synthesis, granular, spectral, piano, etc. | SinOsc, PlayBuf, or another simple engine. |
| Sound | Sample used by sample-based SynthDefs | Short, recognizable sound. |
| Source Out | Output bus | Out1 in stereo. |
| Rec In | Channel captured into live buffers | Microphone/instrument channel. |
| Frequency | MIDI-note output range | 36–84 or 48–72. |
| Translate Frequency | Transposition | 0, then ±12. |
| Amplitude | Dynamic range | Leave headroom. |
| Duration | Event duration | 0.05–1.0 s. |
| Stretch Duration | Time compression/expansion | 1. |
| Pan | Stereo spread | -0.7 to +0.7. |
| Offset | Sample read position | 0 initially. |
| Loop / Reverse | Sample loop and reverse playback | Off initially. |
| SynthCtrl | Three SynthDef macros | 0.5, then explore individually. |
| NoClic | Anti-click parameters | Mid values, tested per SynthDef. |

The envelope contains eight levels and seven time segments. Keep the initial and final levels at zero, and avoid zero-length segments on high-energy sounds.

## 6. Population, Space, and Behavior

| Parameter | Audible consequence |
|---|---|
| Life | Population renewal rate. |
| Generation Speed | Ageing, mutation, and reproduction speed. |
| Maximum Agents | Density ceiling. |
| Maximum Children | Per-agent reproduction limit. |
| Distance Agents | Neighbor interaction range. |
| Distance Signals | Input-event listening range. |
| Deviance | Irregularity of movement/selection. |
| Mutation | Probability or intensity of new traits. |
| Crossing | Genome mixing during reproduction. |
| Learning | Trait copying from neighbors. |
| Birth | Birth probability. |
| Elitist | Favors better-adapted neighbors. |
| Shared Music | Shares musical fragments. |
| Flock | Moves toward collective behavior. |
| Torus | Connects opposite world edges. |

For readable textures, first reduce Maximum Agents and listening range. Then increase mutation or Shared Music gradually.

## 7. Activate Genes

When a musical gene is OFF, the global Synthesizer setting dominates. When it is ON, each agent uses an individual value within the chosen range, and the corresponding global control may become unavailable.

| Gene | Individual property | Musical effect |
|---|---|---|
| Frequency / Translate | Register and transposition | Heterogeneous polyphony. |
| Amplitude | Dynamic range | Agent-level contrast. |
| Duration / Stretch | Duration and time scale | Polymetric densities. |
| Pan | Stereo position | Animated spatialization. |
| Time Buffer | Live-buffer duration | Different memory spans. |
| Sample | Sample choice | Distributed orchestration. |
| Reverse / Loop / Offset | Buffer playback | Phrasing variation. |
| Envelope | Levels and times | Individual articulation. |
| Synthesizer / Controls | SynthDef and macros | Individual timbres. |
| Audio/MIDI Out | Output bus or channel | Distributed routing. |
| Buffer Music / Chord | Memory, size, chord window | Individual structures. |
| Algorithm | Transformation method | Different computational behavior. |

The `All` and `1–12` Fhz band buttons filter accessible pitch regions. Global and per-gene modes are mutually exclusive in the analyzed code. Enable one gene at a time and listen for at least one generation before adding another.

## 8. Effects, Reverb, and Automation

### Effects and Reverb Controls

| Control | Function |
|---|---|
| Type | Selects the processor. |
| Play On/Off | Enables the processor. |
| Source Out | Selects output routing. |
| Amp | Processing level. |
| Pan | Position or width. |
| Control 1–8 | Processor-specific macros. |
| Random Pan / Controls | Enables variation. |
| Jitter | Amount of random variation. |
| Speed | Change frequency. |

Effects receive a parallel bus and may feed reverb. Raise levels gradually to preserve headroom.

### Automation Families

- **Init:** rebuilds a silent population.
- **Preset/Universe:** loads states according to spectral change.
- **ControlA / ControlM:** evolves agent-world or musical-world parameters.
- **GenesA / GenesM:** enables/disables gene families.
- **SynthA / SynthM:** changes SynthDefs or musical ranges.
- **Root:** adapts the root when scale mapping is active.
- **Density:** controls automatic decision interval.
- **Source SoundIn/Agents:** selects statistics from input or agent output.

For long forms, start with one automation family and add a second only if the evolution remains intelligible.

## 9. Presets, Scores, and Recording

| Object | Contents | Shortcut |
|---|---|---|
| Control panel | General settings | `Shift+S`, then digit. |
| Preset | Musical state | `s`, then digit. |
| Preset + genome | State and genomes | `Alt+S`, then digit. |
| Preset + genome + sequence | State, genomes, sequences | `Alt+Shift+S`, then digit. |
| Timestamped preset | Complete dated preset | `Ctrl+T`. |
| Score | Sequence of GUI actions | `u` record; `Ctrl+U` stop. |

### Score Playback

- `p` + number: play once.
- `P` + number: loop.
- `Ctrl+Alt+P` + number: step mode, no loop.
- `Ctrl+Shift+Alt+P` + number: step mode, loop.
- `Enter`: next event in step mode.
- `Ctrl+Z`: stop all scores.

### Audio Recording

- `Alt+R`: start.
- `Shift+Alt+R`: pause/resume.
- `Ctrl+Alt+R`: stop.
- `Ctrl+S`: save the sound buffer currently used by Synthesizer.

The analyzed code prepares an AIFF floating-point file in SuperCollider's recordings directory. Always stop recording cleanly before quitting.

## 10. Essential Shortcuts

| Key | Action |
|---|---|
| Escape or Space | System on/off. |
| `i` | Initialize agents. |
| `m` | Music on/off. |
| `h` | Cycle Audio/File/MIDI/Off source. |
| `q` | Change analyzer. |
| `b` | Loop Music on/off. |
| `e` | Elitist mode. |
| `j` | Torus on/off. |
| `Shift+J` | Flock on/off. |
| `Alt+J` | Shared Music on/off. |
| `o` | Automation on/off. |
| `v` | Virtual Space on/off. |
| `a` | Data display on/off. |
| `w` | Show windows. |
| `z` | Load a random preset. |
| `Ctrl+N` | Add one agent. |
| `Alt+R` | Start audio recording. |

Musical keyboard keys: `y s x d c v g b h n j m , l . e -`. Use `<` and `>` for downward/upward transposition.

## 11. Three Musical Strategies

### Stable, Playable Texture

- Maximum six agents.
- Low mutation; Shared Music OFF.
- One SynthDef, narrow register, short events.
- Enable Loop Music after a useful motif appears.
- Light reverb; no random FX.

### Evolving Ecosystem

- Eight to twelve agents.
- Medium lifetime; moderate Learning and Crossing.
- Enable Flock and Torus.
- Enable two or three genes, such as Pan, Synthesizer, and Duration.
- Limit automation to GenesA or ControlA.

### Live Transformation

- Use Audio source and a SynthDef that reads a live buffer.
- Match Time Buffer to the performer's gesture.
- Set threshold to capture useful attacks.
- Explore Loop, Reverse, and Offset progressively.
- Use spectral FX or delay in parallel while monitoring MasterFX.

Before performance, create a **rescue preset** with a reliable SynthDef, few agents, no automation, and controlled level.

## 12. Troubleshooting

| Symptom | Probable causes | Action |
|---|---|---|
| No sound | System/Music inactive, wrong output, no detected signal | Check transport, Source Out, MasterFX, and Signal IN. |
| Signal IN remains zero | Wrong source/channel, threshold too high, MIDI disconnected | Change source/channel, lower threshold, reinitialize MIDI. |
| Sound too dense | Threshold too low, too many agents, broad loop/chord settings | Raise threshold; reduce Max Agents, Chord Max, and Loop Music. |
| Clicks | Abrupt envelope, unstable buffer/offset, sensitive SynthDef | Lengthen edge times, keep initial/final levels at zero, adjust NoClic. |
| Dangerous level | Agent + FX + reverb accumulation | Reduce Amp, FX level, and PostAmp; keep limiter enabled. |
| Controls move by themselves | Automation active | Disable Automation and subfamilies. |
| A control is disabled | Corresponding gene owns the parameter | Disable that gene. |
| Preset not found | Wrong directory or naming | Check data directory and `preset`/`Preset` naming. |
| MIDI stuck | Notes not released or wrong channel | Toggle MIDI Out, initialize MIDI, and send/reset All Notes Off. |
| Windows scattered | Moved or hidden windows | Use `w` / `Ctrl+W`; keep MasterFX in front. |

### Recommended Shutdown Order

1. Stop audio or score recording.
2. Disable Music.
3. Disable System.
4. Save the preset/control panel if required.
5. Quit, or use `Cmd+Period` in SuperCollider.

## 13. Technical Notes

- Default population: 6 agents; initial maximum: 12.
- Analyzed events are normalized as frequency, amplitude, duration, and identifier.
- Algorithm families include Default, Probability, Euclide, Genetic, Kohonen, and Neural.
- Output formats include Stereo, MultiSpeaker, Rotate2, and Ambisonic.
- The Soft menu opens genome, sequence, score, and live-coding editors.
- OSC can operate as Off, Master, or Slave to synchronize tempo, start, bar, and recording.
- Synth, sound, effect, and reverb lists are loaded from the data directory and can therefore be customized.

## 14. Internal Logic Summary

Agents follows this cycle:

```text
source -> analysis -> normalized signal -> virtual space
       -> agent perception -> memory -> decision algorithm
       -> genome/global parameters -> SynthDef -> FX/Reverb/MasterFX
       -> population learning, mutation, reproduction, and automation
```

A preset restores settings, but exact rendering also depends on genomes, sequences, loaded sounds, SynthDefs, stochastic choices, and external resources. For archiving, preserve all of these elements together.
