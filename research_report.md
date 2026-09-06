# Mini Research #1
## Effect of Butterworth Bandpass Filtering on Transit Signal-to-Noise Ratio

### Abstract

This mini-study evaluates the effect of a conventional Butterworth bandpass filter on the signal-to-noise ratio (SNR) of an exoplanet transit light curve. Real Kepler photometric data for Kepler-8 were accessed through the Lightkurve Python package. A short-cadence light curve from Quarter 5 was selected and cleaned before analysis. The transit signal and out-of-transit noise were measured before and after filtering. A third-order Butterworth bandpass filter with cutoffs of 0.1 and 20 cycles/day was applied using a forward-backward filtering approach. The measured SNR increased from 4.68 to 5.49, corresponding to a 17.20% improvement. Measured noise decreased by approximately 16.35%, while the transit depth changed from 7975.70 ppm to 7818.83 ppm. Validation checks confirmed that the filtered data retained the same number of samples, contained no NaNs or infinite values, showed reduced noise, and preserved the transit signal. These findings demonstrate a positive SNR effect for this specific dataset and filter configuration, while the limited experimental scope prevents generalization to all transit observations.

---

## 1. Introduction

Exoplanet transit photometry measures small changes in the observed brightness of a star. When a planet passes in front of its host star, the measured flux decreases, producing a transit signature in the light curve.

Because transit signals can be relatively small compared with observational noise, signal processing can be useful for improving the visibility of the underlying structure. One simple approach is to apply a conventional digital filter that suppresses unwanted frequency components.

This project focuses on a deliberately narrow question: whether a Butterworth bandpass filter can improve a simple, transparent measurement of transit SNR while retaining the transit signal.

---

## 2. Research Question

**How does conventional Butterworth bandpass filtering affect the signal-to-noise ratio (SNR) of an exoplanet transit light curve?**

### Hypothesis

The expected outcome is that filtering will reduce the measured out-of-transit noise more than it reduces the measured transit depth, producing a higher SNR.

---

## 3. Dataset

The experiment uses Kepler-8 photometric data accessed through the Lightkurve package.

Selected dataset:

- Target: Kepler-8
- Mission: Kepler
- Quarter: 5
- Cadence: short cadence
- Selected light curve: Light Curve 0
- Number of data points: 46,158

The selected light curve was normalized before the main signal and noise measurements.

---

## 4. Methodology

The analysis consisted of the following stages:

1. Retrieve the Kepler-8 Quarter 5 light-curve products.
2. Select Light Curve 0.
3. Clean the data and check for non-finite values.
4. Normalize the flux.
5. Identify a transit region for the focused comparison.
6. Estimate the baseline flux and transit flux.
7. Calculate transit depth.
8. Estimate out-of-transit noise.
9. Calculate the original SNR.
10. Design a Butterworth bandpass filter.
11. Apply the filter using a forward-backward approach.
12. Recalculate signal depth and noise.
13. Calculate filtered SNR.
14. Compare the original and filtered measurements.
15. Run data-integrity and physical-signal-preservation checks.

---

## 5. SNR Definition

The experiment uses:

\[
SNR = \frac{D}{\sigma}
\]

where:

- \(D\) is the measured transit signal depth.
- \(\sigma\) is the estimated out-of-transit noise.

This definition is intentionally simple so that the effect of filtering can be isolated and understood clearly.

---

## 6. Butterworth Filter

A third-order Butterworth bandpass filter was used.

Parameters:

- Order: 3
- Low cutoff: 0.1 cycles/day
- High cutoff: 20 cycles/day

The filter was applied in a forward-backward manner. SciPy provides `sosfiltfilt` for forward-backward filtering using cascaded second-order sections. This approach avoids the phase shift associated with one-direction filtering and is preferable to direct polynomial filtering for numerical stability in many cases.

---

## 7. Experimental Results

### 7.1 SNR

Original:

\[
SNR_{original}=4.6822
\]

Filtered:

\[
SNR_{filtered}=5.4875
\]

Relative improvement:

\[
\frac{5.4875-4.6822}{4.6822}\times100
=17.20\%
\]

Therefore, the measured SNR increased by **17.20%**.

---

### 7.2 Noise

Original noise:

\[
\sigma_{original}=0.001703
\]

Filtered noise:

\[
\sigma_{filtered}=0.001425
\]

The measured noise decreased by approximately **16.35%**.

---

### 7.3 Transit Signal

Original signal depth:

\[
D_{original}=0.007976
\]

Filtered signal depth:

\[
D_{filtered}=0.007819
\]

In ppm:

- Original: **7975.70 ppm**
- Filtered: **7818.83 ppm**

The transit depth was therefore slightly reduced, but the transit remained clearly visible after filtering.

---

## 8. Interpretation

The SNR improvement was primarily driven by noise reduction.

The measured signal depth changed only modestly:

\[
0.007976 \rightarrow 0.007819
\]

while the noise decreased more substantially:

\[
0.001703 \rightarrow 0.001425
\]

Since SNR is proportional to signal depth and inversely proportional to noise, the stronger reduction in noise produced the observed increase in SNR.

This demonstrates an important signal-processing trade-off: filtering can improve detectability by suppressing noise, but aggressive filtering can also alter the signal itself.

---

## 9. Validation

The final validation produced:

- Same number of data points: **PASS**
- No NaNs after cleaning: **PASS**
- No NaNs after filtering: **PASS**
- No infinite values after filtering: **PASS**
- Filtered noise lower than original: **PASS**
- Filtered SNR higher than original: **PASS**
- Transit signal preserved: **PASS**

### Final Validation Status

**PASSED — The experiment is computationally consistent.**

---

## 10. Limitations

The study is intentionally small and should be interpreted accordingly.

### Dataset limitation

Only one target and one selected light curve were used.

### Filter limitation

Only one Butterworth configuration was evaluated. Different cutoff frequencies and filter orders could produce different results.

### Metric limitation

The SNR definition is a simplified experimental metric. Professional transit-search pipelines may use more sophisticated noise models, detrending procedures, transit templates, and detection statistics.

### Generalization limitation

A 17.20% improvement on this dataset does not imply that the same improvement will occur for other stars or transit signals.

---

## 11. Conclusion

The experiment provides evidence that the selected Butterworth bandpass filter improved the measured SNR of the Kepler-8 Quarter 5 Light Curve 0 transit signal.

The key result was:

\[
4.68 \rightarrow 5.49
\]

corresponding to a:

**17.20% SNR improvement**

with approximately:

**16.35% noise reduction**

while retaining the transit signal.

Therefore, the hypothesis was supported **for the tested dataset and filter configuration**.

A stronger follow-up study would evaluate multiple Kepler targets, multiple transit events, and multiple filter configurations.

---

## 12. Reproducibility

The experiment was developed in Google Colab using Python and the following libraries:

- Lightkurve
- NumPy
- SciPy
- Matplotlib
- Pandas

The repository contains the analysis notebook, numerical results, final figure, and this report.

---

## 13. References

1. Lightkurve documentation — Python tools for astronomical time-series analysis.
2. SciPy documentation — `scipy.signal.butter`.
3. SciPy documentation — `scipy.signal.sosfiltfilt`.
4. NASA Exoplanet Archive — astronomical catalog and exoplanet data resources.
