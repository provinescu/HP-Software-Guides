# Time: Complete User Guide

**GUI, sequencer, analysis, synthesis, and spatialization**  
Edition: October 4, 2026

Time combines musical analysis, band-based memory, one central sequencer, and four independent sound chains. For practical GUI operation, see [Time GUI User Guide](Time_GUI_User_Guide_EN.md). For architecture and internal behavior, see [Time Operating Principles](Time_Operating_Principles_and_Logic_EN.md).

## 1. Quick Overview

Four synthesizers are triggered by a global sequencer. Every voice contains a sample or live source, engine, envelope, filter, effect, panner, and Jitter controls.

| Zone | Purpose | First action |
|---|---|---|
| Time Controls | Startup, source, analysis, memory, outputs, master | AudioIn + Onset. |
| Step Sequencer | Chooses voices per step | Rand mode. |
| Synthesizers 1–4 | Sample, pitch, filter, effect, pan | One unmuted voice. |
| Keyboard | Direct 17-note performance | Set Translate/Volume. |
| VST | External plug-in/routing | Enable after internal validation. |

## 2. Reliable First Setup

1. Set Limiter near -3 dB and PostAmp at 0 dB.
2. Choose AudioIn and the correct bus.
3. Choose Onset.
4. Select SinOsc in Synth 1 and unmute it.
5. Enable band A or one pitch band.
6. Select Rand.
7. Enable System.
8. Adjust Thresh, Fhz Filter, Amp Filter, and Dur Filter while playing.

For FileIn, load a file, set FileVol/FileOffset, and enable Loop only after confirming playback.

## 3. Analysis, Memory, and Bands

| Analyzer | Use |
|---|---|
| Onset | Percussion and clear transients. |
| Pitch | Stable monophonic input. |
| Pitch2 | Harmonic/percussive separation. |
| KeyTrack | Dominant tonal class. |
| Keyboard | Software/computer keyboard. |
| MIDI | External controller. |
| Algo Off | Autonomous sequencing without new analyzed pitches. |

Time stores data in general band A and up to twelve pitch bands. Chord Time groups near-simultaneous notes. Time Memory determines retention. Ambitus remaps detected notes before scale correction.

## 4. Sequencer Modes

| Mode | Result |
|---|---|
| Rand | One random active voice. |
| Seq | Reads the 48-step grid. |
| WeightS | One weighted voice. |
| WeightP | Several voices may play from one shared draw. |

The sequencer selects voices; analysis influences pitch, level, or duration.

## 5. Voice Configuration

Every voice has Weight, SynthDef, Bus In, Seq/Pitch/Grain timing, Translate/Note behavior, sample-loop state, Mute/Solo, and MIDI Out.

- **Seq:** node duration follows sequencer/analyzed duration.
- **Pitch:** duration is corrected by playback rate.
- **Grain:** physical selection duration is retained.

## 6. Samples and Live Buffers

Each voice has one loaded sample and one recorded live buffer. S/A selects the source. RecLevel controls new signal; PreLevel retains old content. High PreLevel and moderate RecLevel create cumulative memory. Waveform selection and Waveform Jitter define the playback region.

## 7. Pitch, Jitter, and Envelope

Octave, Semitone, and Cent provide coarse, chromatic, and microtonal transposition. Jitter adds event-by-event variation around base settings for pitch, amplitude, waveform, filter, FX, and panning.

Use non-zero attack/release, zero first/final envelope levels, and low pitch jitter for live readers to avoid clicks.

## 8. Filter and FX Chain

```text
Synth -> Filter -> FX -> Panner -> MasterFX
```

Filters include classic, resonant, pitch, spectral, and creative processors. Effects include AllpassC, FreeVerb, GVerb, JPverb, CombC, DelayC, and WarpDelay. Set filter first, then effect, and compare at equal perceived level.

## 9. Spatialization

Time supports Stereo, Rotate2, MultiSpeaker, Ambisonic, and Dolby 5.1. PanX/PanY and their jitters control movement. Confirm audio-interface output count before using multichannel modes.

## 10. Scales and Harmonic Organization

Ambitus acts first. Scaling then maps pitch to the selected scale, raga, Messiaen mode, pentatonic set, or 22-shruti tuning. Root transposes the harmonic space. Auto Root derives a root from spectral energy.

## 11. Presets and Automation

- Preset + Control stores the musical interface and Time Controls.
- Preset stores the primary state.
- Synthesizer stores one voice.
- Copy Synth duplicates one voice.
- Automation Preset changes the whole configuration when spectral flux and flatness variation crosses Threshold after the time interval.

Keep samples beside preset archives because paths are serialized.

## 12. External Integration

MIDI supports notes, velocity, pitch bend, and per-voice output. VST provides Browse, Editor/GUI, Out, xFade, Gain In, and Pan. OSC master/slave synchronizes tempo, start, and recording. Master mode can also drive Ardour transport.

## 13. Diagnostic Order

1. Source and input bus.
2. Analyzer and OSC data.
3. Validation filters.
4. Band assignment.
5. Mute/Solo, Weight, mode, Step.
6. Buffer/SynthDef/Amp.
7. Filter/FX/panner routing.
8. MasterFX and physical outputs.

| Symptom | Main checks |
|---|---|
| No data | Source, bus, analyzer, Thresh, filters. |
| Data but no band | Band limits and memory expiration. |
| Band but no voice | Mute/Solo, mode, Weight, Step. |
| Voice but silence | Buffer, SynthDef, Amp, routing. |
| Unstable pitch | Pitch analyzer, Ambitus, scale, jitter. |
| Clicks | Envelope, buffer reading, HP controls. |
| Excessive effect | Dry/Wet, feedback, jitter. |
| Different preset | Sample path or changed GUI layout. |

Time **analyzes, stores by band, selects voices, generates parameters, synthesizes, processes, and spatializes**.
