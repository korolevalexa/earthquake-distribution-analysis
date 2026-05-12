# Earthquake Distribution Analysis

Statistical analysis of earthquake events in a seismic catalog.  
The project studies how the distribution of earthquakes changes over time using empirical distribution functions, correlation matrices, PCA, and a correlation function for magnitude and waiting time.

## Overview

Each earthquake event is described by three parameters:

- magnitude `M`;
- depth `D`, km;
- waiting time `ΔT`, seconds, measured as the time from the previous event.

The first event is excluded because its waiting time is undefined.  
The events are sorted by time before computing `ΔT`.

The analysis consists of three parts:

1. construction of the empirical distribution function  
   `F(M, D, ΔT)`;
2. correlation matrix analysis and PCA;
3. analysis of the correlation function  
   `K(M, ΔT) = f(M, ΔT) - g(M)h(ΔT)`.

## 1. Empirical distribution function

The empirical distribution function is constructed on a discrete grid:

- magnitude step: `0.5`;
- depth step: `5 km`;
- waiting time step: `5000 s`.

The distribution function is defined as

```text
F(m, d, t) = P(M ≤ m, D ≤ d, ΔT ≤ t).
