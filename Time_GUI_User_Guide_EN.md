# Time: GUI User Guide

**Getting started for musicians and composers**  
Version: October 4, 2026

> Resources may vary with installed SynthDefs, presets, sounds, and extensions.

## 1. Time in Two Minutes

Time is a real-time composition environment. Four synthesizers are triggered by a global sequencer. Every voice owns a sample or live source, sound engine, envelope, filter, effect, panner, and Jitter parameters.

| Zone | Function | First action |
|---|---|---|
| Time Controls | Startup, source, analysis, memory, outputs, master | Choose AudioIn, then Onset. |
| Step Sequencer | Chooses which voices play on each step | Begin with Rand. |
| Synthesizer 1–4 | Sound, sample, pitch, filter, effect, pan | Keep one voice unmuted. |
| Keyboard | Direct 17-note playing | Set Translate and Volume. |
| VST | External plug-in and routing | Enable after internal sound works. |

Start with SinOsc, no filter, no effect, then add layers one at a time.

## 2. Express Startup

1. Launch Time and wait for the server.
2. Keep Limiter near -3 dB and PostAmp at 0 dB.
3. Choose AudioIn and the correct input bus.
4. Choose Onset.
5. Select SinOsc in Synth 1 and disable Mute.
6. Enable band A or one pitch band.
7. Choose Rand in the sequencer.
8. Enable System.
9. Play sounds and adjust Thresh, Fhz Filter, Amp Filter, and Dur Filter.

For a file: load File for Analyze, choose FileIn, set FileVol and FileOffset, enable Loop if needed, then start System.

## 3. Time Controls

| Command | Use |
|---|---|
| System On/Off | Starts/stops the sequencer and source. |
| UserOperatingSystem | Loads, saves, or copies presets/synthesizers. |
| OSCtempo | Enables tempo analysis. |
| MIDI IN 1–16 | Selects MIDI channel. |
| AudioIn / FileIn | Selects analyzed source. |
| Bus 1–32 | Selects audio input. |
| Algo Off / Onset / Pitch / Pitch2 / KeyTrack / Keyboard / MIDI | Selects data-generation method. |
| Automation | Loads presets according to spectral change. |

The Audio indicator displays detected amplitude and helps verify the source.

## 4. Sources and Analysis

| Mode | Best use |
|---|---|
| Onset | Percussion and clear attacks. |
| Pitch | Stable monophonic instruments. |
| Pitch2 | Harmonic/percussive separation before detection. |
| KeyTrack | Dominant tonal class. |
| Keyboard | Graphical/computer keyboard. |
| MIDI | External controller. |
| Algo Off | Autonomous sequence without new analyzed pitches. |

- Thresh controls attack sensitivity.
- Filter controls pitch/KeyTrack confidence.
- Off/LoPass/HiPass and HzPass prepare the signal.
- Fhz Filter requires a minimum pitch difference.
- Amp Filter requires a minimum level difference.
- Dur Filter requires a minimum duration.

## 5. Memory and Frequency Bands

Time stores events in general band A and up to twelve pitch bands. A voice may listen to several bands.

| Control | Function |
|---|---|
| Chord Time | Window for grouping notes. |
| Time Memory | Duration before clearing. |
| NumFhzBand | Number of active bands. |
| Range Band | MIDI limits for each band. |
| A / 1–12 | Bands allowed for each synthesizer. |
| Ambitus | Remaps incoming notes into a register. |

Changing NumFhzBand resets band memory.

## 6. Global Sequencer

| Mode | Logic |
|---|---|
| Rand | Random active voice. |
| Seq | Reads the step grid. |
| WeightS | Selects one voice according to weight. |
| WeightP | Plays voices whose weight passes one shared draw. |

Step sets active length up to 48 steps. Weight controls voice importance. Mute removes a voice; Solo isolates it. The BPM control is a range from which Time chooses durations when analysis does not provide one.

## 7. Each Synthesizer

| Command | Function |
|---|---|
| Weight | Weight in weighted modes. |
| SynthDef | Sound engine. |
| Bus In | Input recorded by the voice. |
| Seq / Pitch / Grain | Method used to calculate sound duration. |
| Translate / Note | Use of analyzed pitch. |
| Smp Loop | Sample-reader loop. |
| Mute / Solo | Voice state. |
| MIDI Out | Optional output channel. |

The four voices are autonomous but share sequencer, scale, and analysis.

## 8. Sample and Live Buffer

| Control | Function |
|---|---|
| Load Sample | Loads a file into the voice. |
| Reverse | Reverses playback rate. |
| S / A | Selects loaded sample or recorded buffer. |
| l / L | Live-buffer recording mode. |
| R | RecLevel: new signal amount. |
| P | PreLevel: retained content amount. |
| Waveform | Selects start and duration. |
| Jitter Waveform | Randomly moves the read region. |

High PreLevel with moderate RecLevel creates cumulative audio memory.

## 9. Pitch, Amplitude, and Envelope

