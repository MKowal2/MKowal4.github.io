---
layout: post
title: "Texture and Shape Bias in Neural Networks.md"
author: "Matthew Kowal"
meta: "Springfield"
--- 

When we design systems using AI, it is important that our models are learning *exactly* what we want them to learn. Particularly for systems in which failure is always an absolute disaster (e.g., nuclear power plants, data centers, self driving car visual systems, etc.), it is crucial to understand exactly where and why failure cases are prone to occur. Only when we understand our systems can we focus our energy into designing them in a safe way, robust to failures. Yann LeCunn recently [made a twitter thread](https://twitter.com/ylecun/status/1225122824039342081) about how we don't actually fully understand the physics behind the reason why airplanes can fly, pointing out that there isn't a "casual" explination for it. While I appreciate his point, I think there is a large difference between the level of our understanding of airplanes and our level of understand of neural networks. It's true that we don't have the full casual mechanisms behind everything effective airplane flight, but we havn't been finding out massive revelations in the physics of existing commercial aeroplanes. Yes, there are new designs and research being done in aerodynamics but I mean that we don't often see airplanes flying in unexpected ways. In the case of neural networks, we are still finding that our models are acting in very unexpected ways even for the simplest of tasks, let alone real world applications like autonomous vehicles. 

It's well studied that both artificial convolutional neural networks and biological neural networks learn visual features in a hierarchical fashion (see [Visualizing Higher-Layer Features of a Deep Network](https://pdfs.semanticscholar.org/65d9/94fb778a8d9e0f632659fb33a082949a50d3.pdf) and [Neuroanatomy, Visual Cortex
](https://www.ncbi.nlm.nih.gov/books/NBK482504/)). Earlier layers learn kernels that activate from lower level stimuli like edges and blobs, while deep layers learn more complex shapes and objects (depending on the task at hand). For example, take a look at the following image.

<p align="center">
  <img src="/images/feature_hierarchy.png"> This is what you get when you maximize the activation of certain layers. Later layers are highly activated by increasingly complex visual stimuli. (Images are [from Google](https://distill.pub/2017/feature-visualization/).
</p>


