# Earthquake Distribution Analysis

Statistical analysis of earthquake events in a seismic catalog.

The project studies how the distribution of earthquakes changes over time using empirical distribution functions, correlation matrices, PCA, and a correlation function for magnitude and waiting time.

## Repository Structure

    earthquake-distribution-analysis/
    │
    ├── README.md
    ├── requirements.txt
    │
    ├── data/
    │   ├── magn
    │   ├── deep
    │   └── time
    │
    ├── notebooks/
    │   └── earthquake_analysis.ipynb
    │
    ├── figures/
    │   ├── task1_distribution.png
    │   ├── task2_correlation_pca.png
    │   ├── task3_correlation_function.png
    │   └── ...
    │
    └── earthquake_analysis_presentation.pptx

## Overview

Each earthquake event is described by three parameters:

- magnitude `M`;
- depth `D`, km;
- waiting time `ΔT`, seconds, measured as the time from the previous event.

The first event is excluded because its waiting time is undefined. Events are sorted by time before computing `ΔT`.

The analysis consists of three parts:

1. Construction of the empirical distribution function `F(M, D, ΔT)`.
2. Correlation matrix analysis and PCA.
3. Analysis of the correlation function `K(M, ΔT) = f(M, ΔT) - g(M)h(ΔT)`.

## 1. Empirical Distribution Function

The empirical distribution function is constructed on a discrete grid:

- magnitude step: `0.5`;
- depth step: `5 km`;
- waiting time step: `5000 s`.

The distribution function is defined as:

    F(m, d, t) = P(M ≤ m, D ≤ d, ΔT ≤ t)

The catalog is split into two equal parts by the number of events. For each half, the distribution functions `F1` and `F2` are constructed on the same grid.

The difference between the two halves is measured using the C-norm:

    ||F1 - F2||_C = max |F1(i,j,k) - F2(i,j,k)|

The maximum difference is approximately:

    ||F1 - F2||_C ≈ 0.343

at:

    M ≤ 8.5, D ≤ 30 km, ΔT ≤ 40000 s

This means that the accumulated probabilities differ by about 34 percentage points in this region. The main contribution to the difference comes from depth and waiting time rather than magnitude.

## 2. Correlation Matrix and PCA

For the full dataset and for both halves, correlation matrices are computed for:

    M, D, ΔT

The off-diagonal correlation coefficients are small:

    |corr| ≤ 0.12

This suggests that there is no strong pairwise linear dependence between magnitude, depth, and waiting time.

PCA is then applied to the standardized variables. For the full dataset, the eigenvalues are approximately:

    λ1 ≈ 1.196
    λ2 ≈ 0.931
    λ3 ≈ 0.872

Since all eigenvalues are close to `1`, there is no strongly dominant principal direction. This supports the conclusion that the main change in the catalog is not explained by a simple linear dependence between the three variables.

## 3. Correlation Function K(M, ΔT)

The correlation function is defined as:

    K(M, ΔT) = f(M, ΔT) - g(M)h(ΔT)

where:

- `f(M, ΔT)` is the joint normalized histogram;
- `g(M)` is the marginal histogram of magnitude;
- `h(ΔT)` is the marginal histogram of waiting time.

If magnitude and waiting time are independent, then:

    f(M, ΔT) = g(M)h(ΔT)

and therefore:

    K(M, ΔT) = 0

The function is computed:

1. for the full dataset;
2. for the most populated depth layer.

The most populated depth layer is:

    10–15 km

with approximately `34.6%` of all events.

The maximum absolute deviation is:

    max |K| ≈ 0.00879

inside the most populated depth layer, and:

    max |K| ≈ 0.01161

for the full dataset.

The slightly larger value for the full dataset may indicate that part of the apparent dependence between magnitude and waiting time comes from mixing events from different depth layers.

## Main Findings

- The two halves of the catalog have noticeably different empirical distributions.
- The strongest change is associated with shallow events and shorter waiting times.
- The median depth decreases from about `33.0 km` to `13.1 km`.
- The median waiting time decreases from about `21077 s` to `9540 s`.
- The share of events with `D ≤ 30 km` increases from about `34.5%` to `63.5%`.
- Pairwise linear correlations remain weak.
- PCA does not reveal a dominant linear direction.
- The correlation function `K(M, ΔT)` shows local deviations from independence, especially for small waiting times.

## Interpretation

The catalog appears to be non-stationary: the second half contains more shallow and more frequently occurring events.

At the same time, this change is not well described by simple linear correlations. The difference is better captured by comparing empirical distribution functions and by analyzing local deviations in joint histograms.

## Requirements

Main Python libraries:

    numpy
    pandas
    matplotlib
    scipy
    scikit-learn
    jupyter

Install dependencies:

    pip install -r requirements.txt

## Author

Alexander Korolev  
MIPT, FPMI
