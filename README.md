# WaterShield: Predictive Early-Warning for PFAS Contamination 💧

An adaptive machine learning architecture built in Altair RapidMiner to predict future PFOA/PFOS contamination risks in public water systems using historical EPA UCMR5 chemistry data.

## 🎯 Project Overview
Public health responses to waterborne contaminants are frequently reactive. WaterShield shifts this paradigm by using historical chemical signatures to flag public water systems at high risk for future exceedances of the EPA's Minimum Reporting Level (0.004 µg/L). It is designed as a primary screening layer to prioritize confirmatory testing.

## 🔬 Methodology & Temporal Firewall
To ensure scientific rigor and prevent data leakage, this pipeline features:
* **MRL/2 Imputation:** Left-censored data (sub-reporting limit values) were mathematically imputed to maintain continuous chemical baselines without assuming absolute zero.
* **Temporal Split:** Historical data (≤ 2023) was strictly isolated to generate predictive features (historical max/avg), while future data (≥ 2024) was used exclusively for target labeling. Future values were locked out of the training features.
* **Adversarial Auditing:** Monitoring-volume proxies were explicitly removed to force the Random Forest algorithm to rely purely on chemical evidence.

## 📊 Results & Adaptive Thresholding
The baseline Random Forest model achieved a highly robust **ROC-AUC of 0.856**. Because missed public health risks (false negatives) carry a heavier cost than false alarms, the system utilizes custom probability thresholds:

* **High-Sensitivity Mode (Threshold ~0.69):** Default configuration prioritizing hazard detection. Captures **89.68%** of observed future high-risk cases.
* **Resource-Constrained Mode (Threshold 0.65):** Alternative configuration maximizing investigation efficiency, yielding a **78.57%** recall with **64.92%** precision.

## 🚀 How to Run the Project
1. Download Altair AI Studio (RapidMiner).
2. Import the `WaterShield_Pipeline.rmp` file via `File > Import Process`.
3. Connect the pipeline to the UCMR5 dataset (available via the EPA).
4. Run the process to view the final ROC curve, Confusion Matrix, and feature weighting.
