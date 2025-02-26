---
title: "Transformer based Structural Health Monitoring using Frequency Response Function (FRF) - Part 4"
date: 2024-09-22
permalink: /posts/2024/09/blog-post-4/
tags:
  - shm
---

Hey everyone, welcome to the final part of this series. In the earlier posts, we walked through our beam-signal dataset, explored time-domain vs. frequency-domain representations, and even dove into some time-frequency insights using the STFT. Today, I’m excited to share how I trained our transformer model and the many lessons I learned along the way.

I won’t get into the nitty-gritty of how transformers work from the ground up—trust me, that could fill a book! If you’re curious about the deep theory, I highly recommend checking out courses like MIT’s 6.S191 or Andrew Ng’s deep learning series. Instead, I’ll jump straight into the practical side of things: what I did, the hurdles I faced, and how I eventually got the model to work.

I started with a simple transformer encoder model using these hyperparameters:
- **d_model:** 64
- **Attention heads:** 4
- **Encoder layers:** 2
- **Dropout:** 0.1
- **Batch size:** 8
- **Learning rate:** 1e-4
- **Max sequence length:** 1100 (downsampled from the original 6400 points)

Right off the bat, the model performed terribly—its best accuracy was barely around 25%. I was puzzled. I thought, “The dataset’s small, yes, but shouldn’t the model be able to learn something meaningful?” I tried everything: unwrapping the phase data to smooth out those abrupt jumps, normalizing, and even smoothing the signals. Nothing seemed to help.

Then it hit me. I started looking at the frequency-magnitude graphs (you might remember those from Part 1 and 2) and noticed that not all frequencies are equally important. The first three vibrational modes—roughly between 10–40 Hz, 120–160 Hz, and 350–450 Hz—were clearly the key players in differentiating between healthy and damaged states. I decided to filter out frequencies outside these ranges, which made a world of difference. Suddenly, the transformer could focus on the parts of the frequency domain that really mattered.

I also experimented with a hybrid approach, adding a few convolutional layers before the transformer. Convolutional networks are fantastic at grabbing local patterns (like those resonance peaks we saw), and they helped reduce the sequence length even further. This hybrid model provided an extra boost, but it wasn’t until I tweaked the transformer itself that things really started to click.

One surprising yet crucial adjustment was reducing the batch size. With a batch size of 8, the model’s accuracy was erratic. When I lowered it to 4—and eventually to 2—the performance improved dramatically. With smaller batches, the model received more frequent weight updates, which seemed to help it navigate the complex loss landscape better, especially given our small dataset.

Here’s a quick rundown of my key observations:
- **Initial Setup:** With 6400-point data downsampled to 1100 and a batch size of 8, the model plateaued around 25% accuracy.
- **Preprocessing Tweaks:** I tried phase unwrapping, normalization, and smoothing, but these didn’t make a big impact on their own.
- **Focusing on Key Frequencies:** Filtering out everything except the critical bands (10–40 Hz, 120–160 Hz, and 350–450 Hz) dramatically improved the model’s ability to learn.
- **Hybrid CNN-Transformer Model:** Adding convolutional layers helped extract local patterns and reduce the sequence length before feeding the data into the transformer.
- **Hyperparameter Tuning:** Increasing the number of transformer layers from 2 to 3 and reducing the batch size from 8 to 2 were game changers.

The final model achieved an impressive **test accuracy of 97.86%**. Here’s the classification report for a quick look:

         precision    recall  f1-score   support

 Healthy       0.99      0.94      0.96        70
accuracy                           0.98       280


And here’s the confusion matrix that shows just how well the model distinguishes among the classes:

<div style="text-align: center;">
  <img src="/images/blog_related/confusion_matrix.png" alt="Confusion Matrix" style="width:50%;">
  <p><strong>Confusion Matrix: Transformer Model on Beam-Signal Dataset</strong></p>
</div>

So what’s the takeaway? It’s not just about having a fancy model architecture. Success here depended on truly understanding the data. By narrowing our focus to the most informative frequency bands and carefully tuning the training process, we managed to overcome the limitations of a small dataset and get the transformer to really shine.

Thank you for following along through this deep dive into our transformer model training. I hope these insights help you in your own projects on structural health monitoring or any other area where AI meets real-world data. Feel free to leave your thoughts or questions below—I'd love to hear about your experiences or any challenges you've faced.

Until next time, keep exploring and learning!

