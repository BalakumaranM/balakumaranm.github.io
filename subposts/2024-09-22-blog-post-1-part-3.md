---
title: 'Damage Assessment of a Physical Beam Reinforced with Masses using Machine Learning - Part 3'
date: 2024-09-22
permalink: /posts/2024/09/blog-post-1/
tags:
  shm
---

#Dataset Contents and Structure

The dataset contains 280 samples, with 70 samples for each damage condition, providing balanced data for machine learning analysis. Each sample consists of a 6400-point frequency spectrum stored as numerical arrays in MATLAB (.mat) files within zip archives.

The files provided are:

| **File Name** | **Size** | **Description** |
| --- | --- | --- |
| Dataset Beam-signal_Damaged-5.92.zip | 5.0 MB | Frequency domain data for Damaged-5.92% condition |
| Dataset Beam-signal_Damaged-8.87.zip | 5.0 MB | Frequency domain data for Damaged-8.87% condition |
| Dataset Beam-signal_Healthy.zip | 5.0 MB | Frequency domain data for Healthy condition |
| Dateset Beam-signal_Damaged-2.96.zip | 5.0 MB | Frequency domain data for Damaged-2.96% condition |
| DI_FRAC_Exp-estimation.xlsx | 15.5 kB | Experimental damage index estimations |
| Mass position.xlsx | 119.5 kB | Positions of masses on the beam |
| Beam_Reinforced_Masses.png | 165.7 kB | Image of beam setup with masses |
| BeamMasses.png | 100.9 kB | Additional image of beam setup |

The zip files contain the primary vibration data, while the Excel files offer supporting information. "DI_FRAC_Exp-estimation.xlsx" contains damage indices calculated from frequency response functions to measure damage severity. "Mass position.xlsx" specifies how masses are arranged on the beam, helping understand the damage simulation process.

Two images—"Beam_Reinforced_Masses.png" and "BeamMasses.png"—show the experimental setup, displaying the beam and its attached masses to provide visual context.

### Data Format and Accessibility

Each frequency spectrum represents an inertance response in the frequency domain, expressed as magnitude and phase or real and imaginary components. The file sizes (5.0 MB per zip) align with the expected storage requirements for 70 samples per condition, with each sample containing 6400 data points stored as floating-point numbers.

The dataset is freely available on Zenodo, where users can download individual files or the complete archive. Its DOI (10.5281/zenodo.8081690) enables proper citation, and Zenodo's DataCite integration supports academic indexing.

In this dataset, the **Frequency Response Assurance Criterion (FRAC)** is used to compute a damage index (DI) by comparing the frequency response functions (FRFs) of a beam under healthy and damaged conditions. The experimental protocol induces damage by removing reinforcement masses (producing mass loss levels of 2.96%, 5.92%, and 8.87%), which in turn alters the beam’s vibrational characteristics.

The computed DI values are then organized into two clustered components, **DI-1** and **DI-2**. Although both indices originate from the FRAC method, clustering them into two groups helps capture different aspects or features of the damage effect on the structure.

**FRAC Principle:**The FRAC method involves correlating the entire FRF spectrum of a damaged beam with that of a reference (healthy) beam.

- A **high correlation (values close to 1)** indicates that the dynamic response of the beam is similar to the healthy condition, meaning little or no damage.
- A **low correlation (values approaching 0)** suggests significant deviation from the healthy state, reflecting damage severity.
- **Computation Process:**
    1. The FRF (both magnitude and phase) is measured for the beam under different health conditions.
    2. The FRAC is calculated (often over specific frequency bands most sensitive to damage) by comparing the measured response against a baseline healthy response.
    3. The resulting damage index is a normalized value between 0 and 1.

This process is detailed in Sousa et al. (2023), where the authors show that the DI decreases as damage (mass loss) increases, thereby quantifying the severity of the damage

