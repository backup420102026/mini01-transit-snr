# Mini Research #1 — Butterworth Bandpass Filtering for Transit SNR

## Overview

This mini research project investigates whether a conventional Butterworth bandpass filter can improve the signal-to-noise ratio (SNR) of an exoplanet transit light curve while preserving the transit signal.

The experiment uses real Kepler photometric data for **Kepler-8**, specifically **Quarter 5, short-cadence Light Curve 0**.

> **Important:** This is a controlled mini-experiment on one light curve. The result should not be generalized to all stars or all transit observations.

---

## Research Question

**How does conventional Butterworth bandpass filtering affect the signal-to-noise ratio (SNR) of an exoplanet transit light curve?**

### Hypothesis

If the filter suppresses a meaningful portion of the noise while retaining most of the transit signal, the measured SNR should increase.

---

## Dataset

- Target: **Kepler-8**
- Mission: **Kepler**
- Quarter: **5**
- Cadence: **Short cadence**
- Selected product: **Light Curve 0**
- Data source: NASA/Kepler data accessed through `lightkurve`

The selected light curve contains **46,158 data points**.

---

## Method

The experiment follows this pipeline:

```text
Kepler light curve
        ↓
Data cleaning / finite-value check
        ↓
Normalization
        ↓
Transit identification
        ↓
Baseline + transit measurements
        ↓
Original SNR
        ↓
Butterworth bandpass filter
        ↓
Filtered signal
        ↓
Filtered SNR
        ↓
Noise + transit preservation checks
        ↓
Final validation
```

### Filter

A Butterworth bandpass filter was applied using SciPy.

- Filter type: Butterworth bandpass
- Order: 3
- Low cutoff: **0.1 cycles/day**
- High cutoff: **20 cycles/day**

The filtering stage uses a forward-backward filtering approach to avoid introducing a phase shift. SciPy documents `sosfiltfilt` as a forward-backward filter using cascaded second-order sections and recommends second-order sections for numerical robustness. See the references below.

---

## SNR Definition

For this mini-experiment:

\[
SNR = \frac{\text{Transit Signal Depth}}{\text{Out-of-Transit Noise}}
\]

The signal depth is measured from the normalized baseline to the transit level, while noise is estimated from the out-of-transit region.

This is a deliberately simple and transparent SNR definition suitable for this controlled experiment.

---

## Results

| Metric | Original | Filtered |
|---|---:|---:|
| SNR | 4.6822 | **5.4875** |
| Signal depth | 0.007976 | 0.007819 |
| Noise | 0.001703 | **0.001425** |
| Transit depth | 7975.70 ppm | 7818.83 ppm |
| Data points | 46,158 | 46,158 |

### Main findings

- **SNR increased by 17.20%.**
- **Noise decreased by approximately 16.35%.**
- The transit signal remained present after filtering.
- Transit depth changed from **7975.70 ppm** to **7818.83 ppm**, indicating that most of the measured transit depth was preserved.
- The number of data points remained unchanged.
- Final validation checks passed.

---

## Final Figure

Place the saved final figure in:

```text
results/final_butterworth_comparison.png
```

The figure compares the original light curve with the Butterworth-filtered signal and highlights the transit region.

---

## Validation

The experiment included explicit validation checks for:

- Same number of data points
- No NaNs after cleaning
- No NaNs after filtering
- No infinite values after filtering
- Lower filtered noise
- Higher filtered SNR
- Preservation of the transit signal

**Final validation: PASSED**

---

## Limitations

This experiment has several limitations:

1. Only one target was analyzed.
2. Only one selected light curve was used for the main comparison.
3. Only one Butterworth bandpass configuration was tested.
4. The SNR definition is simplified and is intended for this mini-study rather than as a universal transit-detection metric.
5. A higher SNR in this experiment does not automatically mean that the filter is optimal for every transit signal.

These limitations are intentionally documented so that the result is not overstated.

---

## Reproducibility

### Environment

Python 3.10+

### Install dependencies

```bash
pip install -r requirements.txt
```

### Main libraries

```text
lightkurve
numpy
scipy
matplotlib
pandas
```

The analysis was developed and executed in Google Colab.

---

## Repository Structure

```text
mini01-transit-snr/
│
├── README.md
├── requirements.txt
│
├── notebook/
│   └── transit_snr_analysis.ipynb
│
├── results/
│   ├── final_results.csv
│   └── final_butterworth_comparison.png
│
└── report/
    └── research_report.md
```

---

## References

- Lightkurve documentation: https://docs.lightkurve.org/
- SciPy `butter`: https://docs.scipy.org/doc/scipy/reference/generated/scipy.signal.butter.html
- SciPy `sosfiltfilt`: https://docs.scipy.org/doc/scipy/reference/generated/scipy.signal.sosfiltfilt.html
- NASA Exoplanet Archive: https://exoplanetarchive.ipac.caltech.edu/

---

## Conclusion

For the tested Kepler-8 light curve, the selected Butterworth bandpass filter reduced the measured noise and increased the experiment's SNR from **4.68 to 5.49**, corresponding to a **17.20% improvement**, while preserving the transit signal.

The result supports the hypothesis **for this specific dataset and filter configuration**, but broader conclusions require testing multiple targets and filter settings.
