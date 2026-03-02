# Audio Analysis, DSP & Psychoacoustics Reference

## Philosophy

Understanding frequency content, spectral relationships, and acoustic physics informs both analysis and synthesis. Spectral analysis is compositional insight: Grisey analyzed bell spectra and orchestrated the partials across an ensemble. FFT output is timbre rendered visible. Psychoacoustics explains why certain combinations mask, why loudness perception is nonlinear, and why spatial processing works. This knowledge bridges physics, perception, and musical decision-making.

---

## Digital Audio Fundamentals

### Sampling

**Sample rate**: measurements per second (Hz). Captures amplitude at discrete time intervals.
- 44.1kHz: CD standard. Captures up to 22.05kHz (above human hearing threshold ~20kHz).
- 48kHz: video/broadcast standard. Captures up to 24kHz.
- 96kHz, 192kHz: high-resolution. Marginal audible benefit debated. Practical benefit: more headroom for processing (oversampling occurs naturally), less aliasing from nonlinear processing.

**Nyquist theorem**: maximum representable frequency = sample rate / 2 (Nyquist frequency). Frequencies above Nyquist cannot be captured and alias (fold back as false frequencies). Anti-aliasing filter in the ADC removes content above Nyquist before sampling.

**Aliasing**: frequency above Nyquist appears as a false lower frequency. f_alias = |f_signal - n * f_sample| where n is the nearest integer. Example: 25kHz signal sampled at 44.1kHz aliases to 19.1kHz (44.1 - 25 = 19.1). Sounds wrong, cannot be removed post-sampling. Oversampling in plugins (2x, 4x, 8x): temporarily increases internal sample rate to push Nyquist higher, preventing aliasing from nonlinear processing (distortion, saturation, compression).

### Bit Depth

**Bit depth**: precision of each sample measurement. More bits = more possible amplitude values = lower noise floor.
- 16-bit: 65,536 values. ~96dB dynamic range. CD standard.
- 24-bit: 16,777,216 values. ~144dB dynamic range. Professional recording standard. Noise floor well below audibility.
- 32-bit float: floating point representation. Effectively unlimited dynamic range within the DAW (mantissa + exponent representation means clipping on individual channels is mathematically impossible in practice). Standard for modern DAW internal processing.

**Quantization error**: rounding continuous amplitude to nearest available value creates distortion. More bits = smaller error = lower noise floor. At 16-bit, the quantization noise floor is at -96dBFS. At 24-bit, it's at -144dBFS (below the noise floor of any real-world analog equipment).

**Dithering**: adding very low-level random noise before bit-depth reduction. Converts quantization distortion (harmonically related to signal, sounds harsh) into broadband noise (sounds benign). Apply once, at the very last stage, only when reducing bit depth (24->16 for CD). Never dither to 24-bit or 32-bit float. Common dither types: TPDF (triangular probability density function, flat noise), noise-shaped dither (psychoacoustically optimized, pushes noise energy to less-audible frequencies).

### Digital-to-Analog Conversion (DAC)

Reconstructs continuous signal from discrete samples. Reconstruction filter smooths staircase output. DAC quality affects final audio: clock jitter (timing inaccuracy) creates noise and distortion. High-quality DAC = lower jitter, better reconstruction filtering, lower noise floor.

---

## Fourier Analysis & FFT

### Fourier Theorem
Any periodic signal can be decomposed into a sum of sine waves at specific frequencies, amplitudes, and phases. This is the mathematical foundation of spectral analysis. A violin playing A4 (440Hz) is not a single frequency: it is 440Hz + 880Hz + 1320Hz + 1760Hz... each at specific amplitudes. The relative amplitudes of these harmonics define the violin's timbre.

### DFT (Discrete Fourier Transform)
Computes frequency content of a discrete (sampled) signal. Input: N time-domain samples. Output: N/2+1 frequency bins, each with magnitude (how much energy at that frequency) and phase (where in the cycle the sine wave starts).

