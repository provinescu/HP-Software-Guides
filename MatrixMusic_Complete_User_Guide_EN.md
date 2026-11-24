# Matrix Music: Complete User Guide

**GUI, musical analysis, algorithms, synthesis, and matrix routing**  
Edition: October 4, 2026

This manual covers practical use of Matrix Music. For declared ranges and detailed control behavior, see [Matrix Music GUI Control Reference](MatrixMusic_GUI_Complete_Control_Reference_EN.md). For system architecture, see [Matrix Music Operating Principles](MatrixMusic_Operating_Principles_and_Logic_EN.md).

## 1. Matrix Music in Two Minutes

Matrix Music analyzes audio, file, software-keyboard, or MIDI input, stores detected events, then feeds multiple Synth/FX cells. Every cell can be played, routed, automated, frozen, and saved independently.

| Zone | Use | First action |
|---|---|---|
| Matrix Control | Source, analysis, tempo, memory, cell creation | Choose source, then enable System. |
| Synth/FX window | Instrument, buffers, musical data, envelope, algorithm | Add SinOsc for a first test. |
| Master Sliders | Global control over participating cells | Set register, duration, and pan. |
| MasterFX | Limiter and final gain | Start around LimitOut -3 dB, PostAmp 0 dB. |
| Keyboard / VST | Direct playing and external plug-in | Open only after validating the internal engine. |

Start with one source, one synthesizer, and no automation.

## 2. Express Startup

### With Audio Input

1. Launch Matrix Music and wait for loading to finish.
2. Check MasterFX.
3. Enable AudioIn and the required channels.
4. Choose Onsets for the first test.
5. Add SinOsc from Synthesizer + FX.
6. Enable System, then Start in the SinOsc cell.
7. Play articulated sounds and read Display OSC Message.
8. Adjust Thresh, FhzFilt, AmpFilt, and DurFilt.

### With a File

1. Load a file through **MatrixTools > File for Analyze**.
2. Enable FileIn.
3. Set FileAmp and FileOffset.
4. Select the analyzer and enable System.
5. Use Loop if required.

## 3. Matrix Control Panel

| Command | Function |
|---|---|
| System On/Off | Enables or suspends the complete chain. |
| User Operating System | Add, load, save, copy, and close operations. |
| AudioIn / FileIn / Keyboard / MIDI | Selects the source. |
| BPM Nil / Algo / On / OSC | Selects the tempo reference. |
| Onsets / Pitch / Pitch2 / KeyTrack | Selects the analysis method. |
| Data | Capacity of each input memory. |
| NumFhzBand / RangeBand | Splits pitch register into bands. |
| Automation Preset | Automatically loads configurations. |

`Display OSC Message` confirms that frequency, amplitude, duration, and spectral descriptors are flowing.

## 4. Sources and Analysis

| Mode | Use |
|---|---|
| AudioIn | Live microphone or instrument. |
| FileIn | Loaded audio file. |
| Keyboard | Graphical keyboard or musical keys. |
| MIDI IN | External controller on the selected channel. |
| Onsets | Attack detection. |
| Pitch / Pitch2 | Pitch tracking. |
| KeyTrack | Spectral tonal analysis. |

### Essential Filters

- HiPass and LoPass define the analyzed MIDI register.
- Thresh controls analyzer sensitivity.
- Filter adjusts analyzer-specific confidence/behavior.
- FhzFilt, AmpFilt, and DurFilt require a minimum change before storage.
- MaxDur rejects events that are too long.
- Memory removes old data.

## 5. Frequency Bands and Memory

Events are stored in general memory and per-pitch-band memory. A synthesizer may listen to one or more bands, allowing register-based orchestration.

| Control | Effect |
|---|---|
| NumFhzBand | Number of bands, 1–12. |
| RangeBand | MIDI boundaries for each band. |
| Data | Number of remembered events. |
| Memory | Retention duration. |
| Freeze | Freezes data used by one cell. |
| Copy/Edit Music Data | Copies or edits frozen memory. |

Changing the band count resets memories.

## 6. Adding and Organizing Synth/FX Cells

The **Add a New Synthesizer or FX** menu includes synthesis, samplers, one- or two-buffer FFT, pianos, spectral processing, and effects.

| Command | Use |
|---|---|
| Start/Stop | Cell playback. |
| Change Synthesizer or FX | Replaces the engine without creating a new window. |
| MoveAfter / MoveBefore | Changes node order. |
| Out 1…32 / Bypass | Physical output or bypass. |
| Bus In / Send Bus | Internal audio chaining. |
| FX In / Send FX | Effect-matrix routing. |
| Send Bus / Send Bus FX / Local In | Levels for the three paths. |

Node order matters for serial processing.

## 7. Buffers I and II

| Control | Function |
|---|---|
| Load Buffer I/II | Loads a sample. |
| O/X | Enables/disables the buffer. |
| !/@ | Loop Off/On. |
| → / ← | Forward/reverse playback. |
| Duration | Live-buffer size, 0.01–60 s. |
| Offset | Starting position. |
| Pre / Post | Amount of previous/new content. |
| Rec / Rec@ / Rec! | One-shot recording, loop recording, or stop. |

Two-buffer FFT engines require both materials. Simple samplers normally use one.

## 8. Musical Data and Envelope

