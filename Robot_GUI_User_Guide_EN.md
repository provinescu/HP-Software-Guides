# Robot: GUI User Guide

**Quick Start for Musicians and Performers**  
Version: October 4, 2026

> **Purpose:** This manual presents Robot as a musical instrument. It explains what to adjust, in which order, and how to obtain a reliable result without reading the SuperCollider source. Based on analysis of `Robot - copie.txt`.

## 1. Robot in Two Minutes

Robot is a multivoice music-creation environment. Audio input, an audio file, the computer keyboard, or MIDI feeds analyzers. Detected events are played by one or more instruments, synchronized to a grid, transformed by algorithms, and processed by effects.

| Zone | Purpose | First action |
|---|---|---|
| General panel | Startup, source, analysis, tempo, global processes | Choose AudioIn or FileIn, then start. |
| Instrument windows | Sound, memory, algorithms, synthesis, and effects for each voice | Enable one instrument only. |
| MasterFX | Limiter and final output gain | Preserve safety headroom. |
| Keyboard | Computer-keyboard performance | Set Translate and Volume. |
| VST | External instrument or effect | Open only after internal sound is working. |

Learn Robot with one source, one instrument, and no effects. Add layers one at a time.

### Practical Color Code

- Green/red buttons generally indicate active/inactive states.
- Yellow headers separate major control families.
- Gray menus select sources, algorithms, synthesizers, or files.
- Disabled controls are unavailable in the current state or controlled elsewhere.

## 2. Quick Start

### With a Microphone or Audio Input

1. Launch Robot and wait for SynthDefs and sounds to load.
2. In MasterFX, set `LimitOut` around -3 dB and `PostAmp` near 0 dB.
3. Choose AudioIn in the general panel.
4. Select the input channel from AudioIn 1–32.
5. Choose Onsets for a rhythmic test, or Pitch for a monophonic line.
6. Start System.
7. Open one instrument, enable playback, and select SinOsc or PlayBuf.
8. Play several distinct sounds and adjust the analysis threshold.
9. When behavior is stable, enable synchronization or record a sequence.

### With an Audio File

1. Load the analysis file from the menu.
2. Choose FileIn.
3. Choose the analyzer, then start System.
4. Enable one instrument and confirm that it receives events.
5. Use loop and synchronization only after the first successful test.

> If the meters move but Robot does not play, the problem is probably in analysis or the instrument, not the audio interface.

## 3. GUI Map

| Window or zone | Function | When to keep visible |
|---|---|---|
| Control / general panel | Global state, input, analysis, tempo, and processes | Always. |
| Instrument 1…n | Voice-specific settings | While editing that voice. |
| MasterFX | Final limiting and level | Rehearsal and concert. |
| Keyboard | Computer keyboard | When Keyboard is the source. |
| VST | Plug-in, program, output, and mix | When using a plug-in. |
| Shortcuts | Keyboard command list | During learning. |

### Instrument Window Organization

- **SYSTEM PARAMETERS:** behavior of the system for this instrument.
- **FILTER ANALYSE:** criteria used to select events.
- **ALGORITHM PARAMETERS:** Chaos, Neurone, Genetic, and other engines.
- **SYNTHESIZER:** samples, sound engine, envelope, level, and panning.
- **Pre FX / Post FX:** processing before and after the main sound.
- **Sequence / Recording:** capture and playback of performance data.

## 4. General Panel

| Control | Musical function | Recommendation |
|---|---|---|
| System | Starts/stops the global engine | Enable after output setup. |
| AudioIn / FileIn | Selects the analyzed source | Validate one source at a time. |
| MIDI | Enables MIDI command chain | Connect the device before launch. |
| Onsets / Pitch / Pitch2 / KeyTrack / Keyboard | Selects what Robot extracts | Onsets is the simplest test. |
| Tempo Analyze | Allows the source to influence tempo | Leave Off initially. |
| Metronome | Shows temporal position | Use to verify synchronization. |
| Load Instrument | Loads one voice | Build a set progressively. |
| Load Instruments | Loads a group of voices | Recall an orchestration. |
| OSC Off/On | Shows network-connection state | Ignore without an external setup. |

### Global Processes

- **ChaosIn:** produces variation from a chaotic system.
- **NeuroneIn:** uses a neural engine and learning parameters.
- **GenetiqueIn:** evolves data or parameters.
- **Algorithm:** applies selection and transformation rules.
- **Automation:** changes settings automatically.

Every process has a speed. Higher values mean more decisions or changes. Do not enable all processes on the first run. Start with Algorithm only.

## 5. Sources and Sound Analysis

