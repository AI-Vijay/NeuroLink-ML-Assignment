# Neuro-Link Intern Assignment — Signal Processing / ML Engineer
**Candidate:** Vijay Nagargoje
**Role:** Signal Processing / ML Engineer

## 1. Project Overview
This repository contains a complete machine learning pipeline designed to detect physiological stress from Heart Rate Variability (HRV) metrics. The system processes raw, continuous heartbeat data, extracts time-domain features using a sliding temporal window, and classifies the user's state (Relaxed vs. Stressed) using a Random Forest Classifier.

## 2. Technical Architecture & Data Strategy
To optimize for the strict execution time limit while fulfilling all engineering requirements, the pipeline was structured as follows:
* **Signal Processing (Task 1):** A custom peak detection algorithm utilizing a dynamic rolling average threshold and refractory period logic is demonstrated in the notebook using raw ECG data.
* **Feature Engineering & Data Alignment:** For the core ML task, pre-extracted RR intervals from the SWELL-KW dataset were utilized. A temporal windowing function was engineered to perfectly align the high-frequency, asynchronous sensor data (heartbeats) with the low-frequency batch labels (per-minute states).
* **Embedded Simulation (Bonus Task):** The notebook concludes with a real-time hardware simulation utilizing a memory-efficient `collections.deque` to maintain a continuously sliding 60-second window, mimicking the RAM constraints of an embedded microcontroller.

## 3. Model Limitations & Scaling Strategy
To meet the time constraints, this pipeline was optimized for execution speed by processing only a single subject (p1). As a result, the test set size is extremely small (N=17 windows), leading to high variance in accuracy (58.8%). If scaling this pipeline for production, I would implement the following architecture upgrades:
1. **Baseline Normalization:** HRV is highly subject-dependent. I would calculate the resting baseline features for the user and express the active window features as a percentage deviation from that baseline to improve classifier separability.
2. **Expanded Feature Space:** I would add frequency-domain extraction (specifically the LF/HF ratio) to better capture the interplay between the sympathetic and parasympathetic branches.
3. **Temporal Dynamics:** Instead of evaluating windows in isolation, I would implement an LSTM or an XGBoost model with lag features to capture the physiological transition over time.

## 4. Visualizing Parasympathetic Withdrawal: The Physiology of RMSSD (Medical Write-up)
**What RMSSD Measures:**
RMSSD (Root Mean Square of Successive Differences) is a time-domain metric that captures short-term, beat-to-beat variance in heart rate. Physiologically, this high-frequency variation is the direct signature of the Autonomic Nervous System (ANS), specifically the parasympathetic branch. The vagus nerve acts as a dynamic "brake" on the heart, making rapid micro-adjustments to the sinoatrial node in response to respiration and environmental stimuli. A high RMSSD indicates strong vagal tone—meaning the system is highly responsive, relaxed, and operating in a healthy "rest and digest" state.

**Why RMSSD Drops Under Acute Stress:**
When a subject experiences acute physiological or psychological stress, the ANS shifts control to the sympathetic nervous system (the "fight or flight" response). This sympathetic override effectively suppresses vagal tone, lifting the parasympathetic brake. 

Without these continuous vagal micro-adjustments, the heart begins to beat in a rigid, highly metronomic pattern to optimize cardiac output for immediate survival. Because the time intervals between consecutive beats become uniform, the successive differences shrink toward zero. Consequently, a sharp drop in RMSSD serves as a highly reliable, non-invasive biomarker of parasympathetic withdrawal and acute stress onset.

---

## Final Reflections

* **What was the hardest part of this problem?**
The hardest part of this assignment was bridging the gap between medical physiology and data engineering, specifically ensuring that I accurately translated complex biological concepts like vagal tone into programmable, time-series machine learning features

* **What would you do differently with more time?**
With more time, I would consult with a medical domain expert to engineer frequency-domain features, evaluate sequential algorithms like LSTMs to capture temporal dynamics, and deploy my live sliding-window simulation to a Streamlit dashboard for real-time visualization.

* **What question does this assignment make you want to ask us?**
Given the complexity of continuous biosensing, what are the exact compute and memory constraints on your wearable's edge microcontroller, and what additional hardware metrics are you planning to gather to support this ML pipeline?
