# RoboSense: Real-World Industrial Hazard Monitoring Dataset

## Overview

**RoboSense** is a real-world, open-access dataset collected from a hybrid sensing system that integrates **Autonomous Mobile Robots (AMRs)** and **fixed sensor suites** for continuous industrial hazard monitoring.

RoboSense provides synchronized, multi-modal environmental data recorded in a live industrial facility in Alexandria, Egypt, over a six-month deployment. The dataset captures both normal operating conditions and rare, controlled industrial hazard events, and is intended to support research in industrial IoT, robotic sensing, and machine learning based hazard detection.

---

## Key Characteristics

* **Deployment type**: Real industrial testbed (not laboratory)
* **Duration**: 180 consecutive days (December 2024 – June 2025)
* **Sampling rate**: 1 sample per minute
* **Platforms**:

  * 2 Autonomous Mobile Robots (AMR₁, AMR₂)
  * 2 fixed sensor suites (Suite₁, Suite₂)
* **Total size**: More than 1 million labeled records
* **Labeling**: Online runtime labeling (`Normal`, `Hazard_1`, `Hazard_2`, `Hazard_3`)
* **Communication**: MQTT over Wi-Fi to ThingsBoard IoT Cloud

---

## System Architecture

The RoboSense system uses a decentralized hybrid architecture composed of:

* Two mobile sensor suites mounted on omnidirectional AMRs
* Two stationary sensor suites installed at fixed grid coordinates

The testbed covers a **9 m × 26 m** industrial facility divided into a 1 m² grid. Mobile robots traverse the grid to provide spatial coverage, while fixed suites provide baseline measurements.

All sensor suites operate as independent IoT nodes based on the **ESP32 microcontroller**, transmitting synchronized telemetry in real time via MQTT.

---

## Sensor Modalities

Each sensor suite contains **12 synchronized sensors** measuring physical and chemical parameters:

* Ambient temperature
* Ambient humidity
* Pressure
* Altitude
* On-board temperature
* Dust concentration
* Gas sensors:

  * MQ2
  * MQ3
  * MQ5
  * MQ7
  * MQ8
  * MQ135

These sensors cover combustion gases and hazardous compounds including **CO, H₂, CH₄, VOCs**, and particulate matter.

Each suite includes an internal airflow fan to improve gas diffusion and measurement stability.

---

## Hazard Events

The dataset includes **15 real controlled hazard events** distributed across three hazard classes:

* **Hazard_1 (Fire incidents)**: 5 events
* **Hazard_2 (High-temperature fluctuations)**: 4 events
* **Hazard_3 (Gas leakage events)**: 6 events

Each event lasts between **30 and 120 minutes**, providing dense labeled samples with spatial and temporal variation.

---

## Dataset Structure

Raw telemetry is exported from ThingsBoard as per-sensor CSV files and processed through a unified pipeline:

1. Export 12 sensor channels from each of the 4 suites (48 CSV files total)
2. Merge files by timestamp into a unified dataset
3. Add `Suite_ID` to identify the source platform
4. Interpolate missing values due to network interruptions
5. Generate synchronized multi-modal time series ready for analysis

Each record includes:

* Timestamp
* Suite_ID (AMR₁, AMR₂, Suite₁, Suite₂)
* Sensor readings (12 modalities)
* Position (x, y) for mobile platforms
* Runtime label (`Normal`, `Hazard_1`, `Hazard_2`, `Hazard_3`)

---

## Machine Learning Validation

The dataset was validated using four supervised classifiers:

* Random Forest
* XGBoost
* Support Vector Machine (SVM)
* Multi-Layer Perceptron (MLP)

Using a 70/30 train–test split:

* Ensemble models (Random Forest, XGBoost) achieved **macro-average F1-scores up to 0.99**
* All models reached **1.00 overall accuracy**, due to class imbalance dominated by the Normal class

These results confirm the dataset’s suitability for industrial hazard detection under real deployment constraints.

---

## Applications

RoboSense can be used for:

* Industrial hazard detection and classification
* Multi-modal sensor fusion research
* Mobile and fixed sensing benchmarking
* Industrial IoT and Internet of Robotic Things (IoRT) systems
* Rare-event detection under class imbalance

---

## Access

The dataset is publicly available at:

[https://github.com/Amr-Khamis-84/RoboSense-Dataset](https://github.com/Amr-Khamis-84/RoboSense-Dataset)

---

## Citation

If you use this dataset, please cite the following paper:

```
@article{Khamis2025RoboFusion,
title = {Hybrid real-synthetic datasetframework for robotichazard detection in industrialenvironments},
author = {Khamis, Amr and Shaban, Heba A. and Fayed, Heba A. and Aly, Moustafa H.},
journal = {Scientific Reports},
year = {2026},
url = {https://rdcu.be/eYRgE}
},
```

---

## License

This dataset and associated materials are released for academic and research use under the license specified in the repository. Please cite the original publication when using the data in any derived work.

---

## Contact

For questions, clarifications, or collaboration inquiries:

**Amr Khamis**
Department of Electronics and Communications Engineering
Arab Academy for Science, Technology and Maritime Transport (AASTMT)
Email: [amr.khamis@aast.edu](mailto:amr.khamis@aast.edu)
