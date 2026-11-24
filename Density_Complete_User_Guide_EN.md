# Density: Complete User Guide

**GUI, analysis, memory, algorithms, and sound generation**  
Edition: October 4, 2026

This manual explains how to play and configure Density. For exact numeric ranges, see [Density GUI: Complete Control Reference](Density_GUI_Complete_Control_Reference_EN.md). For implementation details, see [Density: Operating Principles and Engine Logic](Density_Operating_Principles_and_Engine_Logic_EN.md).

## 1. Density in One Minute

Density listens to an input stream, extracts significant musical events and spectral descriptors, keeps a recent memory, transforms the data, chooses an orchestration, and renews a texture at an adjustable density.

| Stage | What Density does | What you control |
|---|---|---|
| Listen | Selects and analyzes a source | Audio/File, bus, Onsets/Pitch/Pitch2/KeyTrack/Keyboard/MIDI. |
| Memorize | Keeps recent history | Max Data, MemoryTime, Infinite Memory. |
| Transform | Remaps pitch, amplitude, and duration | Ranges, transposition, quantization, scale, algorithm. |
| Orchestrate | Chooses synthesis, sound, and effect | Instrument/Sound/FX XY surfaces and jitter. |
| Spatialize | Places and mixes voices | Pan, output format, reverb, limiter. |

Start with low Density, 2–4 voices, and Default only. Change one dimension at a time.

## 2. Quick Start

1. Launch SuperCollider and instantiate Density with the correct audio devices. Wait for SynthDefs and sounds to load.
2. Select **Audio** or **File**. For File, use **File for Analyze** to load material.
3. Select an analyzer: Onsets for attacks, Pitch for monophonic lines, Pitch2 for mixed material, or KeyTrack for reduced pitch-class control.
4. Set Max Synth to 2 or 3, Density to about 20–35%, and Range Amp conservatively.
5. Press Start and observe the two status lines and Plotter windows.
6. Adjust Threshold and Filter if too many or too few events are detected.
7. Select Instrument, Sound, and FX progressively.
8. Save a stable preset.

## 3. Application Windows

| Window | Musical role | Key controls |
|---|---|---|
| Density | Central performance panel | Source, analysis, memory, generation, orchestration, tuning. |
| Reverb | Global acoustic space | Model, early/tail levels, dry/wet, pan. |
| Analyze Freq/Amp/Duration | Musical-event monitoring | Display, reset, data looping. |
| Analyze Flux/Flatness/Centroid/Energy/BPM | Spectral monitoring | Speed, display range, reset. |
| Keyboard | Direct playing | Shortcut mode, transposition, volume. |
| VST | External instrument/effect | Run, browser, editor, output, gain, crossfade, pan. |

## 4. AUDIO Panel

| Control | Function | Playing advice |
|---|---|---|
| Start / Stop | Enables/disables analysis and generation | Stop releases voices and clears active memory. |
| Audio / File | Selects the source | Audio follows live input; File is repeatable. |
| Analyze Bus | Channel used for analysis | Select the microphone or source mix. |
| Recording Bus | Channel captured by internal samplers | May differ from Analyze Bus. |
| Limiter | Global output ceiling | Keep headroom and monitor hardware level. |
| File Offset / dB | Position and file level | Available in File mode. |

## 5. MIDI and Keyboard

- MIDI IN selects the note channel for MIDI analysis.
- MIDI Out duplicates generated voices to an external output and, when active, the VST.
- The Channels field contains 13 values: one global channel plus twelve band channels, displayed as 1–16.
- Keyboard Shortcut mode uses `y s x d c v g b h n j m , l . é -`; `<` and `>` transpose.

## 6. Choosing an Analyzer

| Mode | Best for | Behavior |
|---|---|---|
| Onsets | Percussion and clear attacks | Onset detection plus Tartini pitch. |
| Pitch | Voice or monodic instrument | Onset detection plus Pitch. |
| Pitch2 | Mixed material | Median harmonic/percussive separation before detection. |
| KeyTrack | Simplified harmonic control | Pitch class mapped around MIDI 60. |
| Keyboard | Playing from the software keyboard | Notes directly become musical data. |
| MIDI | External keyboard/controller | Note and velocity directly feed the engine. |

