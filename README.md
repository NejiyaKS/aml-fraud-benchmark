# Adversarial Machine Learning — Financial Fraud Detection Benchmark

An empirical audit of decision-time evasion and training-data poisoning against a credit card fraud detection model zoo. This benchmark evaluates the robustness of 4 classifiers and 2 clustering models across 20 attack–defense pairs on a simulated 8,000-transaction dataset (5% baseline fraud rate).

---

## 📊 Benchmark Overview

* **Models Evaluated:** Logistic Regression, Support Vector Machines (RBF Kernel), Random Forest, Multi-Layer Perceptron (MLP/Deep Net), K-Means, and DBSCAN.
* **Attack Vectors:** 
  * **Decision-Time Evasion:** Perturbing fraudulent transaction features at inference time ($x_{adv} = x + \delta$) to bypass detection while preserving financial plausibility.
  * **Training-Data Poisoning:** Injecting corrupted samples or clean-label backdoors into training pipelines to degrade performance or plant trigger backdoors.
* **Interactive Dashboard:** Includes `aml_fraud_dashboard.html` for visual exploration of attack vs. defense deltas and robustness matrices.

---

## 🔬 Cross-Classifier Robustness Matrix

| Classifier | Clean Baseline | FGSM Evasion | PGD Evasion | Poisoned (20% LF) | Sanitized (K-Means) | Sanitized (DBSCAN) |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| **Logistic Regression** | 100.0% | 99.95% | 100.0% | 98.20% | 98.25% | 5.00% |
| **SVM (RBF)** | 100.0% | 100.0% | 100.0% | 99.65% | 99.65% | 5.00% |
| **Random Forest** | 100.0% | 100.0% | 100.0% | 99.00% | 98.75% | 25.35% |
| **MLP (Deep Net)** | 99.90% | 99.95% | 99.90% | 98.80% | 98.70% | 44.35% |

---

## 🛡️ Attack & Defense Reference Matrix

| ID | Attack Method | Target Phase | Paired Defense | Accuracy $\Delta$ |
| :---: | :--- | :--- | :--- | :---: |
| **01** | Fast Gradient Sign Method (FGSM) | Inference | Adversarial Retraining | `+0.15 pp` |
| **02** | Projected Gradient Descent (PGD) | Inference | Feature Squeezing | `+0.05 pp` |
| **03** | Carlini & Wagner ($L_2$) | Inference | Defensive Distillation | `−2.35 pp` |
| **04** | JSMA Saliency Attack | Inference | Gradient Masking | `0.00 pp` |
| **05** | HopSkipJump (Decision-based) | Inference | Randomized Smoothing | `0.00 pp` |
| **06** | Tree Split Perturbation | Inference | Robust Tree Squeezing | `+0.05 pp` |
| **07** | SVM Margin Crossing | Inference | SVM Margin Regularization | `+0.05 pp` |
| **08** | Boundary Uniform Noise | Inference | Denoising Filter | `−0.30 pp` |
| **11** | Clean-Label Backdoor | Training | SPECTRE Clustering (sim.) | `−0.10 pp` |
| **12** | Label Flipping | Training | K-NN Label Consistency | `−0.15 pp` |
| **13** | Gradient-Matching Poisoning | Training | DP-SGD Bound (sim.) | `+0.20 pp` |
| **14** | Bulls-eye Polytope | Training | Loss-Based Outlier Sanitization | `−0.45 pp` |
| **15** | Latent Feature Collision | Training | STRIP Entropy Verification (sim.) | `0.00 pp` |
| **16** | K-Means Centroid Drag | Training | K-Means Distance Filtering | `+0.10 pp` |
| **17** | DBSCAN Density Bridge | Training | DBSCAN Noise Elimination | `−94.90 pp` |
| **18** | Logistic Gradient Poisoning | Training | Huber Loss Residual Filter | `+1.45 pp` |
| **19** | Latent Anomaly Injection | Training | Isolation Forest Scrubbing | `0.00 pp` |
| **20** | Availability / DoS Poisoning | Training | RONI Batch Exclusion (sim.) | `0.00 pp` |

---

## 📌 Key Empirical Findings

* **Random Forest is the Most Resilient:** Ensemble averaging across orthogonal decision thresholds prevents single-perturbation transferability across the tree ensemble.
* **MLP & SVM Vulnerability:** Continuous, high-dimensional decision boundaries are significantly more sensitive to gradient- and margin-based nudges.
* **Sanitization Failure Mode:** Uncalibrated sanitization (e.g., DBSCAN Noise Elimination collapsing accuracy to 5.0%) poses a fail-open risk by discarding legitimate transaction distributions.

---

## 📁 Repository Structure

```text
├──AML_CIA3_2548535.ipynb # Attack generation, training pipelines & evaluations
├──aml_fraud_dashboard.html   # Standalone interactive dashboard
├──Adversarial_ML_CIA3_Report  # Report 
└── README.md                  # Project overview and benchmark results
