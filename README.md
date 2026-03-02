# Music & Audio Production Skill Tree for Claude

A comprehensive skill system that shapes how Claude reasons about music composition, orchestration, theory, history, synthesis, mixing, and audio analysis. 13 interconnected reference files covering Western music from Medieval plainchant through spectral composition, plus synthesis engineering and mixing fundamentals.

## What This Is

Claude already knows music theory. These files don't teach it facts -- they encode *perspective and approach*. The core philosophy is constraint-driven: every innovation in music history emerged because existing tools couldn't express what the composer needed. That framework changes how Claude analyzes, composes, and discusses music.

The skill system is built around a central philosophy file (`MUSIC_SKILL.md`) supported by detailed reference files. Load the philosophy file for general music work; load specific references when you need technical depth on a particular topic.

## How to Use

Drop these files into your Claude skill directory. Claude will load relevant files based on conversation context. You don't need all 13 loaded simultaneously -- 3-5 at a time is typical.

**Start here**: `MUSIC_SKILL.md` is the entry point. It establishes the compositional philosophy and contains a reference map pointing to all other files.

## File Overview

### Core Philosophy (load first)

| File | Coverage |
|------|----------|
| `MUSIC_SKILL.md` | Constraint-driven composition philosophy, analysis frameworks, orchestration-as-expression principles, behavioral guidelines, reference map to all other files |

### Theory & Harmony

| File | Coverage |
|------|----------|
| `REFERENCE_HARMONY_VOICE_LEADING.md` | Functional tonality, extended/chromatic/atonal/spectral/cluster/microtonal harmony, SATB voice-leading, orchestral voice-leading, form & structure (binary, ternary, sonata, rondo, theme & variations, passacaglia), formal distortion, harmonic rhythm |
| `REFERENCE_MODES_SCALES_PROGRESSIONS.md` | 7 diatonic modes, modes of melodic/harmonic minor, pentatonic/blues, Japanese scales, symmetric scales, exotic scales, chord progressions (foundational, jazz, blues, classical), modal harmony, chord substitution, modal interchange, extended/altered harmony, cadences, neo-Riemannian transformations |
| `REFERENCE_COUNTERPOINT.md` | Species counterpoint (all 5 Fux species), canon types (augmentation, diminution, retrograde, inversion, mirror, spiral, mensuration, puzzle), invertible counterpoint (at octave, 10th, 12th), fugue anatomy and construction, contrapuntal forms (invention, passacaglia, chaconne, quodlibet), historical counterpoint styles, practical writing approaches |
| `REFERENCE_MUSIC_TERMINOLOGY.md` | Italian tempo markings (full BPM scale), tempo modifiers, dynamic markings (pppp-ffff), expression/character terms (~60 Italian terms), articulation, ornaments, structural/form terms, interval & chord terminology, German musical terms, French musical terms |

### Instruments & Orchestration

| File | Coverage |
|------|----------|
| `REFERENCE_ORCHESTRAL_VOICES.md` | All orchestral instruments: woodwinds, brass, pitched/unpitched percussion, keyboards. Ranges, registers, extended techniques, timbral characteristics, blend characteristics for each. Orchestral balance principles, transposition reference, contemporary techniques |
| `REFERENCE_STRINGED_INSTRUMENTS.md` | Deep reference for violin, viola, cello, double bass. Ranges, bowing techniques, left-hand techniques, extended techniques, timbral characteristics per register, blend characteristics, ensemble writing |
| `REFERENCE_INSTRUMENT_EXTENDED.md` | Drum kit, electric guitar (pickups, effects chain), bass guitar, keyboards & synthesizers (prepared piano, organ, Rhodes, Wurlitzer), vocals (types, ranges, extended techniques), world instruments (sitar, oud, koto, erhu, shakuhachi, duduk, didgeridoo, tabla, djembe, gamelan, steel pan, and more) |

### History & Context

| File | Coverage |
|------|----------|
| `REFERENCE_MUSIC_HISTORY.md` | Medieval through present: each era's constraints, key developments, key composers. Electronic music history (Theremin through Eurorack). Genre aesthetics (ambient, industrial, breakcore, bass music, noise, drone). 13 composer case studies focused on constraint-to-innovation (Bach, Beethoven, Wagner, Debussy, Stravinsky, Bartok, Shostakovich, Ligeti, Xenakis, Cage, Reich, Grisey, and others) |

### Scoring

| File | Coverage |
|------|----------|
| `REFERENCE_HORROR_SCORING.md` | Horror/tension scoring techniques: dissonance types, cluster techniques, extended string/wind/brass techniques for unease, psychoacoustic exploitation, silence and dynamics, rhythmic disruption, orchestral effects, layering strategies |

### Production & Sound Design

| File | Coverage |
|------|----------|
| `SYNTHESIS_SKILL.md` | Synthesis philosophy (sound design as composition). Oscillators, filters (topology: Moog ladder, Oberheim SEM, Korg MS-20, Roland, Buchla LPG), envelopes, LFOs, modulation routing. FM synthesis from first principles (C:M ratios, modulation index, sidebands). Granular, wavetable, additive, physical modeling, subtractive synthesis. Modular/Eurorack concepts. Sound design approaches |
| `MIXING_SKILL.md` | Mixing philosophy (mixing as composition continued). Signal flow & gain staging, EQ (frequency ranges with character descriptions), compression (circuit types: VCA/FET/optical/variable-mu, parallel/sidechain/serial/multiband), reverb types, delay, saturation (even vs odd harmonics), stereo field (panning, Haas, mid/side), bus architecture, mastering (LUFS targets per platform), monitoring |
| `REFERENCE_AUDIO_ANALYSIS.md` | Digital audio fundamentals (sampling, Nyquist, bit depth, dithering), Fourier/FFT analysis (window functions, STFT, spectrograms), spectral analysis for composition, psychoacoustics (Fletcher-Munson, critical bands, masking, spatial perception), audio measurement (dBFS/dBSPL/LUFS), librosa computational analysis, acoustic physics |

## Design Principles

**Philosophy first, reference second.** The skill files encode how to *think about* music, not just what to know. Claude already has the knowledge -- these files shape the reasoning approach.

**Constraint-driven.** Every technique, every innovation, every formal choice is understood through the constraint that produced it. This framework applies whether analyzing a Bach fugue or designing a synth patch.

**Context window economics.** 13 dense files rather than 45+ granular ones. Only 3-5 files load into context at a time, so each file must be self-sufficient within its domain while cross-referencing others.

**Cross-referenced.** Files reference each other. Counterpoint connects to voice-leading, history connects to harmony, synthesis connects to audio analysis. The skill tree is a web, not a hierarchy.
