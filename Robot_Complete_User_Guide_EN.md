# Robot: Complete User Guide

**GUI, musical practice, architecture, and internal logic**  
Edition: October 4, 2026

This practical guide covers rehearsal and concert use, instrument and sequence creation, effects routing, recording, and analysis. For exact ranges, see [Robot GUI Control Reference](Robot_GUI_Complete_Control_Reference_EN.md). For system architecture, see [Robot Operating Principles](Robot_Operating_Principles_and_Logic_EN.md).

## 1. Robot in Two Minutes

Robot is a multivoice music-creation environment. Audio, file, keyboard, or MIDI input feeds analyzers. Detected events are played by one or more instruments, synchronized to a grid, transformed by algorithms, and processed by effects.

| Zone | Purpose | First action |
|---|---|---|
| General panel | Startup, source, analysis, tempo, processes | Select AudioIn/FileIn, then start. |
| Instrument windows | Sound, memory, algorithm, synthesis, effects | Enable one instrument. |
| MasterFX | Output limiter and gain | Keep safety headroom. |
| Keyboard | Computer-keyboard performance | Set Translate and Volume. |
| VST | External instrument/effect | Open after internal sound works. |

Learn Robot with one source, one instrument, and no effects. Add layers one at a time.

## 2. Express Startup

### Microphone or Audio Input

1. Launch Robot and wait for SynthDefs and sounds to load.
2. Set MasterFX around LimitOut -3 dB and PostAmp 0 dB.
3. Choose AudioIn and select the active input channel.
4. Choose Onsets for rhythm or Pitch for monophonic melody.
5. Start System.
6. Open one instrument, enable playback, and select SinOsc or PlayBuf.
7. Play distinct sounds and adjust the analysis threshold.
8. When stable, enable synchronization or record a sequence.

### File

1. Load the analysis file from the menu.
2. Choose FileIn.
3. Select the analyzer and start System.
4. Enable one instrument and confirm it receives events.
5. Add loop/synchronization only after the first successful test.

If input meters move but Robot does not play, the problem is probably analysis or the instrument, not the audio device.

## 3. GUI Map

| Window | Function | Keep visible when… |
|---|---|---|
| Control | Global state, source, analysis, tempo, processes | Always. |
| Instrument 1…n | Voice-specific settings | Editing that voice. |
| MasterFX | Final level and limiting | Rehearsal and concert. |
| Keyboard | Computer keyboard | Keyboard is the source. |
| VST | Plug-in, program, output, mix | Using a plug-in. |
| Shortcuts | Keyboard command list | Learning. |

An Instrument window is divided into System Parameters, Analysis Filter, Algorithm Parameters, Synthesizer, Pre FX, Post FX, and sequence/recording sections.

## 4. General Panel

| Command | Musical function | Advice |
|---|---|---|
| System | Starts/stops the global engine | Enable after output setup. |
| AudioIn / FileIn | Selects analyzed source | Validate one source at a time. |
| MIDI | Enables MIDI command chain | Connect before launch. |
| Onsets / Pitch / Pitch2 / KeyTrack / Keyboard | Selects extracted information | Onsets is the simplest test. |
| Tempo Analyze | Allows source to influence tempo | Leave Off initially. |
| Metronome | Shows time-grid position | Check synchronization. |
| Load Instrument | Loads one voice | Build a set progressively. |
| Load Instruments | Loads a group | Recall an orchestration. |
| OSC Off/On | Shows network state | Ignore without external setup. |

Global processes include ChaosIn, NeuroneIn, GenetiqueIn, Algorithm, and Automation. Each has a speed. Do not enable all processes on the first run; begin with Algorithm only.

## 5. Sources and Sound Analysis