### FFT (Fast Fourier Transform)
Efficient algorithm for computing DFT. Reduces computational complexity from O(N^2) to O(N log N). Requires N = power of 2 (256, 512, 1024, 2048, 4096, 8192). THE standard algorithm for real-time spectral analysis.

### FFT Parameters & Tradeoffs

**Window size (N)**: number of samples per analysis frame.
- Larger N = better frequency resolution (can distinguish closer frequencies), worse time resolution (blurs temporal events).
- Smaller N = better time resolution (captures transients), worse frequency resolution (blurs close frequencies).
- This tradeoff is the fundamental uncertainty principle of spectral analysis. You cannot simultaneously have perfect time AND frequency resolution.

**Frequency resolution**: sample_rate / N.
- At 44.1kHz, N=1024: 43.1Hz per bin. Cannot distinguish pitches closer than 43Hz apart. Adequate for high frequencies but insufficient for low-register pitch detection.
- At 44.1kHz, N=4096: 10.8Hz per bin. Better pitch resolution.
- At 44.1kHz, N=16384: 2.7Hz per bin. Precise pitch detection even at low frequencies but very slow (371ms per frame).

**Time resolution**: N / sample_rate.
- N=1024 at 44.1kHz: 23ms per frame.
- N=4096: 93ms per frame. A drum hit lasting 10ms is blurred across a 93ms window.
- Tradeoff: long windows see frequency detail, short windows see temporal detail.

**Window function**: applied to the signal before FFT to reduce spectral leakage (artifacts from analyzing signals that don't fit exactly in integer periods within the window).
- Rectangular (no window): maximum leakage. Only use for signals that perfectly fit the window period.
- Hanning (Hann): general purpose. Good frequency resolution, good sidelobe rejection. Standard default.
- Hamming: slightly different sidelobe pattern than Hanning. Common alternative.
- Blackman: better sidelobe rejection (less leakage) but wider main lobe (lower frequency resolution). Good for spectral analysis where accuracy matters more than resolution.
- Kaiser: adjustable parameter trades main lobe width against sidelobe level.

**Overlap**: consecutive FFT frames overlapped (typically 50-75%) for smoother time resolution and better temporal tracking. Hop size = N - overlap (hop = distance between frame starts). 75% overlap with N=4096 = hop size of 1024 samples = ~23ms between frames.

**Zero-padding**: appending zeros to increase FFT size without additional data. Interpolates between frequency bins (smoother spectral display) but does NOT add real frequency resolution. Cosmetic improvement only.

### STFT (Short-Time Fourier Transform)
FFT applied to successive overlapping windows. Creates spectrogram: time-frequency representation. The standard tool for visualizing spectral evolution.

### Spectrogram
2D representation: time on x-axis, frequency on y-axis, amplitude as color/brightness. Shows harmonics, transients, noise floor, formants, spectral evolution. The visual equivalent of listening to timbre.

---

## Spectral Analysis for Composition

### Harmonic Analysis

**Harmonic series**: integer multiples of fundamental frequency.
- Fundamental (f): 1st harmonic. The perceived pitch.
- 2f: octave. 2nd harmonic.
- 3f: octave + perfect fifth. 3rd harmonic.
- 4f: two octaves. 4th harmonic.
- 5f: two octaves + major third. 5th harmonic.
- 6f: two octaves + perfect fifth. 6th harmonic.
- 7f: two octaves + minor seventh (slightly flat of equal temperament). 7th harmonic.
- 8f: three octaves. 8th harmonic.

The harmonic series explains consonance: octave (2:1), fifth (3:2), fourth (4:3), major third (5:4) are the simplest ratios and the most consonant intervals. More complex ratios = more dissonance.

**Harmonic vs inharmonic spectra**: instruments with periodic vibration (strings, air columns) produce harmonic spectra (integer multiples). Non-periodic vibrators (bells, bars, membranes) produce inharmonic spectra (non-integer multiples). Piano strings exhibit slight inharmonicity: stiffness causes upper partials to be progressively sharper than true harmonics.

**Noise content**: broadband energy below harmonic peaks. More noise = breathier timbre. Flute has more noise than clarinet. Bowed string at low bow pressure = more noise. Noise content is a significant timbral characteristic.

### Spectral Features

**Spectral centroid**: weighted average frequency of the spectrum (brightness metric). Higher centroid = brighter sound. Track centroid over time = brightness contour. Useful for comparing timbres, tracking timbral evolution, or measuring the effect of filter processing.

**Spectral flux**: rate of spectral change between consecutive frames. High flux = rapidly changing timbre (attacks, transients). Low flux = stable timbre (sustain, drones). Primary feature for onset detection.

**Spectral rolloff**: frequency below which a specified percentage (typically 85%) of spectral energy is concentrated. Lower rolloff = darker sound with energy concentrated in lower frequencies.

**Spectral envelope**: shape connecting the peaks of harmonic amplitudes. Defines perceived timbre independently of pitch. Vocal formants = fixed spectral envelope peaks that define vowel sounds regardless of sung pitch. Formant filtering in synthesis preserves timbral identity across pitch range.

**Spectral centroid, flux, rolloff, and envelope are the primary features used in audio classification, instrument recognition, and MIR (Music Information Retrieval).**

---

## Psychoacoustics

### Fletcher-Munson Curves (Equal-Loudness Contours)
Human hearing sensitivity varies with frequency and SPL level.
- Most sensitive: 2-5kHz (vocal formant range, evolutionary advantage for speech perception).
- Least sensitive: extreme low frequencies (below 100Hz) and extreme high frequencies (above 10kHz).
- At low listening levels: bass and treble perception drops significantly relative to midrange. A mix that sounds balanced at 85dB SPL will sound thin and mid-heavy at 60dB SPL.
- Implication: mix at moderate levels (around 75-85dB SPL) for the most neutral frequency perception. Brief loud checks for impact, then back to moderate.

### Critical Bands (Bark Scale)
The ear's frequency analysis uses approximately 24 overlapping bands (the cochlea's physical structure determines this). Critical bandwidth: ~100Hz below 500Hz, approximately 20% of center frequency above 500Hz.

