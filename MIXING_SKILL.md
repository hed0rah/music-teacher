# Mixing & Audio Engineering Skill

## Core Philosophy

Mixing is not correction. It is composition continued in the frequency domain, the dynamic domain, and the spatial domain simultaneously. Every EQ move is an arrangement decision. Every compressor setting shapes phrasing. Every reverb choice defines the world the music inhabits.

The goal is not "making it sound good." The goal is making musical intent audible and translatable across playback systems. Sometimes that means pristine balance. Sometimes it means deliberate distortion, lo-fi degradation, or extreme processing. Musical intent determines engineering approach, never the reverse.

---

## Signal Flow & Gain Staging

### Gain Staging
Maintaining optimal signal levels through every stage of the signal chain. Fundamental discipline; everything else depends on this.

**Unity gain**: each processing stage outputs approximately the same level as its input (unless intentionally changing level). If a plugin adds 3dB of perceived loudness, compensate by reducing output 3dB. This ensures you're hearing the processing, not the volume increase (louder always sounds "better" due to Fletcher-Munson curves).

**Headroom**: space between signal peaks and clipping point. In 32-bit float DAWs, individual channel clipping doesn't cause distortion (floating point math handles it), but the master bus and physical output still clip at 0dBFS. Target individual track peaks around -12 to -6dBFS, leaving master bus headroom for summing.

**Gain before processing**: plugins have optimal operating levels. Analog-modeled compressors and saturators have sweet spots related to input level. A compressor designed to model hardware that clips at +24dBu behaves differently with a -20dBFS input vs a -6dBFS input. Set input gain deliberately.

**Trim plugin first in chain**: normalize input level before processing. This gives you consistent behavior from every plugin downstream regardless of source level.

---

## EQ: Frequency Domain Orchestration

### EQ Types

**Parametric**: frequency, gain, Q (bandwidth) independently controllable per band. Most flexible. Fully parametric = complete control.

**Shelving**: boost/cut all frequencies above (high shelf) or below (low shelf) a turnover frequency. Broad tonal shaping. Low shelf boost = warmth. High shelf boost = air/presence.

**High-pass filter (HPF)**: removes everything below cutoff. Essential tool. Use on everything that doesn't need sub-bass: vocals, guitars, synths, overheads, room mics. Clears low-end buildup that accumulates across many tracks.

**Low-pass filter (LPF)**: removes everything above cutoff. Creates distance, warmth. Pushes elements back in the mix. Simulates the natural high-frequency rolloff of distance.

**Dynamic EQ**: EQ that activates only when signal exceeds threshold in target band. Surgical without constant spectral change. Use when: a resonance only appears on loud notes, sibilance is inconsistent, or a frequency range only needs treatment sometimes.

**Linear phase EQ**: no phase shift (traditional EQ shifts phase at the processing frequency). Avoids pre-ringing artifacts at low frequencies. Higher latency. Use on master bus or when phase alignment matters (parallel processing, multiband contexts).

### Frequency Ranges & Character

**20-60Hz (Sub-bass)**: felt more than heard. Kick drum fundamental, bass synth sub, low piano. Too much = muddy, undefined, speaker-damaging. Too little = thin, weightless. Most consumer speakers can't reproduce this range. Check on headphones.

**60-200Hz (Bass)**: warmth, body, weight. Bass guitar fundamental, kick body, low vocals, guitar body. Buildup here is the single most common mix problem ("mud"). 80-120Hz: the kick-vs-bass battleground. Decide which instrument owns which frequency.

**200-500Hz (Low mids)**: warmth vs boxiness. Vocal body, snare body, guitar low-mids, piano warmth. Cuts here often improve clarity dramatically. The "problem zone" - most instruments have energy here that competes. 250Hz: classic "muddy" frequency. 400Hz: "boxy" or "cardboard."

**500Hz-2kHz (Midrange)**: presence, intelligibility. Vocal articulation, guitar bite, snare crack, piano attack. The range where musical information is densest. Too much = harsh, nasal, fatiguing. Too little = distant, hollow, behind glass.

**2-4kHz (Upper mids / Presence)**: human hearing most sensitive here (Fletcher-Munson). Vocal clarity and "cut." Guitar edge. Snare wire brightness. Harshness often lives here (2.5-3.5kHz especially). Small boosts create presence; too much creates ear fatigue fast.

