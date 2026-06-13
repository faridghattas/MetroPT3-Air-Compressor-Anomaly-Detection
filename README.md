# ⚙️ MetroPT Train Compressor - Predictive Maintenance Using Unsupervised Learning

## 📌 Project Overview
This project develops an advanced predictive maintenance (PdM) and anomaly detection pipeline for a critical rolling stock subsystem: the **Metro Train Air Compressor (PT3 Dataset)**. The goal is to move away from traditional static thresholds and utilize Machine Learning to capture multivariate anomalies, providing early warnings for critical failure events like Air Leaks before they cause severe transit delays.

---

## 📊 Dataset & Temporal Splitting
To simulate a real-world industrial deployment scenario and prevent data leakage:
* **Training Data (Normal Baseline):** February & March (The compressor operated under completely clean, failure-free conditions).
* **Testing Data (Evaluation Phase):** April (Contains the first high-stress Air Leak catastrophic failure event documented on **April 18th**).

---

## 🧠 Engineering Insights: How the Model Works Mechanically
Traditional maintenance triggers an alert only when a single sensor exceeds a specific safety limit. This project utilizes **One-Class SVM (RBF Kernel)** to evaluate the multivariate relationships across the entire compressor system simultaneously. 

Mechanically, it learns the normal **"Baseline Cluster"** where:
* High **Motor Current** must co-occur with a proportional rise in **TP2 (Secondary Pressure)**.
* Active **COMP** signals align with structured duty cycles.

When an **Air Leak** develops, this physics-based correlation breaks down—pressure drops sharply while the motor current spikes or continues operating indefinitely to compensate for the loss. In the multi-dimensional feature space, these broken correlations force the data points to drift outside the learned normal boundary, triggering an immediate predictive anomaly alert.

---

## 📈 Model Performance & Evolution

Initially, **Isolation Forest** was deployed as a baseline. While computationally fast, it struggled to capture the gradual, micro-leak degradation phase, yielding an unacceptable **Recall of only 2%** on the failure event.

By pivoting to **One-Class SVM** and applying smart **10% stratified sampling** to overcome the high computational complexity ($O(N^2)$) on massive high-frequency data, the system achieved a massive performance leap:

| Metric | Baseline (Isolation Forest) | Final Model (One-Class SVM) |
| :--- | :---: | :---: |
| **Failure Recall (Detection Rate)** | 2% | **99%** 🚀 |
| **Precision** | 8% | **37%** 📈 |
| **F1-Score** | 0.04 | **0.54** 🔥 |

---

## 🛠️ Tech Stack Used
* **Data Ingestion & Handling:** Pandas, NumPy
* **Data Visualization:** Matplotlib, Seaborn
* **Machine Learning Framework:** Scikit-Learn (OneClassSVM, IsolationForest, StandardScaler)
