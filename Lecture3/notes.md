**Sampling**:

Given a signal $y(t),\ t \in [0,L]$, where $t$ is continuous time measured in seconds and $L$ is the length of the signal.

- Sample rate: $f_{S} = 44100$ Hz.
- Number of samples: $N = \left \lfloor L \cdot f_{S} \right \rfloor$ (floor function, rounds down).
  - Python: `N = int(L * fs)`.
- Time samples: $ts = \{ t_{0}, t_{1},..., t_{N-1} \}$ where $\Delta t = t_{i} - t_{i-1} = \displaystyle\frac{1}{f_{S}}$ is the sampling interval.
  - Python: `ts = np.linspace(0, L, N, endpoint= False)`.
- Sampled values: $ys = \{ y(t_{0}), y(t_{1}), ..., y(t_{N-1}) \}$, or  more succinctly $ys = \{ y[0], y[1],  ..., y[N-1] \}$.

**Aliasing**:

Given a continuous signal. Our discrete, sampled data, is approximate data.

When we only evaluate the signal at discrete times, we lose information on what happened between the samples.

This leads to a frequency ambiguity known as <u>aliasing</u>.

<img src='aliasing.PNG'>

Let $f_O$ be the observed frequency; $f_{S}$, the sampling rate and $f$, the actual frequency of the signal.

- If $-\displaystyle\frac{f_S}{2} < f < \frac{f_S}{2}$, then $f$ is not aliased: $f = f_O$.
- Otherwise, that is $f \notin \bigg[ -\displaystyle\frac{f_S}{2}, \frac{f_S}{2} \bigg]$, $f$ is aliased back into it: $f_O \in \bigg[ -\displaystyle\frac{f_S}{2}, \frac{f_S}{2} \bigg] : f_{0} = f + k \cdot f_{S},\ k \in \Z$.

**The Sampling Theorem**:

A signal $y(t), \ t \in [0,L]$ can be perfectly reconstructed from its samples taken at the sampling rate $f_{S}$, provided that the signal only contains frequencies less than $\displaystyle\frac{f_{S}}{2}$.

The frequency $\displaystyle\frac{f_{S}}{2}$ is called the <u>Nyquist frequency</u>.

**Reconstruction of a signal**:

Consider a 1-second signal given by:
$$
y(t) = \sin (2\pi \cdot 1t) + \sin (2\pi \cdot 3t) + \sin(2\pi \cdot 4t),\ t \in [0,1]
$$
with frequencies $1$ Hz, $3$ Hz and $4$ Hz.

Since the highest frequency component is $4$ Hz, the sampling theorem says that this signal can be reconstructed without information loss if we sample, for example at the sampling rate  of $10$ Hz.

**Spectrum of a real signal is symmetric**:

The spectrum of every real signal $y(t),\ t \in [0,L]$ is symmetric around $0$ Hz. That is, if a frequency $f$ Hz is present in the signal, so is the frequency $-f$ Hz.

- The signal $y(t) = \sin (2\pi \cdot 1t) + \sin (2\pi \cdot 3t) + \sin(2\pi \cdot 4t),\ t \in [0,1]$ contains the frequencies $\{ -4 \textrm{ Hz}, -3 \textrm{Hz}, -1 \textrm{Hz}, 1 \textrm{ Hz}, 3 \textrm{ Hz}, 4 \textrm{ Hz} \}$.

**The Discrete Fourier Transform**:

If we sample a signal at a sampling rate $f_{S}$, the DFT can only detect frequencies between $-f_{S}/2$ and $f_{S}/2$.

Because a real signal has a symmetric spectrum. The DFT can detect frequencies between $0$ and $f_{S}/2$.

- Python: `np.fft.rfft(samples)`. [More here](https://docs.scipy.org/doc/numpy/reference/generated/numpy.fft.rfft.html).

**Distortion of Sound Recording**:

Suppose an Analog to Digital Converter (ADC) is recording a violin at the sampling rate $f_{S} = 10 \textrm{ kHz } = 10,000 \textrm{ Hz}$. A tone with fundamental frequency of $750 \textrm{ Hz}$ is played.

The harmonics are $750$, $1500$, $2250$, $3000$, $3750$, $4500$, $5250$, $6000$, $6750$, $7500$, $8250$, $9000$, $9750$.

<u>Nyquist frequency</u>: $\displaystyle\frac{f_{S}}{2} = \frac{10000}{2} = 5000 \textrm{ Hz}$.

Harmonics above the Nyquist frequency are aliased:

- $5250 \rightarrow -4750 \textrm{ Hz}$ ($k = -1$).
- $6000 \rightarrow -4000 \textrm{ Hz}$ ($k = -1$).
- etc.

The ear cannot distinguish between negative and positive frequencies.

- Ignoring the sign and rearranging the harmonics: $750$, $1500$, $2250$, $3000$, $3750$, $4500$ and $250\ (9750)$, $1000\ (9000)$, $1750\ (8250)$, $2500\ (7500)$, $3250\ (6750)$, $4000\ (6000)$, $4750\ (5250)$. 

- These are no longer integer multiples of the fundamental frequency of $750 \textrm{ Hz}$. Thus the tone we hear in the recording is distorted.

**Harmonics**:

Consider the interval $[0,L]$ measured in seconds.

- Define the frequency $f_{1} = 1/L$.
- This frequency is called the <u>fundamental frequency</u> for the interval $[0,L]$. Sinusoids, both real and complex, with frequency $f_{1}$ complete the cycle in the interval $[0,L]$.
  - Note that, since $N = f_{S}/L$, we also have $f_{1} = 1/L = f_{S}/N$.

Consider the following set of $N$ integer multiples of the fundamental frequency $f_{1}$.
$$
\{ f_{0}=0, f_{1}=1/L, f_{2} = 2f_{1}, ..., \boldsymbol{f_{S}/2}, \textcolor{red}{..., f_{N-1} = (N-1)f_{1} } \}
$$

- In red are the aliased frequencies.

These are known as the <u>harmonics</u>. The DFT will try to detect the presence of these frequencies in the audio signal given by $y(t),\ t \in [0,L]$.

**Why 44100 Hz**?

Range of human hearing is between $20 \textrm{ Hz}$ and $20000 \textrm{ Hz}$.

To guarantee that every frequency in this range is properly recorded, a sampling rate of at least $40000 \textrm{ Hz}$ is necessary.

- CD quality standard sampling rate is $44100 \textrm{ Hz}$.
- Before sampling is taken, a [low-pass filter](https://en.wikipedia.org/wiki/Low-pass_filter) is applied to remove all frequencies outside of the Nyquist frequency range. This prevents aliasing and corruption of the recording.
  - These low-pass filters are also called <u>anti-aliasing filters</u>.