| Control | Function |
|---|---|
| Pan | Pan range. |
| Freq | Allowed MIDI register. |
| Transpose | Pitch offset. |
| Amp | Dynamic range. |
| Dur | Duration range. |
| Stretch | Time stretching/compression. |
| Quant | Grid resolution. |
| Envelope | Eight levels and seven time segments. |

### Click Prevention

- Keep the first and final envelope levels at zero.
- Avoid zero attack and release.
- Use both AntiClicks HPplugins controls where supported.
- Lower the level before extreme spectral processing.

## 9. SynthDef Controls

The `Synthesizer Controls` MultiSlider sends up to twelve macros, `ctrl1...ctrl12`. Meaning changes with the selected engine; the label above the control shows parameter order.

| Family | Examples |
|---|---|
| Synthesis | Frequencies, formants, harmonics, filter. |
| Sampler | Trigger, position, granularity, dispersion. |
| FFT | Threshold, wipe, shift, smear, morph. |
| Reverb | Mix, size, damping, decay. |
| Filtering | Frequency, Q, radius, bandwidth. |

Change one macro at a time and compare at equal loudness.

## 10. Algorithms and Playback Modes

| Item | Logic |
|---|---|
| Tdef | Recurrent synchronized reading. |
| OnFly1 / OnFly2 | Decision when data arrives. |
| Stream1 / Stream2 | Continuous flows derived from data. |
| Default | Reference reading. |
| Probability | Probabilistic selection. |
| Euclide | Euclidean/rhythmic organization. |
| Genetic / Kohonen / Neural | Evolution, similarity, or learning transformations. |

Some SynthDef/mode combinations are invalid; Matrix displays a warning when needed.

## 11. Automation

| Control | Action |
|---|---|
| Automation CtrlSynth | Varies SynthDef macros. |
| Automation MusicData | Varies musical parameters. |
| Time | Change interval. |
| Jitter | Variation amount. |
| P F T A D S Q | Pan, frequency, transposition, amplitude, duration, stretch, quantization. |
| B / Y | Buffer or synthesizer. |
| Global Auto% | Probability of adaptation from analyzed statistics. |

Begin with Auto% 5–20 and Jitter 0.05–0.2. Avoid automating gain, routing, and feedback at the same time.

## 12. Master Sliders, Scale, and Tuning

Master Sliders act on cells that accept global control. Every parameter has a range and Auto%.

| Control | Use |
|---|---|
| Pan / Freq / Amp / Dur | Shared ranges. |
| Transpose / Stretch / Quant | Shared values. |
| Root | Root note. |
| Scale | Chromatic, major, minor, Messiaen, pentatonic, ragas, shrutis. |
| Degrees | Direct editing of scale degrees. |

No Scale disables pitch-scale correction.

## 13. Presets, Cells, and Data

| Object | Contents |
|---|---|
| Synthesizer | One cell, global controls, main panel, associated data. |
| Preset | Every window and its order. |
| OSCmusicData | Analyzed memories and frozen data. |
| Copy Synth / Preset | Duplicates a configuration. |
| Random Preset / Synth | Randomly explores available files. |

- `l` / `L`: load Preset / Synthesizer after a number.
- `s` / `S`: save Preset / Synthesizer after a number.
- `j` / `J`: load / save OSCmusicData.

## 14. MIDI, Keyboard, VST, and OSC

| System | Function |
|---|---|
| Keyboard | 17 chromatic keys, transposition, and volume. |
| MIDI IN | Notes and velocity on selected channel. |
| MIDI Out | Sends to external hardware or VST. |
| VST | Browse, Editor, GUI, Out, xFade, Gain In, Pan. |
| OSC Master/Slave | Synchronizes tempo, start, and recording. |
| Ardour | Play/stop transport in master mode. |

Test MIDI, VST, and OSC independently before performance.

## 15. Essential Shortcuts

| Key | Action |
|---|---|
| Space / Escape | System On/Off. |
| `N` | Add SinOsc. |
| `P` / `Alt+P` | Start/Stop all cells. |
| `p` / `Alt+p` | Start/Stop front cell. |
| `h` | Change source. |
| `q` | Change analyzer. |
| `d` | Resynchronize cells. |
| `a` / `A` | Freeze front cell / all cells. |
| `o` | Automation Preset. |
| `z` / `Z` | Random preset / random synth. |
| `Alt+R` / `Shift+Alt+R` / `Ctrl+Alt+R` | Record / pause / stop. |
| `w` / `Ctrl+w` / `Alt+w` | Window navigation / Control Panel. |

The exact list for the installed version is under **MatrixTools > ShortCut**.

## 16. Quick Troubleshooting

| Symptom | Action |
|---|---|
| No sound | Check System, cell Start, Out, Amp, and MasterFX. |
| No data | Check source, channel, analyzer, Thresh, and filters. |
| Empty memory | Increase Memory or Data; prevent clearing. |
| Silent cell | Enable at least one band and check Freeze. |
| Saturation | Reduce Amp, Send levels, FX, and PostAmp. |
| Clicks | Lengthen envelope and adjust AntiClicks. |
| Uncontrolled automation | Disable CtrlSynth, MusicData, and Auto%. |
| Silent MIDI/VST | Check channel, Run, Gain In, and Out. |

Clean shutdown: stop recording, stop cells, disable System, then close the main window.
