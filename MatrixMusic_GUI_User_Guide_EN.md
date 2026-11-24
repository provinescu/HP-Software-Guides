# Matrix Music: GUI User Guide

**Quick Start for Musicians and Performers**  
Version: October 4, 2026

> Based on analysis of `MatrixMusic - copie.txt`. Available resources vary with installed SynthDefs, presets, sounds, and extensions.

## 1. Matrix Music in Two Minutes

Matrix Music analyzes audio input, a file, keyboard, or MIDI, stores detected events, then feeds multiple Synth/FX windows. Every window can be played, routed, automated, and saved independently.

| Zone | Use | First action |
|---|---|---|
| Matrix Control | Source, analysis, tempo, memory, synth creation | Choose source, enable System. |
| Synth/FX window | Instrument, buffers, musical data, envelope, algorithm | Add SinOsc for first test. |
| Master Sliders | Global control for participating windows | Define register, duration, pan. |
| MasterFX | Limiter and final gain | LimitOut -3 dB, PostAmp 0 dB. |
| Keyboard / VST | Direct playing and external plug-in | Open after internal engine works. |

Start with one source, one synthesizer, and no automation.

## 2. Quick Start

### Audio Input

1. Launch Matrix Music and wait for loading.
2. Check MasterFX.
3. Enable AudioIn and the required channels.
4. Choose Onsets for the first test.
5. Add SinOsc from Synthesizer + FX.
6. Enable System, then Start in the SinOsc window.
7. Play articulated sounds and read Display OSC Message.
8. Adjust Thresh, FhzFilt, AmpFilt, and DurFilt.

### Audio File

1. Load a file through **MatrixTools > File for Analyze**.
2. Enable FileIn.
3. Set FileAmp and FileOffset.
4. Choose the analyzer, then enable System.
5. Enable Loop if required.

## 3. Matrix Control Panel

| Command | Function |
|---|---|
| System On/Off | Enables or suspends the complete chain. |
| User Operating System | Add, load, save, copy, and close. |
| AudioIn / FileIn / Keyboard / MIDI | Selects source. |
| BPM Nil / Algo / On / OSC | Selects tempo reference. |
| Onsets / Pitch / Pitch2 / KeyTrack | Selects analysis method. |
| Data | Sets memory capacity per input. |
| NumFhzBand / RangeBand | Splits register into bands. |
| Automation Preset | Automatically loads configurations. |

Display OSC Message confirms that frequency, amplitude, duration, and spectral descriptors are circulating.

## 4. Sources and Analysis

| Mode | Use |
|---|---|
| AudioIn | Live microphone or instrument. |
| FileIn | Loaded audio file. |
| Keyboard | Graphical keyboard or musical keys. |
| MIDI IN | External controller on selected channel. |
| Onsets | Attack detection. |
| Pitch / Pitch2 | Pitch tracking. |
| KeyTrack | Spectral tonal analysis. |

### Essential Filters

- HiPass and LoPass define the analyzed MIDI register.
- Thresh controls analyzer sensitivity.
- Filter adjusts analyzer-specific behavior.
- FhzFilt, AmpFilt, and DurFilt require a minimum change before storage.
- MaxDur rejects overly long events.
- Memory removes old data.

## 5. Frequency Bands and Memory

Events are stored in general memory and individual pitch-band memories. Every synthesizer may listen to one or more bands.

| Control | Effect |
|---|---|
| NumFhzBand | Number of bands, 1–12. |
| RangeBand | MIDI limits for each band. |
| Data | Number of remembered events. |
| Memory | Retention duration. |
| Freeze | Freezes data used by one synth. |
| Copy/Edit Music Data | Copies or edits frozen material. |

Changing the band count resets memories.

## 6. Adding and Organizing Synth/FX Windows

The **Add a New Synthesizer or FX** menu offers synthesis, samplers, one- and two-buffer FFT, pianos, spectral processing, and effects.

| Command | Use |
|---|---|
| Start/Stop | Window playback. |
| Change Synthesizer or FX | Replaces engine without creating a new window. |
| MoveAfter / MoveBefore | Changes node order. |
| Out 1…32 / Bypass | Physical output or bypass. |
| Bus In / Send Bus | Internal audio chaining. |
| FX In / Send FX | Routing to effect matrix. |
| Send Bus / Send Bus FX / Local In | Levels for the three paths. |

Node order matters in serial processing.

## 7. Buffers I and II

| Control | Function |
|---|---|
| Load Buffer I/II | Loads a sample. |
| O/X | Enables/disables buffer. |
| !/@ | Loop Off/On. |
| → / ← | Forward/reverse playback. |
| Duration | Live-buffer size, 0.01–60 s. |
| Offset | Starting position. |
| Pre / Post | Old/new content ratio. |
| Rec / Rec@ / Rec! | One-shot recording, loop recording, or stop. |

