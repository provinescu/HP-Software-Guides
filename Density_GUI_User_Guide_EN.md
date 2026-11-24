# Density: GUI User Guide

**Graphical Interface for Musicians**  
Edition: Version 1.0, October 2026

> Density listens to a musical stream, extracts features, builds short-term memory, and generates an instrumental texture whose density, register, timbres, effects, and space can be shaped in real time.

## 1. Density in One Minute

| Stage | What Density does | What you control |
|---|---|---|
| Listen | Chooses and analyzes a source | Audio/File, bus, analyzer. |
| Remember | Keeps recent history | Max Data, MemoryTime, Infinite Memory. |
| Transform | Remaps frequency, amplitude, duration | Ranges, transposition, quantization, scale, algorithm. |
| Orchestrate | Chooses synthesis, sound, effect | Three XY surfaces, jitter, sampler/sound. |
| Spatialize | Places and mixes voices | Pan, output format, reverb, limiter. |

Start with low Density, 2–4 maximum synths, and Default. Increase only one dimension at a time.

## 2. Quick Start

1. Launch SuperCollider and instantiate Density with the correct audio devices. Wait for SynthDefs and sounds to load.
2. Choose Audio or File. In File mode, load material with **File for Analyze**.
3. Choose an analyzer: Onsets for attacks, Pitch for monophonic lines, Pitch2 for mixed material, or KeyTrack for reduced pitch-class behavior.
4. Set Max Synth to 2 or 3, Density to about 20–35, and Range Amp conservatively.
5. Click Start and observe the two status lines and Plotter windows.
6. Adjust Threshold/Filter if too many or too few events are detected.
7. Explore Instrument, Sound, and FX gradually, then save a preset.

## 3. Application Windows

| Window | Musical role | Key controls |
|---|---|---|
| Density | Central performance panel | Source, analysis, memory, synthesis, orchestration, tuning. |
| Reverb | Global acoustic space | Models, early/tail levels, dry/wet, pan. |
| Analyze Freq/Amp/Duration | Event monitoring | Display, reset, data loop. |
| Analyze Flux/Flatness/Centroid/Energy/BPM | Spectral monitoring | Speed, display range, reset. |
| Keyboard | Direct playing | Shortcut mode, transposition, volume. |
| VST | External instrument/effect | Run, browser, editor, output, gain, crossfade, pan. |

## 4. AUDIO Panel

| Control | Function | Playing advice |
|---|---|---|
| Start / Stop | Enables/disables analysis and generation | Stop releases voices and clears active memory. |
| Audio / File | Selects source | Audio follows live input; File is repeatable. |
| Analyze Bus | Channel used for analysis | Choose microphone/source-mix channel. |
| Recording Bus | Channel fed to internal samplers | May differ from Analyze Bus. |
| Limiter | Global output ceiling | Preserve headroom; monitor hardware level. |
| File Offset / dB | File position and level | Available in File mode. |

## 5. MIDI and Keyboard

- MIDI IN selects the channel feeding MIDI analysis.
- **Channels MIDI Out On** duplicates generated voices to MIDI and, when active, VST.
- The Channels field contains 13 values: one global channel and twelve band channels, displayed as 1–16.
- Keyboard Shortcut mode uses `y s x d c v g b h n j m , l . é -`; `<` and `>` transpose.

## 6. Choosing an Analyzer

| Mode | Best for | Behavior |
|---|---|---|
| Onsets | Percussion and clear attacks | Attack detection plus Tartini pitch. |
| Pitch | Voice or monodic instrument | Attack detection plus Pitch. |
| Pitch2 | Mixed material | Median harmonic/percussive separation before detection. |
| KeyTrack | Simplified harmonic control | Pitch class mapped around MIDI 60. |
| Keyboard | Software keyboard | Notes directly become musical data. |
| MIDI | External controller | Note and velocity feed the engine directly. |

## 7. Sensitivity, Filters, and Memory

| Control | Effect | Practical adjustment |
|---|---|---|
| Threshold | Attack sensitivity | Raise for false triggers; lower for missed attacks. |
| Filter | Analyzer-specific pitch confidence/filter | Adjust slowly with Plotter open. |
| Off/LoPass/HiPass + FhzPass | Prefilters analyzed signal | Isolate low or high regions. |
| Fhz / Amp / Duration | Minimum accepted variation | Reduces near-duplicates and rapid repetition. |
| Max Duration | Maximum accepted time gap | Beyond it, continuity resets. |
| MemoryTime | Delay before clearing after silence | Extend to retain a trace after playing stops. |
| Max Data | Circular-memory size | Larger is more stable but less reactive. |
| Infinite Memory | Prevents silence clearing | Useful for improvising over established material. |

## 8. Frequency Bands

`FhzBand` divides MIDI 0–127 into equal regions. Index 0 always contains global data. In band mode, Density rotates generation through selected bands that contain data.

- Range Band displays and edits MIDI limits.
- Bands may feed distinct audio and MIDI channels.
- Changing the number of bands resets memories and voices, so do it while stopped.

