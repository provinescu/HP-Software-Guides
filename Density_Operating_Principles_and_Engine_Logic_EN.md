# Density: Operating Principles and Engine Logic

**Architecture, data flow, voice generation, and maintenance**  
Version 1.0, October 2026

> **Audience:** SuperCollider developers, electroacoustic composers, and maintainers.  
> **Source basis:** the supplied `Density` class, GUI, audio analysis, generation, and routing code.

## 1. Architectural Overview

Density is a monolithic SuperCollider class that coordinates the audio server, analysis, local OSC, circular memories, statistical transformation, voice generation, GUI, MIDI, VST, presets, and recording. `sclang` carries the application logic; `scsynth` executes the SynthDefs.

| Layer | Responsibility | Main objects |
|---|---|---|
| Initialization | Devices, server, paths, MIDI, defaults | `init`, `Server.default`, `MIDIClient` |
| Analysis DSP | Source, FFT, onset, pitch, descriptors | `SynthDef OSC Density *` |
| Data transport | `scsynth` replies to `sclang` | `SendReply`, `OSCFunc` |
| Memory | Global and per-band history | `dataFlux...dataDuree`, `indexData*` |
| Transformation | Statistics and algorithms | `computeAlgoFilterDataMusic` |
| Voices | Synth/FX/master selection and creation | `buildSynth`, `PmonoArtic`, `Synth` |
| Scheduling | Creation and destruction | `Tdef Player`, `WatchSilence` |
| Presentation | Windows, actions, menus, shortcuts | `createGUI` |

The core loop is:

```text
source -> analysis -> OSC events/descriptors -> memories
       -> statistics/algorithm -> voice construction
       -> synth -> FX -> master/spatialization -> limiter
```

## 2. Startup Sequence

1. `new` forwards parameters to `init`.
2. `init` configures the palette, menus, MIDI, working directory, devices, and server options, then initializes values and orchestras.
3. `run` waits for the server to boot.
4. `initSynthDef` compiles and adds SynthDefs.
5. Sounds are loaded into buffers; a default analysis file is loaded.
6. Groups, audio buses, and control buses are created.
7. Analysis, reverb, VST, and limiter synths are instantiated, often paused.
8. `createGUI` creates the windows and writes `Init Preset.scd`.
9. **Start** selects the source/analyzer and launches FFT analysis, `Player`, and `WatchSilence`.

## 3. Group Order and Routing

| Group | Contents | Purpose |
|---|---|---|
| `groupeAnalyse` | Sources and analyzers | Feeds `busAnalyzeIn`, `busRecAudioIn`, and `SendReply`. |
| `groupeRecBuffer` | `RecordBuf` synths | Captures fragments for samplers. |
| `groupeSynth` | Patterns and Stream synths | Monophonic production on internal buses. |
| `groupeFX` | Per-voice effects | Reads and rewrites the voice bus. |
| `groupeMasterOut` | Spatialization and output | Converts mono voice buses to the selected output format. |
| `groupeVerb` | Global reverb | Processes output channels. |
| `groupeVST` | VST bridge | External audio/MIDI. |
| `groupeLimiter` | Final protection | Limiter at the end of the chain. |

The groups are successive `ParGroup` instances. Read/write dependencies therefore depend on server-tree order and the selected `addAction`. Any refactor must preserve producer-before-consumer order.

## 4. Sources and Analysis

Two mono buses structure the input:

- `busAnalyzeIn`: analysis signal;
- `busRecAudioIn`: sampler capture/playback signal.

`AudioIn` copies `SoundIn` to the analysis path. `AnalyzeAudioIn` and `RecAudioIn` select channels independently. `FileIn` reads a stereo buffer, folds it to mono for analysis, and also sends it to the recording bus.

| Analyzer | Trigger | Pitch extraction |
|---|---|---|
| Onsets | `Onsets` power, FFT 512 | `Tartini` |
| Pitch | `Onsets` rcomplex, FFT 512 | `Pitch`, 32–4186 Hz |
| Pitch2 | Onsets on percussive component | `Pitch` on harmonic component after `MedianSeparation` |
| KeyTrack | Generic onset | `KeyTrack`, mapped around MIDI 60 |
| Keyboard/MIDI | Explicit trigger | Note already represented as frequency |

## 5. FFT Descriptors

| Data | UGen | Range / interpretation |
|---|---|---|
| Flux | `FFTFlux` | Clipped to 0.0001–1. |
| Flatness | `SpecFlatness` | Clipped to 0.0001–1. |
| Centroid | `SpecCentroid` | 20–12544 Hz. |
| Energy | `SpecPcile` | Percentile frequency, 20–12544 Hz. |
| BPM | `BeatTrack` | Normalized value, displayed × 60. |

`OSC Density FFT` sends these five values at `speed`, 24 Hz by default. `oscFFT` optionally normalizes them through **Range FFT**, writes control buses, and feeds circular buffers of `maximumData` items.

## 6. Receiving Musical Events

