---
layout: post
title: "Texture and Shape Bias in Neural Networks.md"
author: "Matthew Kowal"
meta: "Springfield"
--- 

When we design systems using AI, it is important that our models are learning *exactly* what we want them to learn. Particularly for mission critical systems, or systems for which failure is always an absolute disaster (e.g., nuclear power plants, data centers, self driving car visual systems, etc.), it is crucial to understand exactly where and why failure cases are prone to occur. Only when we understand our systems can we focus our energy into designing them in a safe way, robust to failures. Now Yann LeCunn recently made a post about how we don't actually fully understand the physics behind the reason why airplanes can fly [CITE], pointing out that the FILLTHISIN equation has not actually been solved analytically. While I appreciate his point, I think there is a large difference between the level of our understanding of airplanes and our level of understand of neural networks. Yes, we don't have the full equation for aeroplane figured out, but we havn't been finding out massive revelations in the physics of existing commercial aeroplanes. Yes, there are new designs and research being done in aerodynamics but I mean that we don't often see airplanes flying in unexpected ways. In the case of neural networks, we are still finding that our models are acting in very unexpected ways even for the simplest of tasks, let alone real world applications like autonomous vehicles. 

It's well studied that both artificial convolutional neural networks and biological neural networks learn visual features in a heiarchical fashion [CITE]. Earlier layers learn kernels that activate from lower level stimuli like edges and blobs, while deep layers learn more complex shapes and objects (depending on the task at hand). For example, take a look at the following image.

<p align="center">
  <img src="/images/feature_hierarchy.png"> This is what you get when you maximize the activation of certain layers. Later layers are highly activated by increasingly complex visual stimuli. (Images are [from Google](https://distill.pub/2017/feature-visualization/).
</p>


