---
title: "Transformer based Structural Health Monitoring using Frequency Response Function (FRF) - Part 4"
date: 2024-09-22
permalink: /posts/2024/09/blog-post-4/
tags:
  - shm
---

# Transformer Model Training and Optimization

In [Part 3](https://balakumaranm.github.io/posts/2024/09/blog-post-3/), we explored the structure and contents of our beam-signal dataset, highlighting how frequency response data is collected and stored. Now, we move on to the core of our research: **training a transformer-based model** for damage classification. This post walks through the **model architecture**, **preprocessing steps**, **hyperparameter tuning**, and the **final results**—including lessons learned along the way.

---

## 1. Initial Approach

### Model Architecture

I began with a simple **transformer encoder**:
- **Embedding dimension (d_model):** 64  
- **Number of heads:** 4  
- **Encoder layers:** 2  
- **Dropout:** 0.1  
- **Batch size:** 8  
- **Learning rate:** 1e-4  
- **Max sequence length (max_seq_len):** 1100  

Since each sample originally had **6400** frequency points (ranging from 0 to 2000 Hz with a 0.3125 Hz interval), I **downsampled** them to 1100 points for computational efficiency. However, the initial model yielded a **maximum accuracy of around 25%**, which was essentially random guessing across our four damage classes.

### Early Insights

1. **Small Dataset Challenges**  
   The beam-signal dataset contains only **280 samples** (70 per class). Transformers typically thrive on large datasets, so it’s easy for the attention mechanism to overfit or fail to learn meaningful patterns when data are limited.

2. **Phase Data and Discontinuities**  
   The raw phase data had steep changes and discontinuities. I tried **phase unwrapping** to smooth out these jumps, but it didn’t significantly boost accuracy. Other standard preprocessing steps (e.g., normalization, smoothing) also had limited effect.

---

## 2. Focus on Key Frequency Bands

A crucial breakthrough came when I analyzed the **frequency-magnitude** graphs across different damage conditions. The **first three vibrational modes** of the beam—**10–40 Hz**, **120–160 Hz**, and **350–450 Hz**—proved most influential for distinguishing among healthy, 2.96%, 5.92%, and 8.87% mass loss. 

I decided to **filter out frequencies** outside these ranges, drastically reducing each sample’s sequence length. This allowed the transformer’s attention mechanism to focus on the critical frequency bands where damage signatures were most apparent.

**Result:**  
Accuracy began to climb, confirming that focusing on essential frequency ranges helps the model learn discriminative features.

---

## 3. Hybrid CNN + Transformer

Next, I experimented with a **hybrid approach**, incorporating **convolutional layers** before the transformer encoder. Convolutions are excellent at extracting local patterns—like resonance peaks—while also reducing sequence length. The reduced and more meaningful feature maps then fed into the transformer layers for global pattern recognition.

Although the improvement wasn’t as dramatic as frequency filtering, it still contributed to better overall accuracy and stability during training.

---

## 4. Hyperparameter Tuning and Batch Size

### Increasing the Number of Layers

I discovered that increasing the **number of transformer encoder layers** from 2 to 3 provided a more expressive model. The deeper architecture captured more nuanced relationships in the data, further improving performance.

### Reducing the Batch Size

Another significant gain came from lowering the **batch size**:
- When **batch_size = 8**, training accuracy sometimes soared, but validation accuracy stagnated or regressed.
- Dropping to **batch_size = 4** provided a smoother, more stable learning curve.
- Finally, moving to **batch_size = 2** resulted in even better validation accuracy and consistent improvements across epochs.

**Intuition:**  
With a smaller batch size, the model updates weights more frequently and avoids some local minima or plateaus in high-dimensional weight space. In a **small dataset** scenario, large batch sizes can lead to quick but misleading convergence.

---

## 5. Final Results

By combining:
1. **Filtering out irrelevant frequencies** (10–40 Hz, 120–160 Hz, 350–450 Hz),
2. **A deeper transformer encoder** (3 layers),
3. **A smaller batch size** (2),
4. **Careful early stopping** (to avoid overfitting),

I achieved the following **test accuracy** of **0.9786**:

