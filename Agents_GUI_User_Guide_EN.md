# Agents: GUI User Guide

**Quick Start for Musicians and Performers**  
Guide version: October 4, 2026

> **Purpose:** Translate the interface into concrete musical actions without requiring SuperCollider programming knowledge. Based on analysis of `Agents - copie.txt`.

## 1. Agents in Two Minutes

Agents is an interactive generative-composition environment. It analyzes musical input, converts detected events into pitch, amplitude, and duration data, then distributes them to a population of agents. Every agent transforms and replays material according to its parameters, genes, neighborhood, and selected algorithm.

| Stage | Musical role | What you control |
|---|---|---|
| Input | Microphone, audio file, MIDI, or no input | Source, channel, threshold, analyzer. |
| Population | Agents listen, remember, learn, and play | Count, lifetime, proximity, mutation, sharing. |
| Instrument | Synthesis, sampling, or spectral processing | Synth, sample, register, duration, envelope, pan. |
| Processing | FX, reverb, and master output | Activation, parameters, level, pan, automation. |
| Organization | Tempo, scale, presets, score, recording | BPM, bar, quantization, save, automation. |

For two-state buttons, green generally means active and red inactive. Trust the color and audible result when the text is ambiguous.

### Recommended Workflow

1. Select a source in Control Panel.
2. Set cautious listening level in MasterFX.
3. Initialize the population.
4. Start System, then Music.
5. Configure Synthesizer before enabling Genes or Automation.
6. Save a preset when a useful musical state appears.

## 2. Quick Start Recipes

### A. Improvise with a Microphone

1. Launch Agents and wait for sounds and SynthDefs to load.
2. Choose Audio as source.
3. Choose the analysis input and, when using recorded buffers, the appropriate Rec In channel.
4. Start around `LimitOut = -3 dB` and `PostAmp = 0 dB`.
5. Click Init Agents, then enable System and Music. Common shortcuts are `i`, Space/Escape, and `m`.
6. Play clearly articulated sounds. Lower the analysis threshold gradually if too few events appear.
7. Begin with a simple SynthDef, restricted register, and short durations.
8. Save a preset before enabling automation.

### B. Transform an Audio File

1. Open **AgentsTools > File for Analyze > Load**.
2. Choose File as input source.
3. Enable or disable looping through **File for Analyze > Loop**.
4. Initialize and start Agents.
5. Use **Synchro on Temporal Grid** or `Ctrl+F` to restart exactly on the grid.

### C. Control Agents with MIDI

1. Connect the keyboard before launching the software.
2. Open **AgentsTools > MIDI > Init**, then select the input port if needed.
3. Choose MIDI as source.
4. Play several notes, initialize the agents, and start the system.
5. Enable MIDI Out and choose a channel to control an external synthesizer.

Start without Genes or Automation so the effect of basic settings remains clear.

## 3. Window Map

| Window | Immediate use | When to open |
|---|---|---|
| Control Panel | Startup, source, analysis, tempo, population, global state | Always. |
| Synthesizer | Instrument, sample, register, duration, envelope, output | Sound design. |
| Effects / Reverb | Parallel processing and space | After stabilizing the dry sound. |
| Activate Genes | Gives selected parameters to individual agents | Population diversity. |
| Automation | Evolves groups of settings automatically | Long forms. |
| Virtual Space | Shows signals and agents | Understanding interactions. |
| Control Virtual Space | Changes only the 3D view | Readability. |
| MasterFX | Limiter and final gain | Keep visible in performance. |
| Keyboard / VST | Computer-key performance and VST hosting | As required. |

- `w`: show main windows.
- `Ctrl+W`: alternate window layout.
- `v`: show/hide Virtual Space.
- `Alt+V`: reset Virtual Space.
- Full shortcuts: **AgentsTools > ShortCuts**.

## 4. Control Panel

### Transport and Essential States

| Control | Effect | Recommended use |
|---|---|---|
| System | Starts/stops the global process | First on, last off. |
| Music | Enables/disables musical production | Mute music without necessarily stopping analysis. |
| Init Agents | Rebuilds the population | After prolonged silence or major changes. |
| Loop Music | Repeats remembered sequences | Stabilize a motif. |
| Virtual / Data | Shows space or internal data | Diagnostics. |
| Automation | Starts automatic evolution | Only after manual setup. |

