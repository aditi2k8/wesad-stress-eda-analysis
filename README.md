# Multimodal Affective State & Stress Analysis (WESAD Dataset)
**Course:** Data Analytics | **Subject:** WESAD Subject S2 (Empatica E4 Wristband)

An end-to-end data analytics project featuring **signal cleaning**, **artifact interpolation**, **low-pass/band-pass filtering**, **sensor detrending**, **window-level feature extraction**, and **hypothesis testing (Welch's t-test, One-Way ANOVA, Tukey HSD)**.

---

## 📌 Project Overview
The objective of this project is to determine whether wrist-worn wearable physiological signals (**Heart Rate, Electrodermal Activity, Blood Volume Pulse amplitude, and Skin Temperature**) exhibit statistically significant differences across induced affective states:
1. **Baseline** (resting state)
2. **Stress** (Trier Social Stress Test - TSST: public speaking & mental arithmetic)
3. **Amusement** (humorous video clips)
4. **Meditation** (guided relaxation)

---

## 🔬 Key Methodological Steps
1. **Data Ingestion & Alignment:** Loaded multi-channel asynchronous time-series streams (HR at 1 Hz, EDA at 4 Hz, TEMP at 4 Hz, BVP at 64 Hz).
2. **Data Cleaning & Filtering:**
   - Interpolated zero-readings and hardware dropouts (`EDA < 0.01 µS`, `TEMP < 25°C` or `> 42°C`, `HR < 40` or `> 200` bpm).
   - Applied a 5-point median filter to remove hardware motion spikes from HR and TEMP.
   - Filtered EDA with a 2nd-order Butterworth low-pass filter (1 Hz cutoff).
   - Applied a 3rd-order zero-phase Butterworth bandpass filter (0.5–8.0 Hz) to BVP.
3. **Signal Detrending:** Linear least-squares detrending applied to EDA and Skin Temperature to remove long-term tonic drift while preserving physical units.
4. **Feature Windowing:** Segmented signals into **149 non-overlapping 30-second summary windows** to prevent time-series autocorrelation from skewing statistical tests.

---

## 📊 Visualizations

### 1. Continuous Multi-Channel Signal Streams
Shows the continuous bio-signals over the 130-minute protocol with affective condition segments highlighted:
![Continuous Signals](outputs_analysis/fig1_signals.png)

### 2. Feature Distribution Across Conditions
Boxplot comparison of Heart Rate, EDA, BVP Amplitude, and Skin Temperature across the 4 affective states:
![Feature Boxplots](outputs_analysis/fig2_boxplots.png)

---

## 📈 Key Statistical Findings

### 1. One-Way ANOVA (Across All 4 States)
A One-Way ANOVA across `Baseline`, `Stress`, `Amusement`, and `Meditation` rejected the null hypothesis for every physiological modality ($p < 0.001$):
* **Heart Rate:** $F = 37.7012, p = 2.45 \times 10^{-17}$
* **Electrodermal Activity (EDA):** $F = 41.7540, p = 1.62 \times 10^{-18}$
* **BVP Pulse Amplitude:** $F = 9.6051, p = 7.95 \times 10^{-6}$
* **Skin Temperature:** $F = 48.1265, p = 1.05 \times 10^{-20}$

### 2. Welch's Two-Sample t-Test (Stress vs. Non-Stress)
* **Skin Temperature:** $t = 13.3743, p < 10^{-15}$, Cohen's $d = +1.763$ (Very Large Effect) — peripheral temperature remains tightly elevated during TSST ($35.40^\circ\text{C}$).
* **EDA:** $t = -20.3934, p < 10^{-15}$, Cohen's $d = -2.665$ (Huge Effect) — strong autonomic sympathetic deflection.
* **Heart Rate:** $t = -4.6807, p = 7.15 \times 10^{-6}$, Cohen's $d = -0.625$ (Medium Effect).
* **BVP Amplitude:** $t = -1.4542, p = 0.1544$ — binary separation was not statistically significant, but ANOVA showed significant differentiation between Amusement and Meditation.