**4-8kHz (Presence / Brilliance)**: consonant clarity (sibilance lives here: S, T, CH, SH). Cymbal brightness. Vocal breathiness. De-essing target range. Acoustic guitar pick attack.

**8-12kHz (Brilliance)**: air, sparkle, detail. Hi-hat shimmer, vocal air, acoustic overtones. Adds "expensive" quality to mixes. Naturally attenuated by distance and age.

**12-20kHz (Air)**: extreme top. "Air band." Perceived openness, spaciousness. Naturally rolls off with: analog tape, distance, cables, age-related hearing loss. Often boosted with a wide high shelf for perceived quality.

### EQ Strategy

**Subtractive vs Additive**: cut to remove problems (narrow Q, surgical), boost to enhance character (wide Q, musical). Cutting is generally safer: less likely to cause phase issues, amplify noise, or push levels.

**Frequency masking**: when two instruments occupy the same range, one must yield. Classic approach: decide which instrument "owns" each frequency range. Cut the other there. Kick vs bass: if kick owns 60-80Hz (sub punch), cut bass there and let bass own 80-120Hz (note definition). Vocal vs guitar: vocal owns 1-4kHz presence range, scoop guitar there.

**EQ before or after compression**: EQ before = shapes what the compressor responds to (a low-frequency cut before compression stops bass from triggering excessive gain reduction). EQ after = shapes the compressed sound's tonal balance. Both are valid; the choice depends on intent.

---

## Compression: Dynamic Range Control

### Parameters

**Threshold**: level above which compression engages. Lower threshold = more signal affected. -20dBFS threshold with peaks at -6dBFS = 14dB of signal above threshold. Very different behavior than -10dBFS threshold on the same signal.

**Ratio**: amount of gain reduction above threshold.
- 2:1: gentle leveling. Transparent. 6dB above threshold becomes 3dB above.
- 4:1: moderate compression. Noticeable dynamic control.
- 8:1: heavy compression. Significant squashing.
- 12:1-20:1: limiting territory. Very little signal passes above threshold.
- Infinity:1 (inf:1): brick wall limiter. Nothing passes above threshold.

**Attack**: time for compressor to reach full gain reduction after signal exceeds threshold.
- Fast (0.1-5ms): catches transients. Reduces percussive impact. Tames peaks but can kill punch. Use for: controlling harsh transients, smoothing bass dynamics, transparent peak reduction.
- Slow (10-50ms): lets transients through. Preserves initial attack/punch while controlling sustained body. Use for: drums (preserve hit, control sustain), bass (preserve pluck, control body), anything where the transient IS the musicality.

**Release**: time for compressor to stop compressing after signal drops below threshold.
- Fast (5-50ms): transparent but can cause pumping/breathing (audible gain recovery artifacts, especially on sustained material). Works on fast, percussive material.
- Slow (100-500ms): smoother, more musical, but may not release before next transient (gain reduction accumulates, overall volume drops). Works on sustained material, vocals, bus compression.
- Auto release: program-dependent release time. Adapts to material. Good default for vocals and buses.

**Knee**: transition curve between uncompressed and compressed regions.
- Hard knee: sudden ratio engagement. More aggressive, audible compression.
- Soft knee: gradual ratio onset. More transparent, musical.

**Makeup gain**: compensate for level reduction. The reason compressed signals "sound louder" is makeup gain, not the compression itself. Always level-match before/after to judge compression quality, not loudness.

### Compression Types

**VCA** (SSL G-bus, API 2500): clean, precise, fast. Transparent or punchy depending on settings. Best for: bus compression, drums, precise dynamic control.

**FET** (1176, Distressor): fast, aggressive, colored. Adds harmonic character. "All buttons in" mode = extreme, exciting distortion. Best for: vocals, drums, bass, parallel compression, anything that benefits from energy and character.

**Optical** (LA-2A, LA-3A): slow, smooth, program-dependent. Attack and release determined by light element response (varies with signal). Natural, transparent leveling. Best for: vocals, bass, acoustic instruments, gentle leveling without obvious compression artifacts.