| Mode | Best use | Limitation |
|---|---|---|
| Onsets | Percussion, attacks, articulated playing | Does not precisely describe pitch. |
| Pitch | Stable monophonic voice/instrument | Hesitates on chords/noise. |
| Pitch2 | Alternative pitch tracking | Compare according to timbre. |
| KeyTrack | Harmonic color/tonal area | Requires a sufficiently rich spectrum. |
| Keyboard | Deterministic keyboard/MIDI playing | Does not analyze audio directly. |

### Analysis Filter

- Choice Filter selects the pre-analysis filter.
- HzPass sets cutoff.
- High-pass removes rumble and low-frequency interference.
- Low-pass reduces highs or unwanted attacks.

To set threshold, play the quietest sound that should be detected, lower threshold until an event appears, then verify silence does not trigger continuously.

## 6. Instrument System Parameters

| Control | Audible effect | Cautious use |
|---|---|---|
| Chaos Fhz | Pitch irregularity | Small value preserves the contour. |
| Chaos Amp | Dynamic variation | Avoid extremes with many voices. |
| Chaos Dur | Duration variation | Raise for more unstable textures. |
| Play / Stop | Enables/disables the voice | Test before adding effects. |
| Input / Output | Chooses instrument buses | Check during silence diagnosis. |
| Record | Arms instrument capture | Stop cleanly after the take. |

System parameters determine how data reaches the synthesizer; they do not directly define timbre.

## 7. Analysis Filter per Instrument

| Control | Function | Musical interpretation |
|---|---|---|
| Dist Freq | Accepted pitch difference | Higher means more distinct melodic changes. |
| Dist Amp | Accepted amplitude difference | Higher means stronger dynamic contrast. |
| Dist Dur | Accepted duration difference | Higher means greater rhythmic difference. |
| Memory Time | Temporal memory window | Longer reuses older events. |
| Max Dur | Maximum retained duration | Rejects overly long events. |
| Chord | Maximum associated notes | Harmonic-density limit. |
| ChordDur | Chord-grouping window | Longer groups more notes. |
| MaxDatas | Maximum remembered data | Larger memory is richer and more complex. |

For precise texture, use low distances, Chord 1–2, and short memory. For harmonic clouds, widen pitch distance and chord size.

## 8. Algorithm Parameters

| Family | Typical controls | Effect |
|---|---|---|
| Chaos | Pitch, amplitude, duration, speed | Non-periodic variation. |
| Neurone | Learning, Temperature, memory/dimensions | Similarity and learning. |
| Genetic | Mutation, crossover, selection, generation | Progressive evolution. |
| Algorithm | Speed and rule-specific parameters | Event selection/transformation. |
| Automation | Change speed | Autonomous GUI evolution. |

Temperature generally increases selection freedom. Learning increases adaptation. Compare algorithms by looping a short sequence, changing one parameter, and listening for several bars.

## 9. Synthesizer

| Control | Function | Starting point |
|---|---|---|
| Sample 1 / Sample 2 | Sound materials or cross-processing sources | Two clearly different sounds. |
| Synth | Selects SynthDef | SinOsc or PlayBuf. |
| Tuning | Tuning/transposition | 0, then ±12 semitones. |
| Envelope | Attack, evolution, release | Non-zero attack/release. |
| Level | Instrument level | Keep headroom. |
| Pan | Stereo/spatial position | Center, then widen. |
| Synth Controls | SynthDef-specific macros | Change one at a time. |
| Output | Output bus | Main output for first test. |

Avoid clicks by keeping first/final envelope levels at zero, using non-zero attack/release, and avoiding discontinuous sample-offset jumps.

## 10. Pre FX, Post FX, and MasterFX

| Stage | Role | Good practice |
|---|---|---|
| Pre FX | Processes material before the main sound stage | Prepare/color the signal. |
| Synth | Produces main timbre | Set it dry first. |
| Post FX | Processes complete instrument output | Add delay, space, or final transformation. |
| LimitOut | General limiter threshold | Around -3 dB for tests. |
| PostAmp | Final gain | 0 dB initially. |

Set each instrument without effects, enable Pre FX and compensate gain, then enable Post FX and compensate again. Compare bypass at equal loudness.

