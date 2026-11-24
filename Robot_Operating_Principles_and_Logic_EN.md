# Robot: Operating Principles and System Logic

**Musical architecture, data flow, and operational method**  
Version: October 4, 2026

> **Purpose:** This guide explains how Robot converts a source into musical events, organizes them on a time grid, distributes them to instruments, processes them, and records the result. It is based on the supplied Robot source analysis.

## 1. What Robot Is

Robot is a SuperCollider class combining an audio server, GUI, signal analyzers, parallel instruments, sequences, effects, a shared clock, MIDI/OSC/VST integration, and recording. Its central logic is to convert an input into musical data, then reuse that data in synchronized, transformed, and orchestrated forms.

| Layer | Role |
|---|---|
| Configuration | Devices, channels, hardware buffer, folders, and network ports. |
| Acquisition | Microphone, file, computer keyboard, or MIDI. |
| Analysis | Onsets, pitch, tonal region, amplitude, and tempo. |
| Memory | Events, audio buffers, and instrument sequences. |
| Organization | `TempoClock`, bar grid, quantum, and synchronization. |
| Instruments | Independent synthesis/sample voices. |
| Processing | Pre/post effects, envelopes, filters, and spatialization. |
| Output | Mixing, limiting, routing, and recording. |

Robot is not only a sample player. A source may become a musical command, sound material, or both.

## 2. General Architecture

```text
source -> analyzer -> musical events -> language state/memory
       -> clock/grid -> instrument algorithm -> SynthDef/sample
       -> pre/post FX -> output bus -> MasterFX -> recording/output
```

| Flow | Contents |
|---|---|
| Audio | Continuous samples in buses and buffers. |
| Control | GUI values sent to SynthDefs and routines. |
| Events | Pitch, amplitude, duration, attack, index, and time. |
| Time | Tempo, bar, quantum, and next synchronization point. |
| Network | OSC messages among language, server, and external software. |

Audio buses carry sound. Messages and variables carry decisions. Keeping these flows separate is essential for diagnosis.

## 3. Internal Startup

1. Verify the Robot data folder and recording directory.
2. Configure server memory, devices, input/output counts, and hardware buffer.
3. Set floating-point AIFF recording.
4. Open UDP ports and prepare OSC addresses.
5. Create global state, lists, buffers, and defaults.
6. Boot the server and load SynthDefs.
7. Load sounds, presets, sequences, and resources.
8. Build the GUI and install keyboard shortcuts.

| Creation parameter | Meaning |
|---|---|
| `path` | Robot folder and resources. |
| `ni / o / r` | Input, output, and recorded-channel counts. |
| `devIn / devOut` | Audio devices. |
| `size` | Hardware buffer size. |
| `wid / ori / flag` | Multichannel spatialization settings. |
| `scPort` | SuperCollider server port. |

A device, channel-count, or hardware-buffer change normally requires a server restart.

## 4. Sources and Analysis

Robot maintains separate analysis chains for audio input and file playback while preserving the same musical-event logic.

| Analyzer | Extracted information | Use |
|---|---|---|
| Onsets | Attacks and transients | Rhythmic triggering. |
| Pitch | Fundamental frequency | Melody tracking and transposition. |
| Pitch2 | Alternative pitch strategy | Comparison for different timbres. |
| KeyTrack | Tonal class/region from the spectrum | Harmony and scale adaptation. |
| Keyboard | Notes from keyboard/MIDI | Deterministic control. |
| Tempo | Estimated pulse | Clock synchronization. |

Threshold decides what becomes an event. Too high makes Robot appear inactive; too low turns small fluctuations into events. Analysis filters should isolate the useful region rather than merely increase level.

## 5. Clock, Grid, and Synchronization

Robot separates the moment a command is requested from the time it becomes audible. Quantized commands are scheduled on the shared clock, allowing synchronized starts.

| Concept | Function |
|---|---|
| Tempo | Global clock speed. |
| Bar | Groups beats. |
| Quantum | Resolution or next allowed point. |
| Synchro | Delays an action until the next grid point. |
| NextBar | Schedules an action at the following bar. |
| Pitch/tempo sync | Adapts sample playback to pitch or tempo. |

If an action appears late, inspect quantization before assuming a malfunction.

## 6. Instruments and Sound Generation

Robot organizes several instruments in parallel. Each instrument owns activation, level, recording, SynthDef, samples, envelope, sequence, and routing.

| Element | Logic |
|---|---|
| Play | Allows voice creation. |
| Mute / Solo | Changes monitoring without necessarily erasing the sequence. |
| Record | Captures actions or material associated with the instrument. |
| SynthDef | DSP algorithm executed by the server. |
| Sample 1 / 2 | Material for readers and cross-processing. |
| Envelope | Articulation and click prevention. |
| Tuning | Pitch transformation before synthesis. |
| Output | Stereo, multichannel, or external bus. |

