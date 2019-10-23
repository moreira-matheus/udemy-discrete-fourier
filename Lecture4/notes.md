#### The Discrete Fourier Transform

Given an analog signal $y(t),\ t \in [0,L]$, sample it at the rate $f_{S}$ to obtain $f_{S}L = N$ samples.

The DFT is a <u>frequency detector</u>.

Let $f_{1} = 1/L = f_{S}/N$ be the fundamental frequency.

The DFT detects the presence of the following frequencies in the signal $f_{k} = k\cdot f_{1},\ k = 0, 1, ..., (N-1)$.

The DFT converts the sequence of $N$ samples $\big[ y[0], y[1], ..., y[N-1] \big]$ into another sequence of $N$ complex numbers called <u>Fourier coefficients</u> $\big[ Y[0], Y[1], ...,  Y[N-1] \big]$ where $Y[k]$ expresses the degrees to which the frequency $f_{k} = kf_{1}$ is present in the signal.
$$
Y[k] = \displaystyle\sum_{n=0}^{N-1} y[n] \exp\{-i2\pi kn/N\},\quad k=0, 1, ..., (N-1)
$$
The <u>Inverse Discrete Fourier Transform</u> (IDFT) converts every $Y[k]$ back into $y[k]$:
$$
y[n] = \displaystyle\frac{1}{N}\sum_{k=0}^{N-1} Y[k] \exp\{i2\pi kn/N\}, \quad n=0,1,...,(N-1)
$$
The magnitude $|Y[k]|$ measures the magnitude of the frequency $f_{k} = k f_{1}$ in the signal via $a_{k} = \displaystyle\frac{2|Y[k]|}{N}$, where $a_{k}$ is the peak amplitude of sinusoid of frequency $f_{k}$.

The [argument](https://en.wikipedia.org/wiki/Argument_(complex_analysis)) of $Y[k]$ indicates the phase of the sinusoid of frequency $f_{k}$ with respect to the cosine function.

**Applications of the Inverse DFT**:

As long as the original has no frequency component higher than the Nyquist frequency, the IDFT allows us to perfectly reconstruct the original.

This means that a signal is determined by its frequency content.

- JPEG, MP3 and MPEG uses DFT to compress pictures, audio and video files, respectively.
- Filter out noise or unwanted frequency.

**Complex exponentials**:

When $z = i\theta$ is purely imaginary, it can be shown that $e^{i\theta} = \cos(\theta) + i\sin(\theta)$.

- This is <u>Euler's identity</u>.
- $e^{i\theta}$ is the <u>complex exponential</u>.

Note that, since $\sin^2(\theta) + \cos^2(\theta) = 1,\ \forall\ \theta \in \R$, then $z=e^{i\theta} = \cos(\theta) + i\sin(\theta)$ lies on the unit circle on the complex plane.

**Complex sinusoids**:

Recall that real sinusoids with frequency $f \textrm{ Hz}$ have the forms $y(t) = a\sin(2\pi ft)$ and $y(t) = a\cos(2\pi ft)$.

A <u>complex sinusoid</u> with frequency $f \textrm{ Hz}$ has the form:
$$
y = e^{i2\pi ft} = \cos(2\pi ft) + i \sin(2\pi ft), \quad t \in [0,\infty)
$$

- Note that a complex sinusoid is a complex-valued function: for each $t$, the output $y(t)$ is a complex number.

- It traces out the curve that is the unit circle on the complex plane at the frequency $f$ cycles per second ($\textrm{Hz}$).

- If $f > 0$, the curve is traced in the <u>counterclockwise direction</u>; if $f < 0$, it is traced in the <u>clockwise direction</u>.

**Why does the DFT work** (A geometric explanation)

[Dot product](https://en.wikipedia.org/wiki/Dot_product) measures similarity.

- Consider $u = (1,1)$. Consider the three vectors: $v_1 = (3,4)$, $v_2 = (5,0)$ and $v_3 = (4,-3)$.
- The dot products:
  - $u\cdot v_1 = 1\cdot 3 + 1\cdot 4 = 7$;
  - $u\cdot v_2 = 1\cdot 5 + 1\cdot 0 = 5$;
  - $u\cdot v_3 = 1\cdot 4 + 1\cdot (-3) = 1$.

Basis vectors (x-y plane example):

- $\vec{u} = (1,0), \vec{v} = (0,1)$.
  - Building blocks for all other vectors.
  - $\vec{w} = (3,5) = 3\vec{u} + 5\vec{v}$.
    - $\vec{w} \cdot \vec{u} = 3 \cdot 1 + 5 \cdot 0 = 3$.
    - $\vec{w} \cdot \vec{v} = 3 \cdot 0 + 5 \cdot 1 = 5$.
  - The dot products $\vec{w} \cdot \vec{u}$ e $\vec{w} \cdot \vec{v}$ are scalar projections.

Analogously, DFT = Projection to sinusoids.

- $(e^{-i2\pi k 0/N},e^{-i2\pi k 1/N},...,e^{-i2\pi k (N-1)/N}),\ k \in \{0,1,2,...,(N-1)\}$ are basis vectors for the space of all $N$ samples.
- DFT: dot product of $\{y[n]\}$ with sinusoid of frequency $f_{k}$.

**Reconstruction of a Signal with IDFT**:

The signal:
$$
y = 3\sin(2\pi \cdot 10t) + 2\cos(2\pi\cdot 20t + \pi/4), \quad t \in [0, 1/10]
$$
Sample at $80 \textrm{ Hz}$ for $L = 1/10$ seconds ($8$ samples):
$$
y[n] : [ 1.41, 0.71, 1.59, 3.54, 1.41, -3.54, -4.41, -0.71 ]
$$
DFT yields:
$$
Y[n]: [ 0, -12i, 5.66 + 5.66i, 0, 0, 0, 5.66-5.66i, 12i ]
$$
Taking the magnitudes:
$$
|Y[n]| : [ 0, 12, 8, 0, 0, 0, 8, 12 ]
$$
Frequency in Herz: $[ 0, 10, 20, 30, 40, -30, -20, -10]$.

The amplitude is given by $a_k = \displaystyle\frac{2|Y[k]|}{N}$:

- Then, $a_1 = \displaystyle\frac{2 \cdot 12}{8} = 3$ (coefficient of the sine function).
  - From the list of frequencies, we know it is related to $f_k = 10\textrm{ Hz}$.
- The argument $\arg(Y[1]) = \arg(12i) = -\displaystyle\frac{\pi}{2}$.
  - The argument yields the phase of the sinusoid with respect to the <u>cosine</u> function.
  - Phasing the cosine function by $-\frac{\pi}{2}$, one obtains the <u>sine</u> function.
  - Putting it all together: $a_k \cdot \cos(2\pi \cdot f_k t + \phi) = 3\cos(2\pi\cdot10t -\pi/2) = 3\sin(2\pi\cdot 10t)$.
- Then, $a_2 = \displaystyle\frac{2 \cdot 8}{8} = 2$ (coefficient of the cosine function).
  - This is the coefficient related to $f_k = 20$.
- The argument $\arg(Y[2]) = \arg(5.66 + 5.66i) = \displaystyle\frac{\pi}{4}$.
  - Putting it all together: $2\cos(2\pi\cdot 20t + \pi/4)$.
- The other frequency components are either the (aliased) negative equivalent frequencies ($-10, -20, -30$) or have zero magnitude associated with them ($0, 30, 40$).

