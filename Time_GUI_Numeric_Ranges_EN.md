# Time GUI: Numeric Ranges, Sequencer, Voices, Filters, Effects, and Spatialization

**Analysis date:** October 5, 2026  
**Source basis:** `Time - copie.txt`

Time analyzes live audio or a file, stores recent events in band-based memory, and uses one central sequencer to produce four independent voices.

## 1. Architecture at a Glance

| Layer | Function |
|---|---|
| Source | AudioIn or FileIn; keyboard/MIDI as external control. |
| Analysis | Onsets, Pitch, Pitch2, or KeyTrack. |
| Memory | Frequency, amplitude, duration, and recent chords. |
| Sequencer | Rand, Seq, WeightS, and WeightP. |
| Voices 1–4 | Synth/Sample/Buffer, filters, FX, pitch, and jitter. |
| Output | Limiter, PostAmp, MIDI, VST, and spatialization. |

Diagnostic chain: **source -> analysis -> memory -> sequencer -> voice -> filter/FX -> output**.

## 2. Reading Widgets and Scales

| Type | Meaning |
|---|---|
| EZSlider / EZKnob | One value. |
| EZRanger | Minimum and maximum bounds. |
| `lin` | Uniform progression. |
| `exp` | Increased precision at small values. |
| `db` | Logarithmic amplitude scale. |
| `unipolar` | 0 to 1. |
| `bipolar` | -1 to +1. |
| O / X | Active/inactive function or band. |
| -∞ dB | Digital silence. |

## 3. Source, File, and External Commands

| Control | Range / choice | Behavior |
|---|---|---|
| System On/Off | Two states | Starts/stops all processes. |
| AudioIn / FileIn | Two choices | Selects live input or file reader. |
| Input Bus | Bus 1…N | Analyzed channel; depends on audio configuration. |
| FileVol | -120 to +12 dB, lin | File level before analysis. |
| FileOffset | 0–1 | Normalized file position. |
| Volume | dB ControlSpec | Keyboard/module level. |
| OSCtempo | On/Off | Accepts or ignores external tempo commands. |
| MIDI channel | Dynamic menu | MIDI receive channel. |

Keep FileVol constant when comparing analysis thresholds. Avoid large positive gain before analysis.

## 4. Analysis, Filters, and Chords

| Control | Exact range | Musical effect |
|---|---:|---|
| Thresh, alternate | 0–10, lin | Analyzer/function-specific threshold. |
| Main Thresh | 0–1, unipolar | Detection sensitivity. |
| Filter | 0–1, unipolar | Analyzer confidence/filter. |
| Chord Time | 0.01–1 s, exp | Note-grouping window. |
| Fhz Filter | 0–12 semitones, lin | Minimum stored pitch difference. |
| Amp Filter | 0–60, step 0.1 | Minimum amplitude difference. |
| Dur Filter | 0.01–16 s, exp | Minimum inter-event interval. |
| Speed FFT | 0.01–100, exp | Spectral-tracking cadence/speed. |
| Range FFT | 0–1, ranger | FFT normalization/display bounds. |

If too many events are detected, raise Thresh or the Fhz/Amp/Dur filters. If nothing is detected, reduce them gradually.

## 5. Memory and Bands

| Control | Exact range | Behavior |
|---|---:|---|
| Time Memory | 0.01666–3600 s, exp | Retention duration or delay before clearing. |
| Bands 1…N | O / X | Includes/removes a band. |
| All | O / X | Enables/disables all bands. |
| Chord memory | Depends on Chord Time | Groups close events into one chord. |
| Infinite / Hold | Button, panel-dependent | Retains memory during silence. |

Short memory reacts to current playing. Long memory produces more stable, historical material. Disabling bands reduces the sequencer's available register.

## 6. Sequencer Modes

| Mode / control | Range | Musical behavior |
|---|---:|---|
| Rand | — | Randomly selects permitted material. |
| Seq | — | Traverses events in order. |
| WeightS | — | Weighted source/band selection. |
| WeightP | — | Weighted pitch/position selection. |
| Ambitus | 0–127 MIDI, ranger | Limits voice register. |
| Weight | 0–1, lin | Weight intensity. |
| Jitter % | 0.01–100, exp | Random dispersion of choice/time. |
| Off | 0.01–100, exp | Time offset/read offset, module-dependent. |