Two-buffer FFT engines require both materials. Simple samplers usually require one.

## 8. Musical Data and Envelope

| Control | Function |
|---|---|
| Pan | Pan range. |
| Freq | Allowed MIDI register. |
| Transpose | Pitch offset. |
| Amp | Dynamic range. |
| Dur | Duration range. |
| Stretch | Time expansion/compression. |
| Quant | Grid resolution. |
| Envelope | Eight levels and seven time segments. |

### Click Prevention

- Keep first and final envelope levels at zero.
- Avoid zero attack and release.
- Use both AntiClicks HPplugins controls where supported.
- Reduce level before extreme spectral transformations.

## 9. SynthDef Controls

The large Synthesizer Controls MultiSlider sends up to twelve macros, `ctrl1...ctrl12`. Meaning changes with the selected engine; labels above show parameter order.

| Family | Examples |
|---|---|
| Synthesis | Frequencies, formants, harmonics, filter. |
| Sampler | Trigger, position, granularity, dispersion. |
| FFT | Threshold, wipe, shift, smear, morph. |
| Reverb | Mix, size, damping, decay time. |
| Filtering | Frequency, Q, radius, bandwidth. |

Change one macro at a time and compare at equal loudness.

## 10. Algorithms and Playback Modes

| Mode | Logic |
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

| Command | Action |
|---|---|
| Automation CtrlSynth | Varies SynthDef macros. |
| Automation MusicData | Varies musical parameters. |
| Time | Change interval. |
| Jitter | Variation amount. |
| P F T A D S Q | Pan, frequency, transposition, amplitude, duration, stretch, quantization. |
| B / Y | Buffer or synthesizer. |
| Global Auto% | Adaptation probability from analyzed statistics. |

The three button colors distinguish manual control, local automation, and global control.

## 12. Master Sliders, Scale, and Tuning

Master Sliders acts on windows that accept global control. Every parameter has a range and Auto%.

| Control | Use |
|---|---|
| Pan / Freq / Amp / Dur | Shared ranges. |
| Transpose / Stretch / Quant | Shared values. |
| Root | Root note. |
| Scale | Chromatic, major, minor, Messiaen, pentatonics, ragas, shrutis. |
| Degrees | Direct degree editing. |

No Scale disables harmonic pitch correction.

## 13. Presets, Synthesizers, and Data

| Object | Contents |
|---|---|
| Synthesizer | One window, global controls, primary panel, associated data. |
| Preset | Every window and node order. |
| OSCmusicData | Analyzed memories and frozen data. |
| Copy Synth / Preset | Duplicates a configuration. |
| Random Preset / Synth | Random exploration of available files. |

- `l` / `L`: load Preset / Synthesizer after a number.
- `s` / `S`: save Preset / Synthesizer after a number.
- `j` / `J`: load / save OSCmusicData.

## 14. MIDI, Keyboard, VST, and OSC

| System | Function |
|---|---|
| Keyboard | 17 chromatic keys, transposition, volume. |
| MIDI IN | Notes and velocity on selected channel. |
| MIDI Out | Sends to hardware or VST. |
| VST | Browse, Editor, GUI, Out, xFade, Gain In, Pan. |
| OSC Master/Slave | Synchronizes tempo, start, recording. |
| Ardour | Play/stop transport in master mode. |

Test MIDI, VST, and OSC separately before performance.

## 15. Essential Shortcuts

| Key | Action |
|---|---|
| Space / Escape | System On/Off. |
| `N` | Add SinOsc. |
| `P` / `Alt+P` | Start/Stop all synths. |
| `p` / `Alt+p` | Start/Stop front window. |
| `h` | Change source. |
| `q` | Change analyzer. |
| `d` | Resynchronize synths. |
| `a` / `A` | Freeze front window / all. |
| `o` | Automation Preset. |
| `z` / `Z` | Random preset / random synth. |
| `Alt+R` / `Shift+Alt+R` / `Ctrl+Alt+R` | Record / pause / stop. |
| `w` / `Ctrl+w` / `Alt+w` | Window navigation / Control Panel. |

The exact installed-version list is under **MatrixTools > ShortCut**.

## 16. Quick Troubleshooting

| Symptom | Action |
|---|---|
| No sound | Check System, Start, Out, Amp, MasterFX. |
| No data | Check source, channel, analyzer, Thresh, filters. |
| Empty memory | Increase Memory or Data; prevent clearing. |
| Silent synth | Enable at least one band and check Freeze. |
| Saturated sound | Reduce Amp, Sends, FX, PostAmp. |
| Clicks | Lengthen envelope and adjust AntiClicks. |
| Uncontrolled automation | Disable CtrlSynth, MusicData, Auto%. |
| Silent MIDI/VST | Check channel, Run, Gain In, Out. |

Clean shutdown: stop recording, stop synths, then disable System before closing the main window.