## 7. Sensitivity, Filters, and Memory

| Control | Audible/functional effect | Practical adjustment |
|---|---|---|
| Threshold | Attack sensitivity | Raise for false triggers; lower for missed attacks. |
| Filter | Analyzer-specific pitch confidence/filter | Adjust slowly with Plotter visible. |
| Off/LoPass/HiPass + FhzPass | Prefilters the analyzed signal | Isolate low or high regions. |
| Fhz / Amp / Duration | Minimum accepted variation | Reduces near-duplicates and rapid repetition. |
| Max Duration | Maximum accepted time gap | Beyond it, continuity resets. |
| MemoryTime | Delay before clearing after silence | Extend it to retain a trace after playing stops. |
| Max Data | Circular-memory size | Larger is more stable, but less reactive. |
| Infinite Memory | Prevents silence clearing | Useful for improvising over an established analysis. |

## 8. Frequency Bands

`FhzBand` divides MIDI 0–127 into equally sized bands. Index 0 always contains global data. When band mode is active, Density rotates generation across selected bands that contain data.

- **Range Band** displays and edits MIDI limits.
- Bands may feed separate audio and MIDI channels.
- Changing the number of bands resets memories and voices; do so while stopped.

## 9. Shaping Generated Material

| Control | Action |
|---|---|
| Range FHZ | Output MIDI register. |
| Trans FHZ | Post-remapping transposition. |
| Range Amp | Voice dynamic range. |
| Range Duration | Minimum/maximum durations. |
| Trans Duration | Compresses or expands duration by offset. |
| Stretch | Applies a temporal factor to patterns. |
| Quantization | Timing resolution; grid is `1 / Quantization`. |
| BPM System | System clock tempo; AutoBPM may replace it. |
| FX | Per-voice dry/effect crossfade. |
| Max Synth | Maximum simultaneous voices. |
| Density | Voice-creation probability and renewal interval. |

> Density is used twice: it influences the approximate minimum interval `1 - Density` and the probability of creating a voice. At 0, creation stops. Near 1, renewal is maximal but remains capped by Max Synth.

## 10. Envelope, Sampling, and Playback

| Control | Use |
|---|---|
| Envelope | Eight levels and seven segments for compatible SynthDefs. |
| Threshold / Sensitivity Synth | Generic `ctrlHP1`/`ctrlHP2` macros. |
| Offset | Starting point inside a sound. |
| DurSample | Input-recorded buffer duration. |
| Rec / Pre | New recording level and retained previous content. |
| Ploop | Playback loop. |
| Rloop | Continuous/loop/retrigger recording behavior. |
| Reverse | Normal or reverse playback. |
| Sampler+Sound | Mixed random choice of live and library buffers. |
| Sampler | Live input buffer only. |
| Sound | Library buffer only. |
| RecSnd | Records input into a sound-buffer copy. |

## 11. Instrument, Sound, and FX

The three XY surfaces use X for a family and Y for an item. JitX and JitY add random dispersion around the position. Labels show the resource selected when the most recent voice was created.

- **Instrument:** pianos, synthesis, PreBuf/PostBuf readers, PV processing, harmonic delays, experimental synths.
- **Sound:** voice, strings, woodwinds, brass, keyboards, percussion, and special sounds.
- **FX:** delays, filters, resonators, spectral processing, and transposition.
- **JitterCtrl:** perturbs descriptors and selected controls. Start at zero and increase modestly.

## 12. Tonality, Chords, and Algorithms

| Control | Function |
|---|---|
| AutoRoot | Derives a root from spectral-energy frequency. |
| AutoBPM | Adapts tempo from median analyzed BPM. |
| Chord | Groups short events using the first duration quartile. |
| Algorithm | Selects within Default, Probability, Euclide, Genetic, Kohonen, Neural. |
| Scale/Tuning | Constrains pitches to 12-TET or shruti degrees. |
| Root | Transposes the degree structure. |
| Degrees | Editable scale-degree list. |