`oscMusic` receives frequency, amplitude, and a timer. The stored duration is derived from `lastTime`. A one-event buffer delays validation: `freqTampon` and `ampTampon` become stored values only when the next event satisfies the variation thresholds.

For each accepted event, the engine:

1. rejects or resets continuity when the time gap exceeds `dureeMaximumAnalyze`, or `memoryTime` when Infinite Memory is disabled;
2. evaluates pitch differences in semitones, amplitude differences in dB, and time differences;
3. writes frequency, amplitude, and duration to global buses and memories;
4. finds the frequency band containing the pitch;
5. copies current FFT descriptors and musical data to that band;
6. updates active Stream synths in that band through `computeAlgoFilterDataMusic`.

## 7. Circular Memories and Silence

Each data family contains `numFhzBand + 1` sub-arrays. Index 0 stores all data; subsequent indices store one frequency band each. Until a memory reaches `maximumData`, values are appended. Afterwards, `wrapPut` replaces the oldest values using an independent write index.

`WatchSilence` compares `Main.elapsedTime` with `dureeAnalyzeOSCMusic`. After `memoryTime` without an event, and when `flagMemory` is off, all memories and indices are rebuilt. An inactive band is also cleared when its own `lastTime` exceeds the limits.

## 8. Statistics Prepared for Each Voice

`buildSynth` reads the selected band and creates `dataMusicTransform`. FFT descriptors are summarized by medians after optional jitter. Frequency, amplitude, and duration receive quartiles, interquartile range, semi-interquartile range, standard deviation, coefficient of variation, and skewness.

| Block | Contents |
|---|---|
| `data[0]` | Flux, flatness, centroid, energy, BPM. |
| `data[1]` | Statistics for MIDI frequencies normalized by 127. |
| `data[2]` | Amplitude statistics. |
| `data[3]` | Duration statistics normalized by `dureeMaximumAnalyze`. |

## 9. Transformation Algorithms

| Algorithm | Principle | Consequence |
|---|---|---|
| Default | Direct remapping, optional chords, range, quantization | Maximum fidelity to memory. |
| Probability | Probabilistic operations guided by coefficient of variation and skewness | Variable transpositions and compression/expansion. |
| Euclide | 3D distance `sqrt(f² + a² + d²)`, followed by statistics | Fuses dimensions into a common attractor. |
| Genetic | `HPclassGenetiques.next`, flux, and flatness | Separate mutation/evolution of F/A/D. |
| Kohonen | Separate `HPclassKohonen` learning | Projects sequences onto learned vectors. |
| Neural | `HPNeuralNet.next` on successive triplets | Joint prediction of frequency, amplitude, and duration. |

Every algorithm returns `[newFreq, newAmp, newDuree]`. Frequency is remapped to **Range FHZ** and converted with `midicps`. Amplitude is remapped to `rangeDBintruments`. Duration is remapped, made positive if necessary, and quantized.

## 10. Chords and Pitch Scales

- With Chord ON, events whose duration is at or below the first quartile accumulate in `chordFreq` and `chordAmp`. A longer duration closes the group.
- Scale mapping computes the octave, finds an equal or next-higher degree, and reconstructs the note.
- AutoRoot derives a root from the median Energy frequency, converts it to octave and degree, then applies the selected tuning.
- The engine supports `Tuning.et12` and `Tuning.sruti` according to the menu.

## 11. Orchestration Selection

XY values are discretized against array sizes. Symmetric jitter is added and clipped to 0–1. X chooses a family; Y chooses an item. The mechanism is shared by synths, sounds, and effects.

| Decision | Source |
|---|---|
| Algorithm | Integer draw between `algoLo` and `algoHi`. |
| SynthDef | `synthOrchestra[X][Y]`. |
| Buffer/sound | `listeBuffer` and `soundOrchestra[X][Y]`. |
| Effect | `fxOrchestra[X][Y]`. |
| Pan | Random value between `panSynthLo` and `panSynthHi`. |
| MIDI channel | `chanelsMidi[0 or indexBand] - 1`. |
| Voice bus | First available index in `listeBusOff`. |

## 12. Sampler and Buffer Choice

| Mode | Logic |
|---|---|
| Sampler+Sound | 50/50 draw between a live-captured buffer and a library sound. |
| Sampler | Allocates a `dureeSample` buffer and uses `RecBufferSynth`. |
| Sound | Uses a preloaded buffer selected by XY. |
| RecSnd ON | Allocates/copies a buffer and records input into it. |

`RecBufferSynth` uses `RecordBuf` with `recLevel`, `preLevel`, `loopRec`, and `trigger`. Temporary buffers are tagged with `path = "Buffer for Sampler"` so they can be released with the voice.

## 13. Voice Models

| Type | Creation | Update behavior |
|---|---|---|
| Step, name without `Stream` | `PmonoArtic` + `Pseq newFreq/newAmp/newDur` | Sequence follows the memory computed at creation. |
| Stream | Continuous `Synth` | `oscMusic` recomputes F/A/D and calls `set` on the active synth. |
| StreamEnv | Continuous synth with envelope/release | Same updates; explicit release at end. |

