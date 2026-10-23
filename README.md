# Botnet Detection and Host Profiling on CTU-13 Dataset

This repository contains a Machine Learning pipeline for network traffic analysis and botnet detection using the **CTU-13 dataset**. The project shifts the detection strategy from flow-level / pair-based parsing to **host-level behavioral profiling**.

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1KyjS7XDArTZBbi5tZ6JyTI5vAuP7LmBa)

---

## 📌 Project Overview
* **Dataset:** CTU-13 Network Traffic (Binetflow files)
* **Goal:** Detect infected hosts (bots) based on aggregated network activity metrics.
* **Methodology:** Feature extraction per host, standardized feature scaling, and Logistic Regression with balanced class weights.
* **Validation Strategy:** **Leave-One-Family-Out (LOFO)** cross-validation to evaluate model generalization on completely unseen botnet families.

---

## 📊 Performance Metrics

The baseline model was evaluated under LOFO cross-validation:

| Metric | Score |
| :--- | :--- |
| **ROC AUC** | **0.677** |
| **Precision** | **0.304** |
| **Recall** | **0.167** |
| **F1-score** | **0.215** |

> **Key Takeaway:** Static heuristics and linear models struggle with generalization across unseen botnet families due to overlapping behavior between active legitimate users and malware.

---

## 📈 Visualizations & Results

### 1. Model Evaluation Metrics
Evaluation under LOFO cross-validation showing score distribution, Precision-Recall, and ROC curves:
![Metrics Plots](botnet_analysis_results/metrics_plots.png)

### 2. Feature Importance Analysis
Standardized Logistic Regression coefficients indicating primary indicators of infection:
![Feature Importance](botnet_analysis_results/features_importance_plot.png)

---

## 🛠️ Feature Engineering
Key host-level behavioral features extracted from flow logs:
* `log_ndst`: Logarithm of unique destination IP addresses contacted (strongest indicator of scanning/spam).
* `odd_port_frac`: Fraction of connections made via non-standard network ports.
* `no_reply_frac`: Fraction of unanswered outbound connection requests.
* `min_pair_cv` & `n_regular_pairs`: Measures of connection timing regularity (beaconing behavior).
* `log_out_in`: Log ratio of sent vs. received bytes.

---

## 🚀 Future Improvements
To improve performance against unseen malware families:
1. **Time Windows:** Calculate features over short temporal windows (5–15 minutes) instead of total session averages.
2. **Tree Ensembles:** Transition from linear models to **LightGBM / XGBoost** to capture non-linear feature interactions.
3. **Anomaly Detection:** Apply unsupervised methods (**Isolation Forest**, **Autoencoders**) trained exclusively on normal traffic.

---

## 📁 Repository Structure