[doi.org](https://doi.org/10.1007/s42417-023-01072-7)

**All Possible Analyses (With or Without Machine Learning) can be performed using this dataset ?**

As the dataset focuses on vibration-based damage detection, a variety of analyses can be performed. Below is a comprehensive list of possible analyses—covering traditional methods, machine learning, and deep learning.

1. **Damage Classification Using Machine Learning**
    - **Description:** Train machine learning models to classify the beam’s condition (e.g., healthy vs. damaged) based on the inertance response.
    - **Methods:** Support Vector Machines (SVM), Random Forests, Neural Networks.
    - **Why Important:** Directly addresses the goal of automated damage detection, critical for practical SHM applications.
2. **Feature Extraction and Selection**
    - **Description:** Identify and select key features from the inertance response (e.g., natural frequencies, peak amplitudes, damping ratios) that correlate with damage.
    - **Methods:** Peak detection, statistical moments (mean, variance), or frequency shifts.
    - **Why Important:** Provides the foundation for effective machine learning by highlighting damage-sensitive patterns.
3. **Anomaly Detection**
    - **Description:** Detect deviations from the healthy state without predefined damage levels.
    - **Methods:** One-class SVM, autoencoders, or statistical thresholding.
    - **Why Important:** Ideal for early damage detection in real-time monitoring scenarios.
4. **Modal Analysis**
    - **Description:** Extract the beam’s dynamic properties, such as natural frequencies and mode shapes, from the inertance peaks.
    - **Methods:** Curve fitting, frequency-domain decomposition.
    - **Why Important:** Changes in modal parameters (e.g., frequency shifts) are direct physical indicators of damage.
5. **Deep Learning for End-to-End Damage Detection**
    - **Description:** Use deep learning to automatically learn features and classify damage from raw or minimally processed inertance data.
    - **Methods:** Convolutional Neural Networks (CNNs), Transformers.
    - **Why Important:** Powerful for large datasets, though computational cost may be higher; balances automation and accuracy.
6. **Uncertainty Quantification**
    - **Description:** Assess variability in measurements or damage simulations to improve reliability.
    - **Methods:** Statistical analysis, Bayesian inference.
    - **Why Important:** Enhances confidence in damage detection by accounting for noise or environmental effects.
7. **Stochastic Modeling**
    - **Description:** Model the randomness of damage or external factors affecting the beam.
    - **Methods:** Probabilistic models, Monte Carlo simulations.
    - **Why Important:** Useful for predicting long-term behavior or reliability under uncertain conditions.
8. **Time-Series Analysis (If Time-Domain Data Is Available)**
    - **Description:** Analyze temporal vibration patterns if raw time-domain data exists alongside frequency-domain inertance.
    - **Methods:** ARIMA, LSTM networks.
    - **Why Important:** Limited here since the dataset is frequency-based, but valuable if applicable.
9. **Visualization and Exploratory Data Analysis (EDA)**
    - **Description:** Visualize inertance responses and explore data distributions or trends.
    - **Methods:** Magnitude plots, histograms, Principal Component Analysis (PCA).
    - **Why Important:** A preliminary step to gain insights, though less directly tied to damage detection.
10. **Transfer Learning**
    - **Description:** Apply knowledge from similar vibration datasets to improve performance on this task.
    - **Methods:** Fine-tune pre-trained models (e.g., CNNs from other SHM datasets).
    - **Why Important:** Helpful if your dataset is small, but may not be critical given sufficient data.
11. **Generative Models for Data Augmentation**
    - **Description:** Generate synthetic inertance responses to expand the dataset.
    - **Methods:** Generative Adversarial Networks (GANs), Variational Autoencoders (VAEs).
    - **Why Important:** Useful for deep learning if more data is needed, but less critical for initial analysis.
12. **Physics-Informed Machine Learning**
    - **Description:** Integrate structural dynamics equations into machine learning models.
    - **Methods:** Physics-informed neural networks.
    - **Why Important:** Advanced approach that ensures physical consistency, but may be unnecessary for this dataset.
13. **Baseline Comparison with Traditional Methods**
    - **Description:** Compare machine learning results to simple, non-ML techniques.
    - **Methods:** Visual inspection of FRFs, threshold-based peak shifts.
    - **Why Important:** Validates advanced methods but offers limited standalone value compared to ML.