| Mode | Best use | Limitation |
|---|---|---|
| Onsets | Percussion, attacks, articulated playing | Does not describe pitch precisely. |
| Pitch | Stable monophonic voice or instrument | May hesitate on chords and noise. |
| Pitch2 | Alternative pitch tracking | Compare according to the source. |
| KeyTrack | Harmonic color or tonal area | Requires a sufficiently rich spectrum. |
| Keyboard | Deterministic keyboard/MIDI playing | Does not directly analyze audio. |

### Analysis Filter

- Choice Filter selects the pre-analysis filter.
- HzPass sets cutoff.
- High-pass removes hum and unwanted low frequencies.
- Low-pass reduces highs or unwanted attacks.
- Keep enough level after filtering.

### Fast Threshold Setup

1. Produce the quietest sound that should be detected.
2. Lower the threshold until an event appears.
3. Stop playing and check that silence does not trigger continuously.
4. Raise the threshold slightly if silence still produces events.

## 6. SYSTEM PARAMETERS for Each Instrument

| Control | Audible effect | Cautious use |
|---|---|---|
| Chaos Fhz | Irregularity applied to pitch | Small value preserves the contour. |
| Chaos Amp | Dynamic variation | Avoid extremes with many voices. |
| Chaos Dur | Duration variation | Raise for more unstable textures. |
| Play / Stop | Enables/disables the voice | Test before adding effects. |
| Input / Output | Selects instrument buses | Check during silence diagnosis. |
| Record | Arms instrument capture | Stop cleanly after the take. |

System parameters do not directly create timbre. They determine how data reaches the synthesizer.

## 7. FILTER ANALYSE for Each Instrument

| Control | Function | Musical interpretation |
|---|---|---|
| Dist Freq | Accepted pitch difference | Higher means more distinct events. |
| Dist Amp | Accepted amplitude difference | Higher means stronger dynamic contrast. |
| Dist Dur | Accepted duration difference | Higher means greater rhythmic contrast. |
| Memory Time | Temporal memory window | Longer values reuse older events. |
| Max Dur | Maximum retained duration | Rejects overly long events. |
| Chord | Maximum associated notes | Controls harmonic density. |
| ChordDur | Chord-grouping window | Longer values group more notes. |
| MaxDatas | Maximum stored data | Higher means richer, more complex memory. |

Examples:

- Precise texture: low distances, Chord 1–2, short memory.
- Harmonic cloud: wider Dist Freq, high Chord, moderate ChordDur.
- Slow reminiscence: long Memory Time and larger MaxDatas.

## 8. ALGORITHM PARAMETERS

| Family | Typical controls | Effect |
|---|---|---|
| Chaos | Frequency, amplitude, duration, speed | Non-periodic variation. |
| Neurone | Learning, Temperature, dimensions, memory | Proximity-based organization and learning. |
| Genetic | Mutation, crossover, selection, generation | Progressive evolution of characteristics. |
| Algorithm | Speed and rule-specific parameters | Event selection and transformation. |
| Automation | Change speed | Autonomous GUI evolution. |

Temperature generally increases freedom of selection. Learning increases adaptation. Extreme values make results less predictable.

### Comparative Listening Method

1. Record a short sequence.
2. Loop the sequence.
3. Select one algorithm.
4. Change one parameter only.
5. Compare over several bars.
6. Save a preset if the transformation is useful.

## 9. SYNTHESIZER

| Control | Function | Starting point |
|---|---|---|
| Sample 1 / Sample 2 | Sound materials used or cross-processed | Two clearly different sounds. |
| Synth | Selects the SynthDef | SinOsc or PlayBuf. |
| Tuning | Tuning and transposition | 0, then ±12 semitones. |
| Envelope | Attack, evolution, release | Non-zero attack and release. |
| Level | Instrument level | Preserve headroom. |
| Pan | Stereo or spatial position | Center, then widen. |
| Synth Controls | SynthDef-specific macros | Change one control at a time. |
| Output | Output bus | Main output for the first test. |

### Avoiding Clicks

- Keep the first and final envelope levels at zero.
- Avoid exactly zero attack and release.
- With samples, avoid abrupt position jumps on high-energy waveforms.
- Lower level before experimenting with spectral processing.

## 10. Pre FX, Post FX, and MasterFX

| Stage | Role | Good practice |
|---|---|---|
| Pre FX | Processes material before the main rendering stage | Prepare or color the signal. |
| Synth | Produces the principal timbre | Set it dry first. |
| Post FX | Processes the complete instrument result | Add space, delay, or final transformation. |
| LimitOut | General limiter threshold | Around -3 dB for tests. |
| PostAmp | Final gain after processing | 0 dB initially. |

### Gain-Staging Method

