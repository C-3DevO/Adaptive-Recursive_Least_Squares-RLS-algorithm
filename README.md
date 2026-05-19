# Adaptive Channel Equalization using LMS and RLS

This project implements and compares Least Mean Squares (LMS) and Recursive Least Squares (RLS) adaptive equalizers for QPSK communication systems affected by multipath fading, inter-symbol interference (ISI), and additive noise.

The work focuses on adaptive filtering, recursive estimation, convergence analysis, and communication system equalization.

---

## Overview

In wireless communication systems, multipath propagation causes transmitted symbols to overlap in time, creating inter-symbol interference (ISI).

Adaptive equalizers are used to estimate and compensate for the channel distortion.

This project compares:

- LMS adaptive equalization
- RLS adaptive equalization

using QPSK transmission through a noisy FIR multipath channel.

---

## Communication System Model

### QPSK Transmission

Random QPSK symbols are generated as:

```math
d[n] \in \{\pm1 \pm j\}
```

---

## Multipath Channel

The transmitted signal passes through a complex FIR channel:

```math
x[n] = h[n] * d[n]
```

where:

- `h[n]` is the channel impulse response
- `*` denotes convolution

The received signal becomes:

```math
u[n] = x[n] + v[n]
```

where:

- `v[n]` is additive noise

The multipath channel introduces:

- inter-symbol interference (ISI)
- constellation spreading
- symbol detection errors

---

## Adaptive Equalizer

An adaptive FIR equalizer estimates the transmitted symbols:

```math
y[n] = w^H[n]u[n]
```

The estimation error is:

```math
e[n] = d[n] - y[n]
```

The objective is to minimize the mean square error:

```math
J[n] = E|e[n]|^2
```

---

# LMS Equalization

The LMS equalizer performs stochastic gradient descent using instantaneous error estimates.

## LMS Weight Update

where:

- `μ` → step size
- `u[n]` → received input vector
- `e[n]` → estimation error

---

## LMS Characteristics

### Advantages

- Simple implementation
- Low computational complexity
- Linear complexity in filter length

### Limitations

- Slow convergence
- Sensitive to step-size selection
- Steady-state fluctuations

---

# RLS Equalization

The Recursive Least Squares (RLS) algorithm is an adaptive filtering technique that minimizes a weighted least-squares cost function using all past training samples.

Unlike LMS, which uses a simple gradient approximation, RLS recursively updates the filter coefficients using second-order correlation information, leading to significantly faster convergence.

---

## RLS Cost Function

RLS minimizes the exponentially weighted error cost:

```math
J[n] = \sum_{k=0}^{n} \lambda^{\,n-k} |e[k]|^2
```

where:

- `e[k]` is the estimation error
- `λ` is the forgetting factor with `0 < λ ≤ 1`

The forgetting factor controls how strongly past samples influence the current estimate:

- `λ = 1`  
  → all past samples are weighted equally

- `λ < 1`  
  → older samples are gradually forgotten, improving tracking in time-varying channels

---

## RLS Gain Vector

The RLS gain vector determines how strongly the filter coefficients are updated at each iteration.

```math
k[n] =
\frac{P[n-1]u[n]}
{\lambda + u^H[n]P[n-1]u[n]}
```

where:

- `P[n]` is the inverse correlation matrix
- `u[n]` is the received input vector
- `u^H[n]` denotes the Hermitian transpose

The gain vector adapts automatically based on the input signal statistics.

---

## RLS Weight Update

The equalizer coefficients are recursively updated using the estimation error:

```math
w[n] = w[n-1] + k[n]e^*[n]
```

where:

- `w[n]` is the adaptive equalizer coefficient vector
- `e^*[n]` is the complex conjugate of the error signal

---

## Inverse Correlation Matrix Update

The inverse correlation matrix is updated recursively as:

```math
P[n] =
\frac{1}{\lambda}
\left(
P[n-1]
-
k[n]u^H[n]P[n-1]
\right)
```

This recursive update avoids direct matrix inversion at every iteration, making RLS computationally efficient compared to solving the full least-squares problem repeatedly.

---

## Key Characteristics of RLS

### Advantages

- Very fast convergence
- Lower steady-state estimation error
- Excellent performance in highly correlated channels
- Strong ISI mitigation capability

### Limitations

- Higher computational complexity than LMS
- Requires matrix updates at every iteration
- More memory intensive
- Sensitive to forgetting-factor selection
---

# MATLAB Features

The implementation includes:

- QPSK modulation
- Complex FIR multipath channel simulation
- AWGN noise generation
- LMS adaptive equalization
- RLS adaptive equalization
- Forgetting-factor analysis
- Symbol Error Rate (SER) evaluation
- Constellation visualization
- Convergence analysis

---

# Experimental Setup

## Parameters

```matlab
SNR = 25 dB
Equalizer Length = 20
Channel Length = 5
```

Two RLS configurations were studied:

- `λ = 1.00`
- `λ = 0.98`

---

# Results

## LMS Performance

Observed behavior:

- gradual convergence
- low computational complexity
- visible steady-state fluctuations
- successful recovery of QPSK clusters

The LMS equalizer successfully reduces ISI and restores the constellation structure.

---

## RLS Performance

Observed behavior:

- significantly faster convergence
- lower error floor
- tighter constellation clusters
- lower SER under static channels

RLS converges much faster because it uses recursive correlation information instead of simple gradient updates.

---

# Forgetting Factor Analysis

## λ = 1.00

Behavior:

- all past samples weighted equally
- excellent steady-state accuracy
- strong performance in static channels

Results:

```text
LMS SER = 0.0077
RLS SER = 0.0012
```

RLS achieves lower SER due to faster and more accurate convergence.

---

## λ = 0.98

Behavior:

- older samples gradually forgotten
- improved tracking capability
- slightly more sensitive to noise

Results:

```text
LMS SER = 0.0017
RLS SER = 0.0022
```

With forgetting enabled, RLS becomes more adaptive to time-varying environments but slightly loses steady-state accuracy in static channels.

---

# Key Observations

## LMS

- slower convergence
- simple implementation
- constant step-size causes fluctuations
- lower complexity

## RLS

- rapid convergence
- lower steady-state error
- better SER in static channels
- computationally expensive
- sensitive to forgetting factor selection

---

# Communication Concepts Covered

This project covers several important DSP and communication topics:

- Adaptive filtering
- Channel equalization
- Recursive estimation
- Least-squares optimization
- FIR filters
- QPSK modulation
- Inter-symbol interference (ISI)
- AWGN channels
- Convergence analysis
- Symbol detection
- Estimation theory
- Forgetting-factor adaptation

---

# Main Conclusions

- RLS converges significantly faster than LMS
- RLS achieves lower SER in static channels
- LMS remains attractive due to simplicity and lower complexity
- Forgetting factors (`λ < 1`) improve tracking ability
- In static channels, forgetting can slightly degrade RLS steady-state performance
- Proper parameter tuning is essential for stable adaptive equalization

---

# Files

- `RLS.m` — MATLAB implemnetation
- `RLS_Report.pdf` — Full report with derivations, plots, convergence analysis, and SER comparison

---

# References

1. S. M. Kay, *Fundamentals of Statistical Signal Processing: Estimation Theory*, Prentice Hall, 1993.

2. K. P. Murphy, *Machine Learning: A Probabilistic Perspective*, MIT Press, 2012.
