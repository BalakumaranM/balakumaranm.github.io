---
title: 'Transformer based Structural Health Monitoring using Frequency Response Function (FRF) - Part 3'
date: 2024-09-22
permalink: /posts/2024/09/blog-post-3/
tags:
  shm
---

# Dataset Contents and Structure

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

## Limitation of Having Only Frequency Domain Signal in Terms of Real-Time Structural Analysis Using AI

While our previous posts have focused on analyzing the frequency-domain representation of the vibration data, relying solely on this information comes with some limitations for real-time structural health monitoring. In this section, we'll discuss why converting time-domain signals into richer time-frequency representations is beneficial, and how techniques like the Short-Time Fourier Transform (STFT) can bridge the gap.

### Why Pure Frequency Domain Data Falls Short

When we analyze a signal in the frequency domain, we typically obtain a static picture that summarizes the overall frequency content of the vibration. Although this provides useful information about the beam’s dynamic characteristics, it does not capture how these frequencies evolve over time. This temporal evolution is crucial for real-time monitoring because:

- **Transient Events:** Sudden changes or transient phenomena—such as abrupt impacts or evolving damage—may be smoothed out in a pure frequency-domain view.
- **Dynamic Behavior:** Structural responses can vary with time due to operational or environmental factors. A static frequency spectrum may not capture these variations, limiting the effectiveness of damage detection in real time.

### Converting to a Richer Time-Frequency Representation

To overcome these limitations, we can use the **Short-Time Fourier Transform (STFT)**. The STFT divides the signal into short segments (or windows) and computes the Fourier Transform on each, resulting in a two-dimensional representation that shows how the signal’s frequency content changes over time.

The mathematical formulation of the STFT is:

$$
STFT\{x(t)\}(m, \omega) = \int_{-\infty}^{\infty} x(t) \, w(t-mT) \, e^{-j\omega t} \, dt
$$

where:  
- \( x(t) \) is the original time-domain signal,  
- \( w(t-mT) \) is the window function centered at time \( mT \), and  
- \( \omega \) represents the angular frequency.

The output of the STFT is typically visualized as a **spectrogram**, a 2D image where:
- The **x-axis** represents time,
- The **y-axis** represents frequency, and
- The **color intensity** (or a third dimension) represents the magnitude (amplitude) of the frequencies.

### Benefits for AI and Real-Time Monitoring

This transformation from a 1D frequency-domain signal to a 2D spectrogram (or even a 3D representation, if you consider the magnitude as depth) brings several advantages for AI-based structural health monitoring:

1. **Dense and Informative Representation:**  
   The spectrogram provides a rich, image-like representation of the signal. Each “pixel” contains information about the amplitude of a specific frequency at a specific time, making it easier to detect subtle changes or patterns.

2. **Leverage Transformer-Based Models:**  
   Transformer models, which have recently shown great promise in processing sequential and image-like data, excel at capturing both local and global dependencies. When fed with spectrograms, these models can learn complex patterns related to damage progression and transient events, leading to improved detection accuracy.

3. **Real-Time Inferencing on Edge Devices:**  
   With the dense time-frequency data, AI models can be trained to recognize damage signatures more effectively. Once trained, these models can be deployed on edge devices for real-time monitoring. The ability to process spectrograms quickly means that even devices with limited computing power can provide timely warnings about structural anomalies.

### Visualizing the Transformation

To help illustrate this transformation, consider the following diagram:

<div style="text-align: center;">
  <img src="/images/blog_related/fourier_transform.png" alt="Fourier Transform Diagram" style="width:60%;">
  <p><strong>Fourier Transform Diagram: Converting Time-Domain Data to Frequency-Domain Representation</strong></p>
  <p>
    <a href="https://en.wikipedia.org/wiki/Fourier_transform" target="_blank">View Original Content</a>
  </p>
</div>

Now, imagine a similar diagram that shows the STFT process, where the signal is segmented into overlapping windows, each transformed into a spectrum. The resulting spectrogram is akin to a heatmap, where the color represents the amplitude at each time-frequency coordinate.

### Putting It All Together

In summary, while the frequency-domain data we discussed in Parts 1 and 2 provides valuable insights, it lacks the temporal resolution necessary for real-time analysis. By applying the STFT, we obtain a time-frequency representation that not only captures the evolving dynamics of the beam’s vibration but also produces data that are ideal for training transformer-based AI models. This richer data format enhances our ability to perform real-time structural health monitoring, especially when deployed on edge devices for immediate inference.

For more details on the fundamentals of our dataset and time-domain analysis, please refer back to [Part 1](https://balakumaranm.github.io/posts/2024/09/blog-post-1/) and [Part 2](https://balakumaranm.github.io/posts/2024/09/blog-post-2/).

---

[Go to Part-3](/posts/2024/09/blog-post-4/)

---