Begin with Default. Add Probability next, then test adaptive algorithms one at a time.

## 13. Reverb and VST

Reverb models include Off, BathRoom, Living Room, Church, Cathedral, Canyon, FreeVerb, Allpass, and JPverb. Dry/Wet controls the global mix; EarlyLevel and TailLevel control reflections and late decay; Pan defines placement.

For VST: enable Run, choose Browse and a plug-in, open Editor or GUI, assign output, then adjust Gain In and xFade. Close frees the current controller and recreates an empty VST instance.

## 14. Presets, Files, and Recording

| Action | Menu / shortcut | Result |
|---|---|---|
| Load preset | Preset > Load or `l` + number | Restores GUI, orchestras, and memories. |
| Save preset | Preset > Save or `s` + number | Writes an `.scd` file. |
| Random preset | `z` | Selects a preset from the active folder. |
| New environment | `k` | Changes working folder. |
| Load analysis file | File for Analyze or `Ctrl+f` | Loads one or more buffers. |
| Change file | `f` + number | Selects a preloaded file. |
| Record | `Alt+r` / `Shift+Alt+r` / `Ctrl+Alt+r` | Start / pause / stop AIFF recording. |

## 15. Essential Shortcuts

| Key | Command |
|---|---|
| Escape or Space | Start/Stop. |
| `q` / `Ctrl+q` | Next/previous analyzer. |
| `h` | Audio/File. |
| `i` | End voices quickly. |
| `Ctrl+i` | Reset voices and memories. |
| `Alt+i` | Reset everything and reload Init Preset. |
| `w` / `Ctrl+w` | Next/previous window. |
| `a` | Reset Genetic. |
| `Shift+a` | Reset Kohonen. |
| `Alt+a` | Reset Neural. |

## 16. Three Musical Recipes

### Reactive Lightweight Texture

- Audio + Onsets
- Density 20–30%, Max Synth 3
- Max Data 12–24, MemoryTime 3–6 s
- Default only, Algorithm range `[0, 0]`
- Moderate FX, Living Room reverb

### Sustained Spectral Cloud

- Pitch2, Infinite Memory ON
- Density 55–75%, Max Synth 6–10
- Band mode with 3–5 active bands
- Stream or StreamEnv instruments
- Wide duration range, Stretch > 1, spacious reverb

### MIDI/VST Counterpoint

- MIDI analyzer and MIDI Out ON
- Active scale, fixed Root or AutoRoot
- Chord ON, Quantization 4–16
- VST ON, per-band channels
- Max Synth 4–6 for clarity

## 17. Quick Diagnostics

| Symptom | Probable causes | Actions |
|---|---|---|
| No voice | No data, Density = 0, Max Synth = 0 | Check Start, source, analyzer, and Freq/Amp/Duration status. |
| Too many triggers | Threshold too low, zero variation filters | Raise Threshold, Fhz, Amp, and Duration. |
| Unstable pitches | Polyphonic source with Pitch | Try Pitch2, KeyTrack, or prefiltering. |
| Saturation | Too many voices, high Range Amp, FX/reverb | Reduce Max Synth, Range Amp, and processing levels. |
| Frozen voices after silence | Infinite Memory or Stream synth | Disable Infinite Memory; Stop then Start. |
| Stuck MIDI | Abrupt stop or wrong channel | Stop, initialize MIDI, and reset/send All Notes Off. |

> **Playback safety:** test at reduced level. The combination of multiple voices, spectral effects, resonators, and long reverb can create substantial peaks despite the limiter.

## 18. Technical Summary

Density uses two broad voice models:

- **Step voices:** `PmonoArtic` and `Pseq` replay sequences computed from memory at creation time.
- **Stream voices:** continuous synths updated by new analyzed data through `set`.

Each voice owns a mono bus, optional FX, and a master/spatialization stage. Frequency bands can have separate memories, audio destinations, and MIDI channels. On voice expiry, Density releases the pattern/synth, MIDI, temporary buffers, FX, master stage, and returns the bus to the available pool.