Each voice also owns a dedicated effect and `masterOut`. Spectral descriptors are mapped from the selected band's control buses, keeping timbre and effect continuously reactive.

## 14. Scheduling and Voice Lifetime

`Tdef Player` loops at resolution `1 / quantizationDuree`. It destroys voices after `dureeStretchBPM`, then attempts creation when the current voice count is below `maximumInstruments` and the interval since the last attempt exceeds `1 - globalDensity`.

- A second random test, `rrand(0, 1) < globalDensity`, decides whether creation occurs.
- In band mode, `rangeSynthBand` is scanned circularly.
- Destruction stops/frees the pattern or synth, MIDI, buffers, FX, and `masterOut`, then returns the bus to `listeBusOff`.
- Planned lifetime uses `newDur.sum * globalDensity.reciprocal`.

> **Edge case:** `globalDensity` is used as probability, interval, and reciprocal lifetime factor. A forced voice creation at zero can divide by zero. The GUI normally prevents creation because probability is zero, but the engine should still clamp or guard the value explicitly.

## 15. Output, Spatialization, and Global Effects

- Each synth writes to a mono `busSynthInOut`.
- The effect reads and rewrites that bus using `xFade`.
- `masterOut` reads the bus and writes to per-band output channels in Stereo, Rotate2, MultiSpeaker, Ambisonic, or Dolby 5.1.
- GVerb, FreeVerb, Allpass, and JPverb run in `groupeVerb` and share parameters through `groupeVerb.set`.
- `SynthLimiter` is placed at the tail in `groupeLimiter`.

## 16. MIDI, VST, and External OSC

- Non-Stream patterns may create a parallel MIDI `Pbind`.
- Stream voices send `noteOff`/`noteOn` for each validated event.
- The VST controller receives `vst_midi` events or `fxVST.midi` calls.
- The `/score` responder accepts triplets targeting `all` or `density` with `preset`, `start`, or `stop`.
- OSC master mode synchronizes recording to `slaveAppAddr` and transport to Ardour on port 3819.

## 17. Presets and Persistence

`fonctionSavePreset` walks the children of all windows, serializes type and value, and appends `synthOrchestra`, `soundOrchestra`, `fxOrchestra`, and the eight data families. `fonctionLoadPreset` replays `valueAction`, restoring both display and side effects.

- Views are restored by position in `children`, not a symbolic ID. Reordering widget creation may break compatibility.
- Some sensitive initial controls are intentionally skipped during loading.
- `Init Preset.scd` is rewritten when the GUI is created.

## 18. Maintenance Risks

| Area | Risk | Recommended improvement |
|---|---|---|
| `GUI children.at(n)` | Strong coupling to widget order | Store named references or a control dictionary. |
| Uninitialized arrays | Variables used with `.add` before explicit setup | Initialize all arrays in `init`. |
| Boolean conditions | Ambiguous `and`/`or` precedence | Add parentheses and unit tests. |
| Centroid bus | `busOSCcentroid` receives Energy in `oscFFT` | Verify and correct if unintended. |
| Audio options | `numInputBusChannels` changed with output format | Restart server when required. |
| MIDI cleanup | `data.at(22).stop` without a nil check in one branch | Track optional resources separately. |
| Division | CV, reciprocals, `globalDensity` | Guard zero means and zero values. |
| Sound loading | macOS-specific `mdfind` | Add portable path resolution. |

## 19. Main Function Map

| Function | Inputs | Outputs / effects |
|---|---|---|
| `fonctionLoadFileForAnalyse` | Path | Stereo analysis buffer. |
| `fonctionLoadSoundOrchestra` | List of paths | Family-based `listeBuffer`. |
| `computeAlgoFilterDataMusic` | Normalized F/A/D, statistics, algorithm | `newFreq/newAmp/newDuree`. |
| `buildSynth` | Band index | Buffer -> synth -> FX -> master chain. |
| `fonctionBand` | Button states | `rangeSynthBand`. |
| `fonctionSavePreset` | Windows | Serializable array. |
| `fonctionLoadPreset` | Serialized array | GUI, orchestras, and memory restoration. |
| `plotDataFFT / Music` | Received values | Plotters and status text. |

## 20. Full Runtime Cycle

1. A source feeds `busAnalyzeIn` and optionally `busRecAudioIn`.
2. Analysis SynthDefs send events and descriptors through OSC.
3. `sclang` filters, delays, classifies by band, and stores data.
4. `Player` chooses a band and calls `buildSynth` according to Density and Max Synth.
5. `buildSynth` summarizes data, selects an algorithm, and creates musical sequences.
6. A synth -> FX -> master chain is built, with optional MIDI/VST.
7. Stream voices follow new data; Step voices replay their pattern.
8. At expiration, every voice resource is released and its bus is recycled.