- Two tones within the same critical band: roughness, beating, dissonance.
- Two tones in adjacent bands: moderate dissonance.
- Two tones separated by more than one critical band: consonance, clarity.

**Implication for mixing**: instruments sharing a critical band compete perceptually (masking). EQ carving must account for critical band width, not just frequency overlap.

### Masking

**Simultaneous masking**: a louder sound hides quieter sounds in nearby frequency range. The masking effect extends more upward in frequency than downward. A 1kHz tone at 80dB masks a 1.2kHz tone at 60dB but barely masks a 800Hz tone at 60dB.

**Temporal masking**: loud sounds mask quieter sounds before (pre-masking, ~5-20ms) and after (post-masking, ~50-200ms). Post-masking is stronger than pre-masking. A loud snare hit masks quiet details immediately following it.

**Informational masking**: similar-sounding streams interfere with each other perceptually even when they don't overlap spectrally. Two simultaneous speech streams mask each other more than speech + noise at the same level.

**Implications for mixing**: frequency masking = instruments fighting for same spectral space. EQ carving = unmasking. Temporal masking = quiet details after loud transients are inaudible (don't fight this; use it). Arrangement decisions (instrument selection, register assignment) are pre-mixing unmasking decisions.

### Loudness Perception

- Logarithmic: 10dB increase = approximately twice perceived loudness.
- 3dB = minimum clearly noticeable difference in A/B comparison.
- 1dB = threshold of perception for trained listeners in direct comparison.
- 0.5dB = below reliable perception for most listeners.

**Implication**: when A/B comparing two versions (with/without a plugin, two mix revisions), level-match to within 0.5dB. Otherwise you're comparing loudness, not quality.

### Spatial Perception

**ITD (Interaural Time Difference)**: time delay between ears. Maximum ~0.7ms for sounds at 90 degrees. Effective for localization below ~1500Hz (wavelength larger than head).

**ILD (Interaural Level Difference)**: level difference between ears due to head shadow. Effective above ~1500Hz (wavelength smaller than head, head blocks sound).

**HRTF (Head-Related Transfer Function)**: frequency-dependent filtering by head, pinna, shoulders. Provides elevation and front/back cues. Unique to each person (why headphone spatial audio requires personal HRTF measurement for accuracy).

**Precedence effect (Haas effect)**: first arriving sound determines perceived location, even if a delayed copy is equally loud (within ~5-35ms window). Delays beyond ~35ms are perceived as distinct echoes. Used in stereo widening (Haas effect panning) and sound reinforcement (delay stacking in live sound).

### Psychoacoustic Phenomena

**Beating**: two tones close in frequency create periodic amplitude fluctuation at rate = frequency difference. 1-4Hz beating = pleasant vibrato-like. 15-30Hz = roughness/dissonance (critical band interaction). Used compositionally: detuning oscillators by 1-3Hz creates "analog warmth" from slow beating.

**Combination tones**: two loud tones produce phantom tones at sum and difference frequencies. Most audible: difference tone (f2-f1). Tartini tones. Organ builders exploit this: two pipes at 200Hz and 300Hz produce a perceived 100Hz difference tone, simulating a pipe twice as long.

**Missing fundamental**: brain perceives the fundamental pitch even when only upper harmonics are present. Why small speakers convey bass pitch despite not reproducing sub frequencies. The harmonics present (200, 300, 400Hz) imply a 100Hz fundamental, and the brain "hears" it.

**Octave equivalence**: pitches separated by octave perceived as "same note" in different register. Universal across cultures. Pitch perception = chroma (note name) + height (register).

---

## Audio Measurement

### Level Metering

**dBFS** (decibels Full Scale): digital level relative to maximum. 0dBFS = maximum before clipping. All values negative. The digital reference.

**dBSPL** (decibels Sound Pressure Level): acoustic level. 0dBSPL = threshold of hearing (~20 micropascals). 85dBSPL = recommended limit for extended monitoring. 120dBSPL = threshold of pain.

**dBu, dBV**: analog voltage references. 0dBu = 0.775V (into 600 ohm load, telephony standard). 0dBV = 1V. Important for hardware interfacing and calibrating analog-digital chain. 0VU typically calibrated to -18dBFS or -14dBFS depending on studio standard.

### Loudness Metering (LUFS/LKFS)

**K-weighting**: frequency weighting curve applied before loudness measurement. Boosts 2-4kHz region (ear sensitivity peak), rolls off below 200Hz. Better correlation with perceived loudness than flat RMS measurement.

- **Integrated LUFS**: average loudness over entire program. THE delivery specification for streaming and broadcast.
- **Short-term LUFS**: 3-second sliding window. Shows loudness trends within a piece.
- **Momentary LUFS**: 400ms sliding window. Shows instantaneous perceived loudness.
- **Loudness Range (LRA)**: statistical measure of dynamic range. Difference between soft and loud passages (excluding extremes). Higher LRA = more dynamic range.
- **True Peak (dBTP)**: maximum level between samples (reconstructed analog waveform peak). Digital samples can be below 0dBFS while the reconstructed analog waveform between samples exceeds 0dBFS (inter-sample peak). Streaming target: -1dBTP to prevent clipping during D/A conversion.

### Frequency Analysis

**RTA (Real-Time Analyzer)**: displays frequency content in real time. Octave-band or 1/3-octave-band resolution. Pink noise through system + RTA = frequency response measurement.

**Spectrum analyzer**: higher-resolution FFT-based display. Shows individual frequency components. More detail than RTA.

**Transfer function measurement**: compares input to output. Shows frequency response (magnitude vs frequency), phase response (phase shift vs frequency), coherence (signal-to-noise at each frequency). Used for speaker calibration, room measurement, plugin verification.

### Phase Measurement

**Phase correlation meter**: measures phase relationship between left and right channels.
- +1: perfectly correlated (mono). Left and right identical.
- 0: completely uncorrelated. No relationship between channels.
- -1: perfectly out of phase. Left and right are inverted copies. Complete cancellation in mono sum.
- Healthy stereo mix: stays mostly above +0.3. Dips toward 0 are normal during wide stereo passages. Sustained negative values = mono compatibility problem.

**Group delay**: rate of change of phase with frequency. Non-uniform group delay = time smearing (different frequencies arrive at different times, blurring transients). Minimum-phase EQ introduces group delay. Linear-phase EQ avoids this but introduces pre-ringing and higher latency.

**Polarity vs phase**: polarity = 180-degree flip (all frequencies inverted simultaneously). Phase = frequency-dependent rotation. Polarity flip: a cable wired backwards, a reversed mic. Phase shift: what a filter or speaker crossover introduces. Often confused in practice.

---

## Computational Audio Analysis (Librosa)

### Overview
Librosa (Python): standard library for audio feature extraction and Music Information Retrieval. Provides tools for spectral analysis, rhythm analysis, pitch detection, and feature extraction for machine learning.

### Key Functions

**Loading & Representation**:
- `librosa.load(path, sr=22050)`: load audio file. Returns waveform array + sample rate. Default resamples to 22050Hz (sufficient for most analysis; halves computation).
- `librosa.stft(y, n_fft=2048, hop_length=512)`: Short-Time Fourier Transform. Returns complex matrix (magnitude + phase). n_fft = window size. hop_length = distance between frames.
- `librosa.amplitude_to_db(S)`: convert amplitude spectrogram to decibel scale for visualization.

**Spectral Features**:
- `librosa.feature.spectral_centroid()`: brightness over time.
- `librosa.feature.spectral_bandwidth()`: spectral width over time.
- `librosa.feature.spectral_rolloff()`: frequency below which X% energy concentrated.
- `librosa.feature.spectral_contrast()`: difference between peaks and valleys per sub-band. Useful for distinguishing music from speech.
- `librosa.feature.zero_crossing_rate()`: sign-change rate. Higher = noisier/brighter.

**Pitch & Harmony**:
- `librosa.feature.chroma_stft()`: chromagram - 12 pitch classes over time. Maps all frequency content to the 12 semitones. Shows harmonic content regardless of octave. Useful for chord recognition, key detection.
- `librosa.feature.chroma_cqt()`: constant-Q chromagram. Better frequency resolution at low frequencies than STFT-based chroma.
- `librosa.feature.tonnetz()`: tonal centroid features. Maps harmonic relationships into a 6-dimensional space representing fifths, minor thirds, and major thirds. Useful for key and chord analysis.
- `librosa.core.piptrack()`: pitch tracking via harmonic peak detection.

**Timbre & MFCCs**:
- `librosa.feature.mfcc(y, sr, n_mfcc=13)`: Mel-Frequency Cepstral Coefficients. The standard compact timbral representation. 13-20 coefficients capture the spectral envelope shape. MFCC1 = overall energy. MFCC2 = balance between low and high frequencies. Higher coefficients = finer spectral detail. Standard features for: instrument classification, genre classification, speaker recognition, audio similarity.
- `librosa.feature.melspectrogram()`: mel-scaled spectrogram. Frequency axis warped to match human pitch perception (mel scale: perceptually uniform pitch spacing).

**Rhythm & Onset**:
- `librosa.beat.beat_track(y, sr)`: BPM estimation and beat frame positions. Uses onset strength envelope + autocorrelation.
- `librosa.onset.onset_detect()`: find note/event onsets using spectral flux. Returns frame indices of detected onsets.
- `librosa.onset.onset_strength()`: onset strength envelope over time. Peaks = likely onsets.
- `librosa.feature.tempogram()`: local tempo estimate over time. Shows tempo variations and rhythmic structure.

**Source Separation**:
- `librosa.effects.harmonic(y)`: extract harmonic (tonal) component via median filtering of spectrogram.
- `librosa.effects.percussive(y)`: extract percussive (transient) component.
- HPSS (Harmonic-Percussive Source Separation): fundamental preprocessing step. Separate tonal content (for pitch/chord analysis) from transients (for rhythm analysis).
- `librosa.decompose.decompose(S, n_components)`: NMF (Non-negative Matrix Factorization) decomposition of spectrogram into components. Separates mixture into basis spectra + activation patterns.

### Practical Workflow

1. **Load**: `y, sr = librosa.load(path)`
2. **Visualize**: compute STFT, convert to dB, display as spectrogram
3. **Separate**: HPSS for harmonic vs percussive content
4. **Extract rhythm**: onset detection + beat tracking on percussive component
5. **Extract harmony**: chroma features on harmonic component -> chord/key estimation
6. **Extract timbre**: MFCCs for overall timbral character, spectral centroid for brightness
7. **Segment**: use onset/beat information to segment into structural sections
8. **Classify/Compare**: use extracted features as input to classification or similarity algorithms

---

## Acoustic Physics

### Sound Propagation
Longitudinal pressure waves through air. Speed: ~343 m/s at 20C (varies with temperature, humidity, altitude).

**Wavelength** = speed / frequency:
- 20Hz = 17.15m (large room-scale)
- 100Hz = 3.43m (corner to corner in small room)
- 1kHz = 34.3cm (about the width of a speaker)
- 10kHz = 3.43cm (width of a finger)
- 20kHz = 1.72cm

Low frequencies diffract around obstacles (wavelength larger than obstacle = wave bends around it). High frequencies are directional (wavelength smaller than obstacle = blocked, reflected, absorbed). This is why bass is hard to control acoustically and why high-frequency speakers (tweeters) beam.

### Resonance & Room Modes
Every enclosed space has resonant frequencies (room modes) where standing waves form. Mode frequency = speed_of_sound / (2 * room_dimension). A room 3.43m long has a fundamental mode at 50Hz. At this frequency, the room amplifies the sound at some positions and cancels it at others (nodes and antinodes).

Room modes create: bass peaks and nulls at different listening positions, uneven low-frequency response, and the single biggest acoustic problem in untreated small rooms. Bass traps (absorptive material in corners) reduce mode energy.

### Formants
Fixed resonant frequencies of a cavity. The vocal tract has formants that define vowel sounds:
- F1 (~300-800Hz): open/close quality. Low F1 = closed vowel (ee). High F1 = open vowel (ah).
- F2 (~800-2500Hz): front/back quality. High F2 = front vowel (ee). Low F2 = back vowel (oo).
- F3 (~2500-3500Hz): contributes to vowel color and speaker identity.

Formants are independent of fundamental pitch. A soprano and bass singing "ah" have different fundamentals but similar formant frequencies. This is why we recognize the same vowel across different voices. Instrument bodies also have formants: the resonant peaks of a violin body shape its timbre regardless of the note played.

### Absorption & Reflection
Sound energy hitting a surface is partially absorbed, partially reflected, partially transmitted.
- Soft/porous materials (foam, curtains, fiberglass): absorb mid-high frequencies well. Thickness determines low-frequency absorption depth.
- Hard/dense materials (concrete, glass, tile): reflect efficiently across frequency range.
- Membrane absorbers (tuned panels): absorb at specific low frequencies via resonance.
- Helmholtz resonators: tuned cavity absorbers for specific narrow frequency bands.
- Diffusion (irregular surfaces, diffuser panels): scatter reflections in time and direction without absorbing energy. Preserves room liveliness while reducing distinct echoes.

---

## Cross-References

- **Audio analysis <-> Synthesis**: spectral content of target sound guides synthesis approach. FFT analysis -> additive resynthesis. Spectral centroid tracking -> filter envelope design.
- **Audio analysis <-> Mixing**: frequency analyzer reveals masking. LUFS metering ensures delivery compliance. Phase measurement catches mono compatibility problems.
- **Audio analysis <-> Composition**: spectral analysis is the foundation of spectral music (Grisey, Murail). Understanding harmonic series = understanding consonance at physical level. Critical bands explain dissonance.
- **Audio analysis <-> Psychoacoustics**: Fletcher-Munson explains volume-dependent frequency perception. Masking explains why EQ carving helps clarity. Temporal masking explains why quiet details after loud transients vanish.

See also: SYNTHESIS_SKILL.md for synthesis applications. MIXING_SKILL.md for practical engineering applications.