## 11. Sequences, Presets, and Recording

| Object | Stored content | Use |
|---|---|---|
| Sequence | Instrument events | Motif or loop. |
| Instrument preset | One voice's settings | Recall timbre/behavior. |
| Multi-instrument setup | Voice group | Recall orchestration. |
| GUI score | Time-ordered interface actions | Replay a complete form. |
| Audio | Sound result | Archive or publication. |
| Buffer | Recorded in-memory sound | Reuse as sample. |

Recommended workflow: validate one instrument, record a short sequence, save its preset, add other instruments, save the multi-instrument setup, record a GUI score if needed, then record final audio.

## 12. MIDI, Keyboard, and VST

- Keyboard Shortcut mode enables computer-key playing.
- Translate transposes from -36 to +31 semitones.
- Volume sets keyboard level.
- Connect MIDI before launch, initialize it, and verify channel.

| VST control | Function |
|---|---|
| Open / Close | Loads or closes the plug-in. |
| Editor | Opens plug-in interface. |
| Program | Selects bank/program. |
| Out 1…32 | Selects audio output. |
| xFade | Blends paths/states. |
| Gain In | Input level. |
| Pan | Output position/width. |

## 13. Useful Shortcuts

| Command | Action |
|---|---|
| Space / Escape | Start/stop system. |
| `F` / `Alt+F` | File playback On/Off. |
| `Ctrl+F` | Load/add a file. |
| `e` | Change analysis algorithm. |
| `y` / `Alt+Y` | Change sound environment. |
| `P` / `Ctrl+P` | Play/stop instrument. |
| `Alt+P` | Play at next bar. |
| `Alt+R` | Start audio recording. |
| `Shift+Alt+R` | Pause/resume recording. |
| `Ctrl+Alt+R` | Stop recording. |
| `W` / `Alt+W` | Show windows. |
| `Alt+V` | Reset effects. |
| `Ctrl+S` | Save active sound/buffer. |

Use the GUI Shortcuts window for the exact commands in the installed version.

## 14. Three Musical Recipes

### Live Microphone Response

- AudioIn + Onsets
- One SinOsc or PlayBuf instrument
- Short memory, low Chord, moderate Dist Dur
- Enable synchronization after validation
- Light Post FX and cautious MasterFX level

### Harmonic Texture from File

- FileIn + Pitch or KeyTrack
- Long Memory Time, Chord 4–8, moderate ChordDur
- Two instruments in different registers
- Slow automation or moderate Genetic behavior

### Keyboard and VST Performance

- Keyboard or MIDI source
- VST loaded and output verified
- One sequence as foundation, live playing above it
- GUI score for formal changes and audio recording for archive

Prepare a rescue preset with one instrument, no automatic process, no heavy effect, and a safe output level.

## 15. Troubleshooting

| Symptom | Check | Action |
|---|---|---|
| No sound | System, instrument, SynthDef, output | Enable in that order; test SinOsc. |
| No events | Source, analyzer, filter, threshold | Choose Onsets and reduce threshold. |
| Too many events | Threshold/memory too permissive | Raise threshold; reduce MaxDatas and Chord. |
| Out of tempo | Sync, tempo, quantum | Wait for next bar or disable Sync. |
| Saturation | Instrument, Pre/Post FX, master | Reduce every stage, not only PostAmp. |
| Clicks | Envelope, sample, offset | Add attack/release and avoid jumps. |
| Autonomous settings | Chaos, Neurone, Genetic, Automation | Disable processes one at a time. |
| Silent MIDI | Port, channel, initialization | Reconnect and reinitialize MIDI. |
| Silent VST | Plug-in, bus, Gain In | Open editor; verify Out and Gain In. |
| Empty sequence | Record not armed | Arm before playing and stop cleanly. |

Clean shutdown: stop audio recording, scores and sequences; stop instruments; stop System; save presets/configurations; close VST and external connections.