To compare modes, use one voice, minimal jitter, narrow Ambitus, and short memory.

## 7. Pitch and Transposition

| Control | Exact range | Result |
|---|---:|---|
| Octave | -4 to +4, lin | Octave transposition. |
| Semitone | -12 to +12, lin | Chromatic transposition. |
| Cent | -100 to +100, lin | Microtuning. |
| Ambitus | 0–127 MIDI, ranger | Output bounds. |
| Scale / Tuning | Dynamic menu | Constrains pitches to scale/tuning. |
| Root | Menu/control | Scale root. |

Total transposition:

```text
Octave × 12 + Semitone + Cent / 100
```

Begin with Octave and Semitone; refine with Cent.

## 8. Amplitude, Mix, and Output

| Control | Range | Behavior |
|---|---:|---|
| Amp | dB ControlSpec | Voice gain. |
| Dry Wet | dB ControlSpec | Processed-signal mix/level, implementation-dependent. |
| Limiter | dB ControlSpec | Output safety threshold. |
| PostAmp | dB ControlSpec | Gain after general mix. |
| Mute / Active | O / X | Enables/disables a voice or module. |
| MIDI Out | Off or channels 1–16 | Disables or transmits events. |

Four voices, effects, and VST sum together. Set every Amp with headroom; do not use PostAmp to compensate an already saturated structure.

## 9. Jitter and XY Surfaces

| Control | Exact range | Behavior |
|---|---:|---|
| JitterX % | 0.01–100, exp | Expands horizontal selection area. |
| JitterY % | 0.01–100, exp | Expands vertical selection area. |
| Jitter % | 0.01–100, exp | General parameter dispersion. |
| Surface X | 0–1 normalized | Selects a family/list region. |
| Surface Y | 0–1 normalized | Selects an item in the family. |
| Weight | 0–1 | Favors selected positions/data. |

Low jitter yields targeted, repeatable output. High jitter explores more of the loaded lists. XY surfaces select discrete items; they do not necessarily perform continuous morphing.

## 10. Voices, Samples, and Buffers

| Command | State / behavior | Effect |
|---|---|---|
| Synth menu | Dynamic choice | Selects a SynthDef. |
| Sample/Buffer menu | Dynamic choice | Selects a file or recorded source. |
| Record / Write | Action or On/Off | Writes input to a live buffer. |
| Loop | O / X | Repeats playback. |
| Reverse | O / X | Reverses playback when supported. |
| Offset | `Off` 0.01–100 or internal position | Delays/offsets playback or trigger. |
| Envelope | Editable curve | Controls attack, sustain, and release. |
| Voice active | O / X | Enables/disables voice chain. |

Use non-zero attack/release for Reverse, short loops, and rapid buffer changes.

## 11. Filters and Effects

| Zone | Behavior |
|---|---|
| Filter menu | Selects time-domain, resonant, or spectral filter. |
| FX menu | Selects voice effect. |
| Dry Wet | dB according to code; processed level/mix. |
| FX active | Enables/bypasses processing. |
| Pre/Post | Sets processing position. |
| Specific parameters | Dynamic ranges from selected SynthDef/effect. |

Test filter without FX first, then add one effect and compare at equal loudness. Spectral processing and granulation may significantly increase CPU usage.

## 12. Spatialization, MIDI, and VST

| Control | Range / choice | Behavior |
|---|---:|---|
| VST Pan | -1 to +1, ranger | Pan bounds. |
| xFade | 0–1 | Direct/processed blend. |
| Gain In | 0–1 | VST input gain. |
| VST Out | Outputs 1–32 | Plug-in physical output. |
| MIDI Out | Off, 1–16 | Voice MIDI output channel. |
| OSCtempo | On/Off | External tempo synchronization. |
| Output routing | Dynamic menu | Audio/multichannel output. |

## 13. Buttons and State Changes