**Variable-Mu / Tube** (Fairchild 670, Manley Vari-Mu): slow, warm, glue. Gentle gain reduction with tube saturation. Best for: bus compression, mastering, mix glue, warmth.

### Compression Techniques

**Parallel compression** (New York compression): blend uncompressed signal with heavily compressed copy. The compressed signal brings up quiet details (upward compression) while the dry signal preserves transients and dynamics. Result: louder perceived sustain without killing peaks. Essential for drums, effective on vocals and bass. Send -> aux track with heavy compression (8:1+, fast attack/release, lots of gain reduction) -> blend to taste.

**Sidechain compression**: compressor triggered by external signal.
- Kick triggers bass compressor: bass ducks on each kick, creating rhythmic space. The kick "pushes through" without EQ conflict.
- Vocal triggers music bus compressor: music gently ducks under vocal, creating clarity without volume automation.
- Sidechain filter: HPF on sidechain input prevents low frequencies from triggering compression (stops kick drum sub from over-compressing the mix bus).

**Serial compression**: multiple compressors in series, each doing gentle gain reduction (2-3dB each). 2-3dB from each of three stages = 6-9dB total reduction, but sounds more transparent than a single compressor doing 9dB because each stage works less hard.

**Multiband compression**: independent compression per frequency band. Use sparingly. Can cause phase issues between bands and unnatural spectral balance shifts. Prefer dynamic EQ for frequency-dependent compression in most cases.

---

## Reverb & Spatial Processing

### Reverb Parameters

**Pre-delay**: time between dry signal and reverb onset. Creates separation between source and its space.
- 0-10ms: source embedded in space (close, intimate).
- 10-30ms: natural room feel.
- 50-100ms: source clearly separated from reverb. Vocal clarity with reverb depth.

**Early reflections**: first discrete reflections from nearby surfaces. Define perceived room size and shape. Critical for spatial realism. Dense early reflections = small room. Sparse = large space.

**Decay time (RT60)**: time for reverb to decrease 60dB.
- 0.3-0.8s: small room, booth.
- 0.8-1.5s: medium room, studio.
- 1.5-3s: large hall.
- 3-6s+: cathedral, massive space.

**Damping**: rate at which high frequencies decay relative to low frequencies. Natural spaces absorb highs faster than lows. High damping = warm, dark reverb tail. Low damping = bright, metallic, shimmering tail.

**Diffusion**: density of late reflections. High = smooth, dense wash. Low = individual echoes audible (flutter, discrete reflections).

### Reverb Types

**Room**: small, short, intimate. Acoustic placement without obvious "reverb" effect.
**Hall**: large, long. Orchestral, cinematic depth and grandeur.
**Plate** (EMT 140): artificial (metal plate vibration). Dense, bright, slightly metallic. Classic on vocals, snare. Cuts through mixes better than natural reverbs.
**Chamber**: dedicated reverberant room (real or simulated). Warm, natural, smooth.
**Spring**: physical spring vibration. Colored, splashy, lo-fi. Guitar amp reverb. Character piece.
**Convolution**: impulse response of real space applied to signal. Most realistic. Less flexible (fixed parameters).
**Algorithmic**: mathematically generated. Fully adjustable. Less realistic, more versatile.
**Shimmer**: pitch-shifted reverb in feedback loop (typically octave up). Ethereal, ambient, otherworldly. Brian Eno, Sigur Ros territory.

### Reverb as Mix Tool

Reverb defines spatial depth. Dry signal = close, intimate. Wet signal = distant, large space. Pre-delay = clarity within depth. Short reverb on vocals + long reverb on instruments = vocal forward, instruments recessed. Different reverb types on different elements = spatial separation (plate on snare, hall on vocals, room on guitars: each occupies different perceived space).

---

## Delay

**Slapback** (50-120ms, single repeat): thickening, doubling, rockabilly. Adds dimension without obvious echo.
**Tape delay**: modulated, degrading repeats. High-end loss and saturation with each repeat. Warm, organic. Space Echo character.
**Digital delay**: clean, precise. Tempo-synced or free.
**Ping-pong**: alternates left/right. Stereo movement and width.
**Multi-tap**: multiple delay times simultaneously. Complex rhythmic patterns.
**Granular delay**: delay line processed granularly. Textural, ambient, experimental. Clouds of repeated fragments.