Observed synthesis families include PlayBuf readers, SinOsc, synthetic pianos/MdaPiano, harmonic delays, numerous spectral operations, and VSTPlugin hosting.

> The event specifies **what to play**; the SynthDef specifies **how sound is produced**.

## 7. Sequences, Memory, and Algorithms

| Object | Contents | Behavior |
|---|---|---|
| Audio buffer | Continuous signal | Read, loop, reverse, offset, speed. |
| Sequence | Musical-event list | Linear playback, loop, synchronization. |
| GUI score | Ordered interface actions | Replays a session or formal evolution. |
| Preset | Parameter state | Restores an environment. |
| Algorithm | Selection/transformation rule | Changes order, probability, or relationships. |

Material, organization, rendering, and decision are separate. Changing a sample changes material; changing a sequence changes form; changing the SynthDef changes timbre; changing the algorithm changes decision logic.

## 8. Effects, Routing, and Level

| Stage | Function | Main risk |
|---|---|---|
| Pre-FX | Transforms before the main stage | Strongly changes analysis or timbre. |
| Synthesis | Produces instrument voices | Voice accumulation. |
| Post-FX | Processes instrument output | Feedback and energy accumulation. |
| Output bus | Mixes or spatializes | Channel mismatch or summing. |
| MasterFX | Final gain and limiting | Saturation caused upstream. |

Set instrument levels first, then effects, then master. Do not compensate upstream clipping by lowering only the final output.

## 9. MIDI, Keyboard, OSC, and VST

| System | Role |
|---|---|
| Virtual keyboard | Converts computer keys into notes with transposition and volume. |
| MIDI In | Controls notes, parameters, and possibly instrument selection. |
| MIDI Out | Drives external synthesizers or applications. |
| OSC | Synchronizes and exchanges commands among processes/machines. |
| VST | Hosts an external instrument or effect. |

MIDI and keyboard create discrete events directly. Audio must first be analyzed. Test MIDI, OSC, and VST independently before combining them.

## 10. Recording, Scores, and Reproducibility

| Capture | Preserved content | Use |
|---|---|---|
| Audio | Final sound or selected channels | Publication and archive. |
| Buffer | Internal sound material | Reuse as a sample. |
| Sequence | Instrument events | Arrangement and transformation. |
| GUI score | Parameter gestures in order | Replay a performance. |
| Preset | Static values | Restore a starting state. |

A score can reproduce actions while producing a different sound if random algorithms, different files, or live input are involved. Preserve sounds, SynthDefs, presets, sequences, scores, and audio configuration together.

## 11. Reliable First-Test Method

1. Use stereo output and a reasonable hardware buffer.
2. Disable effects, automation, and external connections.
3. Select one source and confirm its level.
4. Use Onsets or Keyboard.
5. Enable one instrument with SinOsc or PlayBuf.
6. Set moderate register, duration, and envelope.
7. Start the global system, then the instrument.
8. Enable synchronization and observe the next grid point.
9. Record and replay a short sequence.
10. Add one effect and save a preset.

Recommended exploration order: source/analysis, clock/grid, one instrument, sequence, second instrument, effects, MIDI/VST, then scores and concert scenarios.

## 12. Layered Diagnostics

| Symptom | Layer | Test |
|---|---|---|
| No signal | Device/source | Check input, channel, and meter. |
| Signal but no events | Analysis | Lower threshold, change analyzer, inspect filter. |
| Events but no sound | Instrument/routing | Enable Play; check SynthDef and bus. |
| Out of tempo | Clock | Check tempo, quantum, and sync. |
| Distorted sound | Synthesis/gain | Reduce voice count, amplitude, and effects. |
| Empty sequence | Memory | Arm recording before playing. |
| Silent VST | Plug-in/MIDI/bus | Open plug-in; verify channel and output. |
| Missing recording | Capture | Check state, directory, and clean stop. |
| GUI active, server frozen | Server | Inspect Post window, release nodes, restart. |

Clean shutdown: stop recording, sequences, and scores; stop instruments; stop the global system; save useful states; release synths, buffers, and external connections.

## 13. Musical Logic Summary

| Question | Robot's answer |
|---|---|
| Where does music come from? | Audio input, file, keyboard, MIDI, or sequences. |
| What is understood? | Attacks, pitch, energy, tonality, and tempo according to the analyzer. |
| Where is information stored? | Variables, buffers, events, sequences, scores, and presets. |
| When does it play? | Immediately or on a `TempoClock` grid. |
| Who plays it? | One or more instruments, each with a SynthDef and routing. |
| How does it evolve? | Parameters, algorithms, transposition, selection, loops, and effects. |
| How is it reproduced? | Sounds, presets, sequences, scores, and the same configuration. |

Robot **listens, describes, memorizes, organizes, interprets, transforms, mixes, and records**.