## 9. Shaping Generated Material

| Control | Action |
|---|---|
| Range FHZ | Output MIDI register. |
| Trans FHZ | Transposition after remapping. |
| Range Amp | Dynamic range. |
| Range Duration | Duration bounds. |
| Trans Duration | Compresses or expands duration. |
| Stretch | Time factor applied to patterns. |
| Quantization | Grid resolution; subdivision = `1 / Quantization`. |
| BPM System | Clock tempo; AutoBPM may replace it. |
| FX | Dry/processed blend for voice effects. |
| Max Synth | Maximum simultaneous voices. |
| Density | Voice-creation probability and interval. |

Density controls both approximate interval `1 - Density` and creation probability. At 0, no voices. Near 1, renewal is maximal but remains limited by Max Synth.

## 10. Envelope, Sampling, and Playback

| Control | Use |
|---|---|
| Envelope | Eight levels and seven segments. |
| Threshold / Sensitivity Synth | Generic `ctrlHP1` / `ctrlHP2` parameters. |
| Offset | Starting point inside a sound. |
| DurSample | Duration of input-recorded buffer. |
| Rec / Pre | Recording level and retained previous content. |
| Ploop | Buffer playback loop. |
| Rloop | Continuous/loop/retrigger recording behavior. |
| Reverse | Forward/reverse playback. |
| Sampler+Sound | Mixed random choice. |
| Sampler | Recorded input only. |
| Sound | Library only. |
| RecSnd | Records input into a copied sound buffer. |

## 11. Instrument, Sound, and FX

The three XY surfaces use X for a family and Y for an item. JitX and JitY add random dispersion. Labels show the resource selected for the most recently created voice.

- Instrument includes pianos, synthesis, buffer readers, PV processing, harmonic delays, and experimental synths.
- Sound includes voice, strings, woodwinds, brass, keyboards, percussion, and special sounds.
- FX includes delays, filters, resonators, spectral processing, and transposition.
- JitterCtrl perturbs descriptors and some selections. Start at 0 and increase modestly.

## 12. Tonality, Chords, and Algorithms

| Control | Function |
|---|---|
| AutoRoot | Derives root from spectral-energy frequency. |
| AutoBPM | Adapts tempo from median analyzed BPM. |
| Chord | Groups short events using the first duration quartile. |
| Algorithm | Randomly selects within Default, Probability, Euclide, Genetic, Kohonen, Neural. |
| Scale/Tuning | Constrains frequency to 12-TET or shruti degrees. |
| Root | Transposes the degree structure. |
| Degrees | Editable scale-degree list. |

## 13. Reverb and VST

Reverb models include Off, BathRoom, Living Room, Church, Cathedral, Canyon, FreeVerb, Allpass, and JPverb. Dry/Wet controls global mix; EarlyLevel and TailLevel control early reflections and the late tail; Pan defines placement.

For VST: enable Run, choose Browse, open Editor or GUI, assign output, then adjust Gain In and xFade. Close releases the controller and creates an empty VST instance.

## 14. Presets, Files, and Recording

| Action | Menu / shortcut | Result |
|---|---|---|
| Load preset | Preset > Load or `l` + number | Restores GUI, orchestras, memories. |
| Save preset | Preset > Save or `s` + number | Writes an `.scd` file. |
| Random preset | `z` | Chooses from the active folder. |
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
- Density 20–30, Max Synth 3
- Max Data 12–24, MemoryTime 3–6 s
- Default, algorithm range `[0, 0]`
- Moderate FX, Living Room reverb

### Sustained Spectral Cloud

- Pitch2, Infinite Memory On
- Density 55–75, Max Synth 6–10
- Band mode with 3–5 active bands
- Stream or StreamEnv instruments
- Wide duration range, Stretch > 1, spacious reverb

### MIDI/VST Counterpoint

- MIDI analyzer and MIDI Out On
- Active scale, fixed Root or AutoRoot
- Chord On, Quantization 4–16
- VST On, channels per band
- Max Synth 4–6 for clarity

## 17. Quick Diagnostics

| Symptom | Probable causes | Actions |
|---|---|---|
| No voice | No data, Density = 0, Max Synth = 0 | Check Start, source, analyzer, status line. |
| Too many triggers | Threshold too low, variation filters at zero | Raise Threshold, Fhz, Amp, Duration. |
| Unstable pitches | Polyphonic source with Pitch | Try Pitch2, KeyTrack, or filtering. |
| Saturated sound | Too many voices, high Range Amp, FX/reverb | Lower Max Synth, Range Amp, and processing. |
| Frozen voices after silence | Infinite Memory or Stream synth | Disable Infinite Memory; Stop then Start. |
| Stuck MIDI | Abrupt stop or wrong channel | Stop, MIDI Init, send/reset All Notes Off. |

Test at reduced level. Multiple voices, spectral effects, resonators, and reverb may produce high peaks despite the limiter.