### Source and Analysis

- **Audio:** analyzes live input.
- **File:** reads and analyzes a loaded file.
- **MIDI:** converts note and velocity messages into agent data.
- **Off:** stops new data from entering.

Analyzers include Onsets, Pitch, Pitch2, KeyTrack, and Keyboard. Threshold controls sensitivity. Too high means almost no signal; too low creates over-detection and dense textures.

### Tempo and Grid

- Tempo/BPM sets the musical clock.
- Beats per Bar sets measure length.
- Quanta Music rounds durations to a grid. Higher values allow finer resolution.
- Tempo Analyze lets the signal influence tempo.
- Metronome and counter show bar position and elapsed time.

## 5. Synthesizer Panel

This panel has the strongest direct influence on sound while the corresponding genes remain disabled.

| Control | Musical function | Starting point |
|---|---|---|
| SynthDef | Sampler, synthesis, granular, spectral, piano, etc. | SinOsc, PlayBuf, or another simple engine. |
| Sound | Sample used by sample-based engines | Short, recognizable sound. |
| Source Out | Output bus | Out1 in stereo. |
| Rec In | Channel captured into live buffers | Microphone/instrument channel. |
| Frequency | MIDI-note output range | 36–84 or 48–72. |
| Translate Frequency | Transposition | 0, then ±12. |
| Amplitude | Dynamic range | Preserve substantial headroom. |
| Duration | Event duration | 0.05–1.0 s. |
| Stretch Duration | Time compression/expansion | 1. |
| Pan | Stereo spread | -0.7 to +0.7. |
| Offset | Sample read position | 0 initially. |
| LoopP / Reverse | Sample loop and reverse playback | Off initially. |
| SynthCtrl | Three SynthDef macros | 0.5, then explore one by one. |
| NoClic | Anti-click parameters | Mid values, tested per SynthDef. |

The envelope contains eight levels and seven time segments. Keep the first and final levels at zero and avoid zero-length segments on high-energy sounds.

## 6. Population, Space, and Behavior

| Parameter | Audible consequence |
|---|---|
| Life | Population renewal rate. |
| Generation Speed | Ageing, mutation, and reproduction speed. |
| Maximum Agents | Population-density ceiling. |
| Maximum Children | Per-agent reproduction limit. |
| Distance Agents | Neighbor interaction range. |
| Distance Signals | Listening range for analyzed events. |
| Deviance | Irregularity of movement. |
| Mutation | Probability of new traits. |
| Crossing | Genome mixing during reproduction. |
| Learning | Trait copying from neighbors. |
| Birth | Birth probability. |
| Elitist | Favors better-adapted neighbors. |
| Shared Music | Shares musical fragments. |
| Flock | Moves toward collective behavior. |
| Torus | Connects opposite world boundaries. |

### Virtual Space Colors

- Red: incoming signals.
- White: agent currently playing.
- Blue: young agent.
- Green: adult agent.
- Yellow: old agent.
- Magenta: attraction toward neighbors.
- Cyan: attraction toward signals.

For a readable texture, first reduce Maximum Agents and listening range. Then increase Mutation or Shared Music gradually.

## 7. Activate Genes

When a musical gene is disabled, the global Synthesizer setting dominates. When enabled, each agent uses its own value within the selected range, and the corresponding global control is usually disabled.

| Gene | Individual property | Musical effect |
|---|---|---|
| Frequency / Translate | Register and transposition | More heterogeneous polyphony. |
| Amplitude | Dynamic range | Contrast among agents. |
| Duration / Stretch | Duration and time scale | Polymetric densities. |
| Pan | Stereo location | Animated spatialization. |
| Time Buffer | Live-buffer length | Different memory spans. |
| Sample | Sample selection | Distributed orchestration. |
| Reverse / Loop / Offset | Buffer playback | Phrasing variation. |
| Envelope | Levels and times | Individual articulation. |
| Synthesizer / Controls | SynthDef and macros | Individual timbres. |
| Audio/MIDI Out | Output bus/channel | Distributed routing. |
| Buffer Music / Chord | Memory and chord window | Individual structures. |
| Algorithm | Transformation method | Different computational behavior. |