1. Lower MasterFX or monitor the output closely.
2. Set each instrument without effects.
3. Enable Pre FX and compensate gain.
4. Enable Post FX and compensate again.
5. Compare bypass at equal perceived level.
6. Raise the master only at the end.

The limiter is protection, not a mixing tool. Continuous limiting means upstream levels are too high.

## 11. Sequences, Presets, and Recording

| Object | Stored content | Use |
|---|---|---|
| Sequence | Events from one instrument | Create a motif or loop. |
| Instrument preset | One voice's settings | Recall timbre and behavior. |
| Multi-instrument setup | Group of voices | Recall an orchestration. |
| Score | GUI actions over time | Replay a complete form. |
| Audio | Sound result | Archive or distribution. |
| Buffer | Sound recorded in memory | Reuse as a sample. |

### Recommended Workflow

1. Create and validate one instrument.
2. Record a short sequence.
3. Save the instrument preset.
4. Add other instruments.
5. Save the multi-instrument configuration.
6. Record a GUI score if interface gestures must be reproduced.
7. Start final audio recording.

Presets do not always include external audio. Preserve the Robot folder and all used resources.

## 12. MIDI, Keyboard, and VST

### Keyboard

- Setup Keyboard ShortCut enables/configures computer-key playing.
- Translate transposes from -36 to +31 semitones.
- Volume sets keyboard level.
- Keyboard is especially useful with the Keyboard analyzer.

### MIDI

- Connect the device before launching Robot.
- Initialize MIDI from the GUI or menu.
- Check the channel if notes arrive but control the wrong instrument.
- MIDI Out can drive an external synthesizer.

### VST

| Control | Function |
|---|---|
| Open / Close | Loads or closes the plug-in. |
| Editor | Opens the plug-in interface. |
| Program | Selects a bank/program. |
| Out 1…32 | Selects audio output. |
| xFade | Blends paths or states. |
| Gain In | Level sent to plug-in. |
| Pan | Output position or width. |

## 13. Useful Shortcuts

| Command | Action |
|---|---|
| Space or Escape | Start/stop system. |
| `F` / `Alt+F` | File analysis playback On/Off. |
| `Ctrl+F` | Load/add a file. |
| `e` | Change analysis algorithm. |
| `y` / `Alt+Y` | Change sound environment. |
| `P` / `Ctrl+P` | Play/stop instrument. |
| `Alt+P` | Play at next bar. |
| `Alt+R` | Start audio recording. |
| `Alt+Shift+R` | Pause/resume recording. |
| `Ctrl+Alt+R` | Stop recording. |
| `W` / `Alt+W` | Show windows. |
| `Alt+V` | Reset effects. |
| `Ctrl+S` | Save active sound/buffer. |

Robot contains additional shortcuts and commands followed by digits. Use the GUI ShortCuts window for the exact installed-version list.

## 14. Three Musical Recipes

### Live Microphone Response

- AudioIn + Onsets
- One SinOsc or PlayBuf instrument
- Short memory, low Chord, moderate Dist Dur
- Enable synchronization after validation
- Light Post FX and cautious MasterFX level

### Harmonic Texture from a File

- FileIn + Pitch or KeyTrack
- Long Memory Time, Chord 4–8, moderate ChordDur
- Two instruments in different registers
- Slow automation or moderate Genetic behavior

### Keyboard and VST Performance

- Keyboard or MIDI source
- VST loaded and output verified
- One sequence as foundation, live playing above it
- GUI score for form changes, audio recording for final archive

Before a concert, prepare a rescue preset with one instrument, no automatic process, no heavy effect, and a safe output level.

## 15. Quick Troubleshooting

| Symptom | Check | Action |
|---|---|---|
| No sound | System, instrument, SynthDef, output | Enable in that order; test SinOsc. |
| No events | Source, analyzer, filter, threshold | Choose Onsets and reduce threshold. |
| Too many events | Threshold/memory too permissive | Raise threshold; reduce MaxDatas and Chord. |
| Out of tempo | Sync, tempo, quantum | Wait for next bar or disable Sync. |
| Saturated sound | Instrument, Pre/Post FX, master | Reduce every stage, not only PostAmp. |
| Clicks | Envelope, sample, position | Add attack/release and avoid jumps. |
| Autonomous settings | Chaos, Neurone, Genetic, Automation | Disable processes one at a time. |
| Silent MIDI | Port, channel, initialization | Reconnect and reinitialize MIDI. |
| Silent VST | Plug-in, bus, Gain In | Open editor; verify Out and Gain In. |
| Empty sequence | Record not armed | Arm before playing and stop cleanly. |

Clean shutdown: stop audio recording, scores, and sequences; stop instruments; stop System; save presets/configurations; close VST and external connections.