**Key parameters**: time, feedback (repeat count), filter on repeats (darken = push back in time, brighten = bring forward), modulation (chorus-like movement), mix level.

---

## Saturation & Distortion

### Harmonic Generation
Saturating a signal adds overtones not present in the original.
- **Even harmonics** (2nd, 4th, 6th): warm, pleasant, "musical." Tube circuits emphasize these. Sounds like natural loudness/warmth.
- **Odd harmonics** (3rd, 5th, 7th): aggressive, edgy, buzzy. Transistor clipping emphasizes these. Sounds like distortion/grit.
- **Mix of both**: tape, transformers. Complex, rich, cohesive.

### Saturation Types
- **Tape**: gentle compression + harmonics + high-frequency rolloff + low-frequency emphasis. The "glue" effect. Cohesive, warm. Studer, Ampex character.
- **Tube**: primarily even harmonics, soft clipping curve. Warm, thickening. Subtle to obvious depending on drive level.
- **Transistor**: more odd harmonics, harder clipping. More aggressive. Neve preamp character.
- **Transformer**: iron core saturation. Low-mid thickening, subtle coloration. API, Neve transformer character.
- **Digital clipping**: hard, harsh, flat. Avoid unless deliberately seeking lo-fi/industrial aesthetics.

---

## Stereo Field & Spatial Processing

### Panning
**LCR panning**: hard left, center, hard right only. Forces clarity by eliminating ambiguous "somewhere left" placement. Simple, effective, punchy mixes. Not always appropriate but a powerful discipline.

**Frequency and panning**: bass-heavy elements centered (kick, bass, sub). Low frequencies are omnidirectional in most speaker setups. Off-center bass = unbalanced physical response. Sub below ~80Hz always centered.

### Width Techniques

**Haas effect**: same signal delayed 1-35ms in one channel. Creates perceived width. Source appears on the earlier side. Mono compatibility issues: delay = phase cancellation when summed.

**Mid/Side processing**: encode stereo as Mid (center info, L+R) and Side (width info, L-R). Process independently. EQ the sides differently from center. Compress mid and side separately. Powerful for mastering and bus processing.

**Double tracking**: record the same performance twice, pan each L/R. Natural width from micro-timing and pitch variations between takes. The most natural-sounding stereo width technique.

**Complementary panning**: balance elements by panning counterparts opposite. Guitar L / keys R. Hi-hat L / shaker R. Creates a balanced, wide stereo field.

### Mono Compatibility
Critical. Phone speakers, Bluetooth speakers, some club systems (mono below ~300Hz), some streaming platforms. Always check mixes in mono. Phase issues from stereo widening techniques collapse in mono, causing level drops or cancellation of key elements.

---

## Bus Architecture & Submix Strategy

### Group Buses
Route related tracks to group buses for collective processing:
- **Drum bus**: all drums -> bus compression (2-4dB GR), EQ, saturation. Glues kit together.
- **Bass bus**: bass instruments -> consistent dynamics and tone.
- **Vocal bus**: all vocals -> consistent reverb, compression, EQ treatment.
- **Music bus** (all-minus-vocals): enables vocal-vs-music dynamic control (sidechain, volume balance).
- **Effects return bus**: reverb/delay returns grouped for collective EQ/level control.

### Parallel Processing Buses
Aux sends -> return tracks with heavy processing -> blend with dry signal. Parallel compression, parallel saturation, parallel distortion. The blend knob = the most important parameter.