| Control | Effect |
|---|---|
| Octave | Octave transposition. |
| Semitone | Chromatic transposition. |
| Cent | Microtuning. |
| Amp | Voice level. |
| Jitter % | Random-variation amount. |
| O/X button | Enables the corresponding jitter. |
| Envelope | Eight levels and seven time segments. |

To avoid clicks, set first/final envelope levels to zero, use non-zero attack/release, reduce pitch jitter on live readers, and adjust HPplayBuf Rec controls.

## 10. Filters

Every voice has a Filter stage after the Synth. ByPass preserves the sound; other choices apply time-domain or spectral processing.

| Family | Examples |
|---|---|
| Classic | LPF, HPF, RLPF, RHPF, BPF, BRF. |
| Resonance | Ringz, Resonz, CombC, Formlet. |
| Pitch | FreqShift, PitchShift. |
| Spectral | MagFreeze, MagNoise, MagClip, BrickWall, BinShift, RectComb. |
| Creative | DJ_FX, WaveLoss, Warp+Delay. |

Dry/Wet controls dosage. Three parameter labels change with the selected filter. Every parameter and the mix have independent Jitter.

## 11. Effects

| Effect | Main controls |
|---|---|
| ByPass | No processing. |
| AllpassC | Delay and Decay. |
| FreeVerb | Room and Damp. |
| GVerb | Room, RevTime, Damping, TailLevel, RefLevel. |
| JPverb | RevTime, Damping, EarlyDiff, ModDepth, ModFreq. |
| CombC | Delay, Decay, OctLow. |
| DelayC | Delay. |
| WarpDelay | Trigger, Pitch, WinSize, Overlap, Delay. |

Set the filter before the effect to understand the chain.

## 12. Panning and Output Formats

PanX and PanY are controlled with the Panner pad. X/Y Jitter automates movement.

| Format | Principle |
|---|---|
| Stereo | Left-right panning. |
| Rotate2 | Two-channel rotation. |
| MultiSpeaker | PanAz over the selected speaker count. |
| Ambisonic | B-format encoding/decoding. |
| Dolby 5.1 | Front, center, LFE, and rear channels. |

Verify interface output count before selecting a multichannel format.

## 13. Scales and Tuning

| Control | Function |
|---|---|
| No Scale | No pitch correction. |
| Tempered | Custom equal-tempered tuning. |
| Scale | Western scales, Messiaen modes, pentatonics, and ragas. |
| Indian Shrutis | 22-division tuning and related ragas. |
| Root | Root transposition. |
| Degrees | Direct scale-degree editing. |
| Auto Roots | Derives root from spectral energy. |

Ambitus acts before scale mapping; Root and Degrees then define harmonic space.

## 14. MIDI, Keyboard, VST, and OSC

| System | Function |
|---|---|
| Keyboard | 17 notes, -36 to +31 semitone transposition, and volume. |
| MIDI IN | Notes, velocity, and pitch bend. |
| MIDI Out | One output per synthesizer. |
| VST | Browse, Editor, GUI, Out, xFade, Gain In, Pan. |
| OSC Master/Slave | Synchronizes tempo, start, and recording. |
| Ardour | Play/stop transport in master mode. |

Musical Keyboard Shortcut temporarily replaces system shortcuts with note keys.

## 15. Presets, Copy, and Automation

| Object | Contents |
|---|---|
| Preset + Control | Musical GUI and Time Controls. |
| Preset | Main state without all external controls. |
| Synthesizer | One complete voice. |
| Copy Synth | Copies a source voice to a target. |
| Automation Preset | Changes preset when flux and flatness exceed Threshold. |
| Random Preset/Synth | Loads a random file from the active folder. |

Keep samples with presets because saved files store their paths.

## 16. Essential Shortcuts

| Key | Action |
|---|---|
| Space / Escape | System On/Off. |
| `t` | Change sequencer type. |
| `m` | Change MIDI mode. |
| `i` | Recall Init Preset. |
| `Ctrl+F` | Add an analysis file. |
| `w` / `Ctrl+W` | Cycle windows. |
| `k` | Choose a new environment. |
| `z` / `Z` | Random preset / synthesizer. |
| `u` | Automation Preset On/Off. |
| `h` | Switch AudioIn/FileIn. |
| `q` | Change analyzer. |
| `Alt+R` / `Shift+Alt+R` / `Ctrl+Alt+R` | Record / pause / stop. |

Commands `l`, `L`, `s`, `S`, `o`, `a`, and `f` are followed by a number for preset, synth, source, target, or file selection.

## 17. Troubleshooting

| Symptom | Action |
|---|---|
| No sound | Check System, Mute/Solo, active band, Amp, output. |
| No data | Check source, bus, analyzer, Thresh, filters. |
| Voice rarely plays | Raise Weight or use Seq. |
| Unexpected pitch | Check Ambitus, Translate/Note, scale, Root, Jitter. |
| Empty buffer | Check S/A, RecLevel, PreLevel, and l/L mode. |
| Saturation | Reduce Amp, Dry/Wet, PostAmp, and feedback. |
| Clicks | Correct envelope and HP Rec controls. |
| Unpredictable automation | Disable Jitter, Auto Root, and Automation Preset. |

Clean shutdown: stop recording, turn off System, then close windows.
