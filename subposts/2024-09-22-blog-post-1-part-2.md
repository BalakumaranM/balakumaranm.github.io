---
title: 'Damage Assessment of a Physical Beam Reinforced with Masses using Machine Learning - Part 2'
date: 2024-09-22
permalink: /posts/2024/09/blog-post-1/
tags:
  -shm
---

What is this? why do we need to convert that signal value to this ? 

Let’s go through one by one

You can see the above time domain signal particularly at the left acceleration to time graph. If you look closer you can see that the acceleration is not reaching a constant upper bound and lower bounds (like a sinusoidal wave pattern), instead it varies with every cycle, but it feels like following a pattern. Even though it doesn’t look like a sinusoidal wave pattern, the signal comprises of multiple sinusoidal wave patterns combined together.

To understand this, in simple terms, we can see that the motor at the one end of the cantilever beam is exciting the beam. If you zoom in to the motor. The motor will usually rotate and it will be converted to translation motion (up and down acceleration) by connecting some mechanism to it. In our case there are multiple weights attached to the beam. So imagine one vibration exciting another vibration (like rotating motor to translating acceleration, then it induces vibration in every masses attached to the cantilever beam one by one. Every single vibration has it’s own frequency and amplitude. Why is it so ? In simple terms while transfering one energy (vibration) from one medium to another medium some energy got lost and the vibration pattern varies. But thechnically there are multiple factors involved here like reflection, attuniation, and scattering or distortion.

So at the accelerometer, all this individual sinusoidal wave patterns were combined and create a complex pattern of vibration. So now our job is to untangle them into individual wave patterns. That is what is done above that from time-domain graph to frequency - magnitude(Inertance) graph.

![FourierTransform.png](/images/blog_related/fourier_transform.png)

How it is done ?

It is done using the method called Fast Fourier Transform (FFT). 

Here we will first look at the brute force method of how time-domain signal is converted to frequency-domain signal.

If you want to know how much of a particular sin wave is in a signal, just multiply the signal by the sin wave at each point and then add up the area under the curve.

![FourierTransform1.png](/images/blog_related/fourier1.png)
![FourierTransform2.png](/images/blog_related/fourier2.png)

As a simple example, say our signal is just a sin wave with a certain frequency, then pretend we dont know that. And we try to figure out which sin waves add to make it up, if you multiply the signal with a sin wave of arbirtary frequency. The wave are uncorrelated. Means you are just as likely to find places where they upto same both positive or both negative as they were have opposite sign, and therfore when you multiply them together the area above the x axis is equal to the area below the x axis, so these areas add up to zero, which means that frequency sine waves is not part of your signal, and this will be true for almost all frequencies you could try (assuming we are looking over a long enough time frame), the only exception is if the frequency of the sine wave exactly matches that of the signal. Now these waves are corelated, so their product is always positive so is the area under the curve. That indicated that this sine wave is part of our signal. 

And the same trick works even if the signal is composed of bunch of different frequencies. If the sin waves frequency is one of the components of the signal it will correlate with the signal producing a non zero area.And the size of the area tells you the relative amplitude of that frequency sin wave in the signal. 

![FrequencyDomainGraph.png](/images/blog_related/)

Repeat this process for all frequencies of sin wave and you get the frequency spectrum, essentially which frequencies are present and in what proportions.