Band buttons All and 1–12 filter accessible pitch regions. Global and per-gene modes are mutually exclusive. Enable one gene at a time and listen for at least one generation before adding another.

## 8. Effects, Reverb, and Automation

### Effects and Reverb

| Control | Function |
|---|---|
| Type | Selects effect or reverb. |
| Play On/Off | Enables processor. |
| Source Out | Selects output. |
| Amp | Processing level. |
| Pan | Position or width. |
| Control 1–8 | Processor-specific macros. |
| Random Pan / Controls | Enables variation. |
| Jitter | Random-variation amount. |
| Speed | Change frequency. |

Effects receive a parallel bus and may feed the reverb bus. Raise their level gradually.

### Automation Families

- Init: rebuilds the population when it stops producing music.
- Preset/Universe: loads presets from spectral changes.
- ControlA / ControlM: evolves the agent world or musical world.
- GenesA / GenesM: enables/disables gene families.
- SynthA / SynthM: changes the synth or musical ranges.
- Root: adapts the root when scale mapping is active.
- Density: controls automatic-decision speed range.
- Source SoundIn/Agents: selects statistics from input or agent output.

For long forms, start with one automation family. Add a second only if the evolution remains intelligible.

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
- Enter: next event in step mode.
- `Ctrl+Z`: stop all scores.

### Audio Recording

- `Alt+R`: start.
- `Shift+Alt+R`: pause/resume.
- `Ctrl+Alt+R`: stop.
- `Ctrl+S`: save the sound buffer currently used in Synthesizer.

Stop recording cleanly before quitting so the AIFF file is finalized.

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
| `z` | Load random preset. |
| `Ctrl+N` | Add one agent. |
| `Alt+R` | Start audio recording. |

Musical keys: `y s x d c v g b h n j m , l . e -`. Use `<` and `>` for downward/upward transposition.

## 11. Three Musical Strategies

### Stable, Playable Texture

- Maximum six agents.
- Low mutation; Shared Music off.
- One SynthDef, narrow register, short events.
- Enable Loop Music after a useful motif appears.
- Light reverb; no random FX.

### Evolving Ecosystem

- Eight to twelve agents.
- Medium lifetime; moderate Learning and Crossing.
- Enable Flock and Torus.
- Enable only two or three genes, such as Pan, Synthesizer, and Duration.
- Limit automation to GenesA or ControlA.

### Live Transformation

- Audio source and a SynthDef that reads a live buffer.
- Time Buffer matched to the performer's gesture.
- Threshold adjusted for useful attacks.
- Loop, Reverse, and Offset explored progressively.
- Spectral FX or delay in parallel, monitored in MasterFX.

Before a concert, create a rescue preset with one reliable synth, few agents, no automation, and controlled level.

## 12. Quick Troubleshooting

| Symptom | Probable causes | Action |
|---|---|---|
| No sound | System/Music inactive, wrong output, no detected signal | Check transport, Source Out, MasterFX, Signal IN. |
| Signal IN stays at zero | Wrong source/channel, threshold too high, MIDI disconnected | Change source/channel, lower threshold, reinitialize MIDI. |
| Sound too dense | Threshold too low, too many agents, broad loop/chord settings | Raise threshold; reduce Max Agents, Chord Max, Loop Music. |
| Clicks | Abrupt envelope, unstable buffer/offset, sensitive SynthDef | Lengthen edge times, keep endpoint levels at zero, adjust NoClic. |
| Dangerous level | Agent + FX + reverb accumulation | Reduce Amp, FX level, PostAmp; keep limiter enabled. |
| Settings move alone | Automation active | Disable Automation and subfamilies. |
| A control is disabled | Corresponding gene owns the parameter | Disable that gene. |
| Preset not found | Wrong name/folder | Check data directory and `preset`/`Preset` names. |
| MIDI stuck | Notes not released or wrong channel | Toggle MIDI Out, initialize MIDI, reset All Notes Off. |
| Windows scattered | Moved/hidden windows | Use `w` / `Ctrl+W`; keep MasterFX in front. |

Recommended shutdown: stop recording, turn off Music, turn off System, save if needed, then quit or use Cmd+Period.