### Master Bus Processing
Final processing on stereo output. Apply EARLY in the mix process (mix into it, don't add at the end).
Typical chain: gentle bus compression (1-3dB GR, slow attack, auto release) -> broad EQ (gentle shelf adjustments, not surgical) -> optional saturation -> limiter (catch peaks, final loudness).

---

## Mastering Fundamentals

### Purpose
Final preparation for distribution. Translation across playback systems. Consistent loudness across album/playlist. Format-specific delivery.

### Loudness Standards (LUFS)

**Streaming platform targets**:
- Spotify: -14 LUFS integrated
- Apple Music: -16 LUFS integrated
- YouTube: -14 LUFS
- Tidal: -14 LUFS
- Amazon Music: -14 LUFS

**Broadcast**: -24 LUFS (EBU R128), -24 LKFS (ATSC A/85).

**Loudness normalization**: streaming services turn down masters louder than target. Being louder than -14 LUFS on Spotify means your track gets turned DOWN and has LESS dynamic range than a track mastered to -14 LUFS. No advantage to slamming loudness for streaming release.

### Mastering Chain

1. **Corrective EQ**: fix mix problems (resonances, imbalances).
2. **Tonal EQ**: broad character shaping (high shelf for air, low shelf for warmth).
3. **Compression**: gentle (1-3dB GR), glue, cohesion. Slow attack, auto or slow release.
4. **Saturation** (optional): subtle harmonic enhancement, warmth.
5. **Stereo processing**: mid/side EQ, stereo width adjustments.
6. **Limiting**: final loudness. Catches peaks. 2-4dB gain reduction typical for modern masters. True peak ceiling at -1dBTP.
7. **Dithering**: only if reducing bit depth (24-bit -> 16-bit). Apply once, last in chain. Never dither to 24-bit or 32-bit float.

### Metering

- **Peak meters**: instantaneous peaks. Prevent clipping.
- **RMS meters**: average level over time. Approximates perceived loudness.
- **LUFS meters**: K-weighted loudness measurement. Industry delivery standard. Integrated (full program), Short-term (3s window), Momentary (400ms window).
- **True peak meters**: detect inter-sample peaks (peaks between samples that exceed 0dBFS on D/A conversion). Target: -1dBTP for streaming.
- **Spectrum analyzer**: visual frequency content. Identify imbalances.
- **Phase correlation meter**: +1 = mono, 0 = decorrelated, -1 = out of phase. Stay above 0 for mono compatibility.
- **Stereo field display**: visual stereo image representation.

---

## Monitoring & Reference

**Room treatment**: the most impactful investment. Untreated room = inaccurate frequency response = bad decisions. Bass traps in corners (low-frequency absorption), absorption panels at first reflection points (side walls, ceiling), diffusion on back wall.

**Reference tracks**: compare your mix to professional releases in similar genre. Level-match before comparing (louder always sounds "better"). A/B frequently throughout mixing, not just at the end.

**Multiple playback systems**: monitors, headphones, phone speaker, car, earbuds. If it works everywhere, it's a good mix. If it only works on your monitors, your monitors are lying to you (or your room is).

**Mix at moderate volumes**: ear fatigue from loud monitoring causes bad decisions. Fletcher-Munson: bass and treble perception change with SPL. Mix at moderate levels for the most neutral frequency perception. Turn up briefly to check impact, then back down.

---

## Common Pitfalls

- **Solo button disease**: sounds great soloed, disappears in context. Always mix in context of the full arrangement.
- **Over-processing**: every plugin adds complexity. If it's not clearly improving the sound, remove it. The best processing is often none.
- **Ignoring arrangement**: the best mix move is often muting a track. Mixing cannot fix arrangement problems. If two guitars play the same thing in the same register, no amount of EQ will separate them. Remove one.
- **Low-end guessing**: untreated rooms lie about bass. Reference on headphones (even cheap ones) for low-end decisions. Use spectrum analyzers as visual confirmation.
- **Not committing**: endless tweaking. Set a deadline. Print stems. Revisit with fresh ears. Perspective requires distance.

---

## Cross-References

- **Mixing <-> Composition/Orchestration**: frequency masking is the audio engineering equivalent of voice-leading problems. Good arrangement = good pre-mixing. Registral separation in orchestration = frequency separation in mixing.
- **Mixing <-> Synthesis**: understanding spectral content from synthesis parameters informs EQ decisions. If you built the sound, you know its spectrum before reaching for an analyzer.
- **Mixing <-> Acoustic Physics**: room acoustics, speaker behavior, psychoacoustics determine monitoring accuracy. Fletcher-Munson curves, masking, and critical bands directly affect mix decisions.
- **Mixing <-> Audio Analysis**: spectral analysis reveals masking. LUFS metering ensures delivery compliance. Phase measurement catches mono compatibility issues.

See also: REFERENCE_AUDIO_ANALYSIS.md for DSP, psychoacoustics, metering details. SYNTHESIS_SKILL.md for understanding source material.
