# Synthesis & Sound Design Skill

## Core Philosophy

Sound design is compositional. Understanding oscillators, filters, and modulation is orchestration in the electronic domain. A sawtooth through a resonant low-pass filter is as deliberate a timbral choice as writing for horn in its dark low register. Modulation routing is formal structure: LFO rates create rhythmic relationships, envelope shapes define phrasing, FM ratios determine harmonic content.

Synthesis is not about presets. It is about understanding why a sound works, what acoustic properties create its character, and how to build from fundamentals toward expressive intent. Every patch decision answers a question: what does this sound need to express?

---

## Oscillators: Fundamental Waveforms

Every waveform is a combination of sine waves (Fourier's theorem). Understanding harmonic content = understanding timbre.

### Sine Wave
Single frequency, no harmonics. Pure. The fundamental building block of all other waveforms. Every complex sound decomposes into sine waves.
- **Harmonic content**: fundamental only. No overtones.
- **Character**: pure, featureless, clinical. Acquires character only through context.
- **Use when**: sub-bass (below ~80Hz harmonics are felt not heard; sine is clean), FM carrier, additive synthesis building block, test tones, pure tonal reinforcement.
- **Orchestral parallel**: none. No acoustic instrument produces a pure sine. Closest: flute in high register, tuning fork.

### Sawtooth Wave
All harmonics (odd and even) at amplitudes of 1/n (where n = harmonic number). The richest standard waveform.
- **Harmonic content**: 1st at full amplitude, 2nd at 1/2, 3rd at 1/3, 4th at 1/4... Gradual spectral rolloff.
- **Character**: bright, buzzy, rich, full. Maximum harmonic material for filtering.
- **Use when**: subtractive synthesis source (most common oscillator choice), leads, pads, bass, brass emulation, any sound needing rich harmonic content to sculpt.
- **Orchestral parallel**: full brass section, bright bowed strings. Maximum harmonic energy.

### Square Wave
Odd harmonics only at amplitudes of 1/n. Missing even harmonics create the characteristic hollow quality.
- **Harmonic content**: 1st, 3rd at 1/3, 5th at 1/5, 7th at 1/7... Only odd harmonics. Even harmonics absent.
- **Character**: hollow, woody, covered. "Clarinet-like" in low register. Retro/chiptune character in high register.
- **Use when**: hollow tones, sub-octave reinforcement (odd harmonics lock with fundamental differently than even), retro sounds, bass with body but less brightness than saw.
- **Orchestral parallel**: clarinet in chalumeau register, covered French horn.
- **Pulse width**: square wave = 50% duty cycle pulse wave. Adjusting duty cycle shifts harmonic content (see pulse wave).

### Triangle Wave
Odd harmonics only at amplitudes of 1/n^2. Energy drops off much faster than square wave.
- **Harmonic content**: 1st, 3rd at 1/9, 5th at 1/25, 7th at 1/49... Odd harmonics with rapid amplitude decay.
- **Character**: soft, mellow, close to sine but with subtle warmth. Gentler than square.
- **Use when**: soft bass, gentle sub-oscillator, LFO waveform (smooth modulation), situations where sine is too pure but square is too hollow.
- **Orchestral parallel**: soft flute, recorder.

### Pulse Wave / PWM
Variable duty cycle version of the square wave. 50% = square. Narrower or wider duty cycles shift the harmonic balance.
- **Narrow pulse** (10-20% duty cycle): thin, nasal, reedy. More harmonics become audible.
- **Wide pulse** (40-60%): approaches square wave character.
- **PWM (Pulse Width Modulation)**: LFO or envelope modulates the duty cycle. Creates a chorus-like, animated effect as the harmonic spectrum constantly shifts. Classic analog pad technique. The "shimmering pad" sound of the Roland Juno.
- **Why PWM works**: constantly changing harmonic content creates perceived movement from a single oscillator, eliminating the need for chorus/detune effects.

### Noise
All frequencies at random amplitude. Spectrally dense, unpitched (or pitched if filtered).
- **White noise**: equal energy per frequency (Hz). Sounds bright because high frequencies are perceptually dominant. Flat on linear frequency analyzer.
- **Pink noise**: equal energy per octave. Rolls off at 3dB/octave. Sounds balanced to human hearing. Flat on logarithmic (1/3 octave) analyzer. Reference for room tuning.
- **Brown/Red noise**: steeper rolloff (~6dB/octave). Low-frequency emphasis. Darker, rumbling.
- **Use when**: percussion synthesis (snare, hi-hat transients), breath/air simulation, texture layers, wind effects, ocean/rain ambience, filtered noise as tonal element.

### Wavetable
Arbitrary waveform shapes stored as lookup tables. Multiple waveforms arranged in sequence; scanning through the table morphs between shapes.
- **Character**: depends entirely on table content. Can be anything from simple waveforms to complex spectral snapshots.
- **Key parameter**: wavetable position. Modulating position with LFO/envelope creates timbral evolution impossible with static waveforms.
- **Power**: combines intuitive control (one knob = timbral morph) with spectral complexity (each position can contain arbitrary harmonic content).
- **Implementations**: PPG Wave (1981, first commercial wavetable synth), Waldorf, Serum, Vital, Ableton Wavetable.

---

## Filters: Spectral Shaping

Filters remove, emphasize, or reshape frequency content. The filter is often more important than the oscillator in defining a synthesizer's character.

### Low-Pass Filter (LPF)
Attenuates frequencies above cutoff. The most common synthesis filter. Subtractive synthesis is fundamentally "rich oscillator through LPF."

**Slope** (dB/octave, poles):
- 6dB/oct (1-pole): gentle, transparent. Subtle tonal shaping.
- 12dB/oct (2-pole): moderate. Oberheim SEM character. Clear, defined but not aggressive.
- 24dB/oct (4-pole): aggressive, dramatic. The Moog ladder filter. Thick, bass-heavy, characterful. Removes harmonics decisively.
- 36dB/oct, 48dB/oct: extreme. Near-brick-wall filtering.

**Steeper slope = more dramatic cutoff, less natural**. 12dB sounds more organic; 24dB sounds more "synthesizer."

**Cutoff frequency as register control**: parallel to orchestral register shifts. Moving cutoff up = moving the sound into a brighter register. Moving cutoff down = darkening, retreating.

### High-Pass Filter (HPF)
Attenuates frequencies below cutoff. Removes low-end weight.
- **Use**: thinning sounds, removing rumble, creating distance/airiness, telephone/radio simulation (HPF + LPF combined).
- **In mixing context**: HPF on everything that doesn't need sub-bass. Clears spectral space.

### Band-Pass Filter (BPF)
Passes a band of frequencies, attenuates above and below. Combination of HPF and LPF.
- **Width (Q/bandwidth)**: narrow BPF = vowel-like resonance, wah, formant. Wide BPF = gentle tonal shaping.
- **Modulated BPF**: sweeping the center frequency creates wah effect. Envelope-controlled BPF = auto-wah.

### Notch Filter (Band-Reject)
Removes narrow frequency band. Opposite of BPF.
- **Use**: removing specific resonances, phaser effects (multiple notches swept together).

### Comb Filter
Series of evenly-spaced notches creating metallic, pitched quality. Short delays create comb filtering.
- **Flanging**: modulated comb filter (very short delay time swept by LFO).
- **Karplus-Strong**: noise burst into short feedback delay = comb filter that produces pitched plucked-string sound. The delay length determines pitch.

### All-Pass Filter
Passes all frequencies equally but shifts phase. No amplitude change.
- **Use**: phasers (multiple all-pass filter stages create swept notch pattern), reverb design (allpass diffusion networks).

### Resonance (Q)
Amplification at cutoff frequency. The most important filter parameter after cutoff.
- **Low Q**: gentle rolloff, natural, transparent.
- **Moderate Q**: emphasizes cutoff frequency, adds nasal/vocal quality. Creates formant-like peaks.
- **High Q**: strong resonant peak. Feedback at cutoff creates ringing, whistling quality.
- **Self-oscillation**: Q high enough that filter generates its own sine wave at cutoff frequency. Filter becomes oscillator. Fundamental to Moog character. Kick drum synthesis: self-oscillating filter with pitch envelope = punchy sine-based kick.

### Filter Character by Design
- **Moog ladder** (4-pole LPF): warm, thick, bass-heavy. Loses low-end at high resonance (bass disappears into self-oscillation). THE analog filter sound.
- **Oberheim SEM** (2-pole state-variable): clear, versatile, less colored. Low-pass, high-pass, band-pass, notch all from one filter.
- **Korg MS-20**: aggressive, gritty, screaming at high resonance. Dual filter (HPF + LPF).
- **Roland**: generally clean, flexible. TB-303 filter: specific acid character from 3-pole (18dB/oct) design with accent circuit.
- **Buchla**: low-pass gate (combines filter and VCA). Amplitude and brightness coupled (natural: louder sounds are brighter). West-coast synthesis fundamental.

---

## Envelopes: Temporal Shaping

### ADSR

**Attack**: time from trigger to peak amplitude.
- Fast (0-5ms): percussive. Click transient. Drums, plucks, stabs.
- Medium (10-50ms): soft attack but still defined. Keys, guitar-like.
- Slow (100ms-2s+): swelling. Pads, strings, ambient. Entrance without transient.

**Decay**: time from peak to sustain level.
- Short (5-50ms): plucky. Combined with low sustain = percussive.
- Medium (50-200ms): settling. Piano-like decay from bright attack to mellower sustain.
- Long (200ms-2s): gradual settling. Smooth transition.

**Sustain**: amplitude level held while note is held. NOT a time value - a level (0-100%).
- 0% sustain: fully decaying sound. Pluck, percussion. No sustained tone.
- 50% sustain: sound decays to half volume and holds.
- 100% sustain: organ-like. No decay phase. Full volume throughout note duration.

**Release**: time from note-off to silence.
- Short (0-50ms): tight, defined. Staccato. Clean cutoff.
- Medium (50-300ms): natural decay. Most acoustic instruments.
- Long (300ms-5s+): trailing, ambient. Reverb-like sustain. Pad releases.

### Beyond ADSR

**Multi-stage envelopes**: AHDSR (Attack-Hold-Decay-Sustain-Release: hold maintains peak before decay begins), multi-segment envelopes with arbitrary breakpoints.
**Looping envelopes**: envelope loops between two points, becoming a complex LFO.
**Envelope curves**: linear (straight lines between points), exponential (fast then slow, or slow then fast), logarithmic. Exponential attack sounds more "natural" because human amplitude perception is logarithmic. Most analog synths use exponential envelopes naturally.

### Envelope Destinations

**Amplitude envelope** (VCA): the fundamental shaping of a sound's volume over time. Every sound has an amplitude envelope.
**Filter envelope**: modulates filter cutoff over note lifetime. THE most important modulation in subtractive synthesis. Fast filter envelope decay = brightness that fades (pluck, bass). Slow filter envelope attack = timbral swell (pad, brass).
**Pitch envelope**: modulates oscillator pitch. Short pitch envelope with fast decay = drum transient, percussive "zap." Longer pitch envelope = riser, fall, sci-fi effect.

### Velocity as Envelope Modulator
MIDI velocity (how hard you play) modulating envelope parameters creates expressive dynamics:
- Velocity -> filter envelope depth: harder = brighter. Mimics acoustic instruments (louder playing excites more harmonics).
- Velocity -> attack time: harder = snappier attack.
- Velocity -> amplitude: harder = louder (basic but essential).

---

## LFOs: Cyclic Modulation

### Waveform Shapes
- **Sine**: smooth, continuous modulation. Vibrato, tremolo. Most natural.
- **Triangle**: similar to sine but with linear ramps. Slightly more "mechanical" modulation.
- **Square**: abrupt on/off switching. Trance gate, rhythmic stutter.
- **Sawtooth** (up/down): ramp up = gradual build then sudden drop. Ramp down = sudden start then gradual fade. Asymmetric modulation.
- **Sample & Hold** (S&H): random value at each cycle. Stepped random modulation. Classic sci-fi bleeps (S&H on pitch). Random filter sweeps (S&H on cutoff).
- **Random/Smooth random**: interpolated random values. Smooth wandering modulation.

### Rate as Musical Parameter
- **Sub-Hz rates** (0.01-1Hz): slow evolution, barely perceptible movement. Ambient.
- **Moderate rates** (1-8Hz): standard vibrato (5-7Hz = natural vibrato speed), tremolo, gentle wobble.
- **Fast rates** (8-20Hz): extreme vibrato, approaching audio rate. "Ring mod" territory.
- **Audio rate** (20Hz+): when LFO enters audio range, modulation becomes FM synthesis. The boundary between modulation and synthesis dissolves.

### Tempo Sync
LFO rate locked to BPM. Creates rhythmic modulation:
- 1/1 (whole note): slow sweep per bar.
- 1/4 (quarter note): sweep per beat. Standard sidechain feel.
- 1/8, 1/16: faster rhythmic modulation. Trance gates, rhythmic filter.
- Dotted/triplet values: polyrhythmic LFO feel against straight beat.
- **Unsynced LFOs**: free-running, not locked to tempo. Creates organic, non-repeating movement. More "natural" than tempo-synced.

---

## Modulation Routing: Architecture

Modulation is what separates a static tone from a living sound. Source -> Destination -> Amount.

### Essential Routings
- **Envelope -> Filter cutoff**: timbral evolution over note lifetime. The single most important modulation routing in subtractive synthesis.
- **Envelope -> Pitch**: pitch sweeps for drums, risers, zaps.
- **LFO -> Pitch**: vibrato. 5-7Hz, subtle depth (2-10 cents) = natural. Wider = extreme.
- **LFO -> Amplitude**: tremolo. Rate and depth define character.
- **LFO -> Filter cutoff**: auto-wah, sweeping filter.
- **Velocity -> Filter envelope depth**: harder playing = brighter (natural dynamics).
- **Velocity -> Amplitude**: harder = louder.
- **Aftertouch -> Vibrato depth**: pressure after key-down adds vibrato. Expressive.
- **Mod wheel -> LFO depth or filter**: performer real-time control.

### Advanced Routings
- **Envelope -> LFO rate**: accelerating/decelerating modulation over note lifetime.
- **LFO -> LFO rate**: complex evolving modulation (frequency modulation of LFO).
- **Velocity -> Envelope attack time**: harder = faster attack.
- **Keytracking -> Filter cutoff**: filter opens with higher notes (mimics acoustic instruments where higher notes are naturally brighter). Essential for natural-sounding patches.
- **Random -> Pitch**: subtle random pitch = analog drift emulation. Makes digital synths sound "alive."

### The Modulation Matrix
Arbitrary source-to-destination routing with variable depth. The modulation matrix IS the instrument. A patch is defined more by its modulation routing than its oscillator selection. Two patches with the same oscillators but different modulation = two completely different instruments.

---

## FM Synthesis: From First Principles

### Core Concept
One oscillator (modulator) modulates the frequency of another (carrier). Result: new harmonic spectrum determined by the ratio and depth of modulation.

### Carrier:Modulator Ratio (C:M)
Determines which harmonics appear in the output spectrum.
- **Integer ratios = harmonic spectra**: 1:1 produces odd and even harmonics (saw-like). 1:2 produces odd harmonics only (square-like). 1:3, 1:4 etc. produce progressively sparser harmonic patterns.
- **Non-integer ratios = inharmonic spectra**: 1:1.41 (square root of 2) = metallic, bell-like. 1:PI = extreme inharmonicity. These create the clangorous, metallic tones FM is famous for.
- **The ratio determines the QUALITY of the sound. The index determines the COMPLEXITY.**

### Modulation Index (I)
I = modulator amplitude / modulator frequency. Controls how many sidebands (and therefore how much spectral complexity) the output contains.
- **Low index** (I < 1): few sidebands, simple spectrum. Mellow, sine-like with subtle harmonics.
- **Moderate index** (I = 1-5): clear harmonic structure. Most musical tones.
- **High index** (I > 5): many sidebands, complex/bright spectrum. Harsh, bright, metallic.
- **Dynamic modulation index**: envelope modulating index over time creates timbral evolution. This is the key to expressive FM. High index at attack decaying to low index = bright attack fading to mellow sustain. Parallels acoustic instruments where louder/harder playing produces brighter tone.

### Sideband Generation
FM creates sidebands at frequencies: carrier +/- (n * modulator frequency) for n = 1, 2, 3...
Each sideband pair's amplitude determined by Bessel functions of the first kind. Negative frequency sidebands fold back as positive frequencies with inverted phase (can cause cancellation). This is why FM spectra are complex and sometimes unpredictable - phase interactions between positive and folded-back sidebands.

### Practical FM Architecture
- **2-operator FM**: one carrier, one modulator. Simple timbral control. Sine -> sine = basic FM.
- **4-operator FM** (DX100, TX81Z, Reface DX): complex enough for most sounds. 8 algorithms.
- **6-operator FM** (DX7): full complexity. 32 algorithms (fixed routing configurations of 6 operators as carriers and modulators in various arrangements).
- **Algorithm**: the fixed routing of operators. Algorithm 1 might be a simple stack (op6 mod op5 mod op4 mod op3 mod op2 mod op1). Algorithm 32 might be all 6 operators as independent carriers (additive synthesis).
- **Feedback**: operator modulating itself. Creates saw-like or noise-like spectra depending on feedback amount. The DX7's feedback parameter is crucial for bass and distorted tones.

### Why FM Matters
FM creates spectra that evolve naturally with dynamics: velocity -> modulation index = louder notes are brighter (more harmonics). This parallels acoustic instruments. FM from first principles (Chowning, Stanford, 1967) is not an "effect" - it is a compositional technique for generating and controlling spectral complexity.

---

## Granular Synthesis

### Concept
Sound fragmented into tiny grains (1-100ms). Each grain is a windowed fragment. Grains recombined at high density create new textures. The fundamental operation: decouple time and pitch.

### Key Parameters
- **Grain size**: 1-10ms = tonal smearing, spectral character dominates. 10-50ms = balanced. 50-100ms = recognizable source fragments.
- **Grain density**: grains per second. 100+ = smooth, continuous texture. 10-50 = grainy, pointillist. 1-10 = sparse, individual events audible.
- **Position**: where in source material grains are read. Fixed position = freezing a single moment. Scanning slowly = time-stretching. Jumping randomly = shuffled texture.
- **Pitch**: each grain independently pitched. Pitch-shift without time-change (or vice versa). Scatter pitch = cloud of transpositions.
- **Spray/scatter**: randomization of grain start position. Creates organic variation, avoids mechanical repetition.
- **Window shape**: how each grain fades in/out. Gaussian (smooth), Hanning (balanced), triangular (sharper), rectangular (clicks at boundaries). Window shape affects smoothness and artifacts.

### Compositional Application
Granular synthesis = time and pitch as fully independent parameters. Freeze a moment indefinitely. Stretch 1 second to 10 minutes. Scatter a vocal across pitch space. Transform recognizable sound into abstract texture. Connection to Xenakis' concept of sound clouds: statistical distributions of individual events creating mass behavior.

---

## Additional Synthesis Methods

### Wavetable Synthesis
Single-cycle waveforms stored in sequence. Scanning through wavetable = morphing between timbres. Position modulation (LFO/envelope scanning through table) = dynamic spectral evolution with intuitive single-parameter control. Serum, Vital, PPG Wave approach.

### Additive Synthesis
Build complex timbres by combining individual sine waves (partials). Each partial has independent amplitude envelope. Can model acoustic instrument behavior: bell partials decaying at different rates, brass harmonics appearing progressively during attack. Computationally expensive. Resynthesis: FFT analysis -> additive recreation -> individual partial modification. Bridge between analysis and synthesis.

### Physical Modeling
Mathematical models of physical sound-producing mechanisms.
- **Karplus-Strong**: noise burst into feedback delay line = plucked string. Delay length = pitch. Feedback filtering = decay character. Remarkably simple algorithm, remarkably convincing result.
- **Waveguide synthesis**: digital model of traveling waves in physical medium (string, tube). Yamaha VL1. Responds naturally to playing parameters (bow pressure, blow force, strike position).
- **Modal synthesis**: model resonant modes of physical body. Each mode has frequency, amplitude, decay rate. Struck objects, resonant bodies.

### Subtractive Synthesis
The dominant paradigm. Start with harmonically rich source (saw/square), remove harmonics with filter. Signal flow: Oscillator(s) -> Mixer -> Filter -> Amplifier -> Output. Each stage modulated by envelopes and LFOs. The "sculpting" metaphor: start with a block of harmonic marble, carve away to reveal the sound within.

---

## Modular Synthesis / Eurorack

### Philosophy
No fixed signal path. Every connection is a patch cable decision. Forces understanding of signal flow at fundamental level. The patch IS the composition.

### Module Categories
- **VCO** (Voltage Controlled Oscillator): sound source. Responds to pitch CV.
- **VCF** (Voltage Controlled Filter): spectral shaping. Cutoff controlled by CV.
- **VCA** (Voltage Controlled Amplifier): amplitude control. Opens/closes signal path.
- **EG** (Envelope Generator): time-varying control voltage.
- **LFO**: cyclic control voltage at sub-audio rates.
- **Sequencer**: step-based or programmatic CV generation. Patterns, melodies.
- **Mixer**: combine audio or CV signals.
- **Effects**: delay, reverb, distortion, wavefolder.
- **Utilities**: attenuators, multiples, switches, logic, slew limiters (portamento), sample & hold, clock dividers, quantizers.
- **Random/Noise**: stochastic CV and audio sources.

### CV (Control Voltage)
1V/octave standard: 1 volt increase = one octave up. Linear, predictable pitch tracking. Gate: on/off signal for note events. Trigger: short pulse for envelope triggering. This is the physical manifestation of the modulation matrix: actual voltages controlling actual circuits.

### Patch as Composition
In modular, removing a cable changes the composition. Repatching = rearranging. This parallels orchestration: reassigning a melody from oboe to clarinet changes the piece. The modular forces awareness that signal routing IS musical decision-making.

---

## Sound Design Approaches

### From Intent
Start with what you want to hear. Ask:
- What is the fundamental pitch behavior? (Sustained, decaying, evolving, unpitched)
- What is the harmonic content? (Simple/dark or complex/bright? Harmonic or inharmonic?)
- How does timbre evolve over time? (Static, brightening attack, darkening decay, morphing?)
- What is the spatial character? (Dry/close, wet/distant, moving?)
- What physical metaphor applies? (Bowed, struck, blown, scraped, electronic?)

### Spectral Thinking
Connect synthesis to spectral analysis:
- Analyze target timbre's spectrum (mentally or with FFT).
- Identify dominant partials and their relative amplitudes.
- Choose synthesis method that best reproduces those relationships.
- FM for harmonic/inharmonic spectra with natural dynamics, additive for precise partial control, subtractive for broad spectral shapes, granular for time-domain manipulation.

### Layering
Multiple synthesis methods or instances combined:
- Sub layer: sine or triangle for low-end weight (clean fundamental).
- Body layer: saw or wavetable for mid-range character and harmonic content.
- Top layer: noise, FM, or bright wavetable for attack definition and air.
- Each layer processed independently (separate filters, envelopes, effects), combined at mixer stage.

---

## Common Pitfalls

- **Preset mentality**: tweaking presets teaches parameter relationships but not synthesis thinking. Build from initialized patch to understand signal flow.
- **Overmodulation**: too many modulation sources = chaotic, unfocused. Each modulation routing should have clear purpose.
- **Ignoring the filter**: oscillator selection matters less than filter treatment. The filter is where timbre lives.
- **Static sounds**: acoustic instruments are never static. Movement (however subtle) creates life. Subtle LFO on pitch (drift), filter (movement), or amplitude (breath) keeps sounds organic.
- **Loudness as quality**: a well-designed quiet sound is more compositionally useful than a maxed-out wall. Dynamic range within sound design = dynamic range in composition.

---

## Cross-References

- **Synthesis <-> Orchestration**: oscillator selection = instrument choice, filter = register, modulation = performance technique, layering = ensemble scoring.
- **Synthesis <-> Acoustic physics**: harmonic series, resonance, formants are the same phenomena whether acoustic or electronic. Understanding one deepens understanding of the other.
- **Synthesis <-> Mixing**: knowing a sound's spectral content from its synthesis parameters informs EQ and processing decisions. If you built the sound, you know its spectrum.
- **Synthesis <-> Spectral composition**: Grisey's approach (analysis -> orchestration) parallels synthesis (analysis -> resynthesis). Both start from understanding what sound IS, spectrally.

See also: REFERENCE_AUDIO_ANALYSIS.md for FFT, spectral analysis, psychoacoustics. MIXING_SKILL.md for processing synthesized sounds in a mix context.
