---
layout: post
title: "Texture and Shape Bias in Neural Networks.md"
author: "Matthew Kowal"
meta: "Springfield"
--- 

intro /
Texture bias in general /
Proof that texture 'bias' exists from first paper /
What is 'bias' /
ImageNet-trained CNNs are biased towards texture; increasing shape bias improves accuracy and robustness / 
Exploring the Origins and Prevalence of Texture Bias in Convolutional Neural Networks / 
Future Work / 
Conclusion


When we design systems using AI, it is important that our models are learning *exactly* what we want them to learn. Particularly for systems in which failure is an absolute disaster (e.g., nuclear power plants, data centers, self driving car visual systems, etc.), it is crucial to understand exactly where failure cases are prone to occur and why do they occur in the first place. Only when we understand our systems can we focus our energy into designing them in a safe way, robust to failures. Yann LeCunn recently [made a twitter thread](https://twitter.com/ylecun/status/1225122824039342081) about how we don't actually fully understand the physics behind the reason why airplanes can fly, pointing out that there isn't a "casual" explination for it. While I appreciate his point, I think there is a large difference between the level of our understanding of airplanes and our level of understand of neural networks. It's true that we don't have the full casual mechanisms behind everything behind why airplane can fly, but we havn't been finding out massive revelations in the physics of existing commercial aeroplanes. Yes, there are new designs and research being done in aerodynamics but I mean that we don't often see airplanes flying in unexpected ways. In the case of neural networks, we are still finding that our models are acting in very unexpected ways even for the simplest of tasks, let alone real world applications like autonomous vehicles. 

It's well studied that both artificial convolutional neural networks and biological neural networks learn visual features in a hierarchical fashion (see [Visualizing Higher-Layer Features of a Deep Network](https://pdfs.semanticscholar.org/65d9/94fb778a8d9e0f632659fb33a082949a50d3.pdf) and [Neuroanatomy, Visual Cortex
](https://www.ncbi.nlm.nih.gov/books/NBK482504/)). Earlier layers learn kernels that activate from lower level stimuli like edges and blobs, while deeper layers learn to activate from more complex shapes and objects (depending on the task at hand). For example, take a look at the following image.

<p align="center">
  <img src="/images/feature_hierarchy.png"> This is what you get when you maximize the activation of certain layers. Later layers are highly activated by increasingly complex visual stimuli. (Images are [from Google](https://distill.pub/2017/feature-visualization/).
</p>

You can clearly see that different layers are maximally activated by a wide range of inputs, which become increasingly complex as the layers get deeper. Now although this seems like sufficient evidence that neural networks trained for image classification learn complex shapes (see layers Mixed4d and Mixed4e), it is not quite that simple. 

### Bag-of-Local Features

Question: what do you think the performance of a neural network would be when trained on image classification, if you restricted the [receptive field](https://medium.com/mlreview/a-guide-to-receptive-field-arithmetic-for-convolutional-neural-networks-e0f514068807) to < 15% of the total image size? That is, if the model can only make predictions based on tiny image patches, how well do we expect the model to perform? The intuitive answer says the model will perform poorly, as it is restricted from viewing any object in its entirety, and can only see textures in the form of image patches. However, a recent paper ([Approximating CNNs with Bag-of-local-Features models works surprisingly well on ImageNet
](https://arxiv.org/abs/1904.00760)) showed that these restricted models (named 'BagNets') actually perform close to the models that have a full receptive field and make predictions based on the entire image. This should naturally beg the question: "do our networks actually learn robust object shape?". 

In the paper mentioned above, they show only marginal performance drops on ImageNet when a networks receptive field is restricted from 224x224 (image size) to 33x33, 17x17, and 9x9 patches. This is solid evidence of two things: 1) ImageNet is mostly solvable by using only 'bag-of-local features' (texture based) approaches, and 2) 