| Command | States | Consequence |
|---|---|---|
| System | On / Off | Starts/stops Time. |
| Audio type | AudioIn / FileIn | Changes source. |
| Voice/module | O / X | Enables/disables function. |
| Mode | Rand / Seq / WeightS / WeightP | Changes sequencer strategy. |
| Loop / Reverse / Record | O / X | Changes buffer reading/writing. |
| MIDI Out | Off / 1…16 | Routes events. |
| VST Run | On / Off | Enables plug-in. |
| Browse / Editor / GUI / Close | Actions | Selects, shows, or closes VST. |
| Preset Load/Save | File action | Restores/stores complete state. |

## 14. Recommended Musical Starting Points

| Goal | Configuration |
|---|---|
| Reactive texture | Onsets; Memory 1–10 s; Rand; Jitter 0.01–3%; Ambitus 48–76. |
| Melodic sequence | Pitch; Seq; Ambitus 48–84; Octave 0; Semitone 0; Cent 0. |
| Weighted harmony | Chord Time 0.05–0.2 s; WeightS/P 0.4–0.8; scale enabled. |
| Slow cloud | Memory 30–300 s; Dur Filter 0.2–2 s; Jitter 5–20%; moderate FX. |
| Microtonality | Cent ±5–30; narrow Ambitus; one voice before layering. |
| Four-voice counterpoint | Separate bands/Ambitus; reduced Amp; distinct MIDI/FX. |

## 15. Troubleshooting and Safety

| Symptom | Check |
|---|---|
| No sound | System, source, analysis, memory, Voice O/X, Amp, output, Master. |
| No data | Bus, FileVol, Threshold, Filter, analyzer. |
| Too many notes | Raise Thresh or Fhz/Amp/Dur Filter; reduce memory. |
| Unstable pitch | Try Pitch2/KeyTrack; reduce Jitter and Ambitus. |
| No chords | Raise Chord Time; verify source/polyphony. |
| Saturation | Lower Amp, Dry Wet, PostAmp, VST Gain In. |
| High CPU | Reduce voices, FFT Speed, spectral FX, buffers, VST. |
| Incomplete preset | Check SynthDefs, samples, VST, and paths. |

## 16. Exact Declared Control Specifications

| GUI label | Exact specification | Widget |
|---|---|---|
| Volume | `db` | EZSlider |
| Thresh, alternate | `0, 10, lin, 0` | EZSlider |
| FileVol | `-120, 12, lin, 0` | EZSlider |
| FileOffset | `unipolar` | EZSlider |
| Main Thresh | `unipolar` | EZSlider |
| Filter | `unipolar` | EZSlider |
| Chord Time | `0.01, 1, exp, 0` | EZSlider |
| Time Memory | `0.01666, 3600, exp, 0` | EZSlider |
| Fhz Filter | `0, 12, lin, 0` | EZSlider |
| Amp Filter | `0, 60, lin, 0.1` | EZSlider |
| Dur Filter | `0.01, 16, exp, 0` | EZSlider |
| Speed FFT | `0.01, 100, exp, 0` | EZSlider |
| Limiter | `db` | EZSlider |
| PostAmp | `db` | EZSlider |
| Range FFT | `unipolar` | EZRanger |
| Ambitus | `0, 127, lin, 0` | EZRanger |
| Weight | `0, 1, lin, 0` | EZSlider |
| Jitter % | `0.01, 100, exp, 0` | EZSlider |
| Octave | `-4, 4, lin, 0` | EZSlider |
| Semitone | `-12, 12, lin, 0` | EZSlider |
| Cent | `-100, 100, lin, 0` | EZSlider |
| Amp | `db` | EZSlider |
| Dry Wet | `db` | EZSlider |
| Off | `0.01, 100, exp, 0` | EZSlider |
| JitterX % / JitterY % | `0.01, 100, exp, 0` | EZSlider |
| xFade | `unipolar` | EZKnob |
| Gain In | `unipolar` | EZKnob |
| Pan | `bipolar` | EZRanger |

SynthDef, sample, filter, effect, bus, output, MIDI-port, and plug-in lists are dynamic and are built from resources available at startup.
