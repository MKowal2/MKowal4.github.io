---
layout: post
title: "Texture and Shape Bias in Neural Networks.md"
author: "Matthew Kowal"
meta: "Springfield"
--- 

intro: motivation, why do we care about learning what spurious correlations are being learned if the performance is good?/
Proof that CNNs classificy based on textures from Approximating paper/
ImageNet-trained CNNs are biased towards texture; increasing shape bias improves accuracy and robustness / 
  What is 'bias' /
Exploring the Origins and Prevalence of Texture Bias in Convolutional Neural Networks / 
Future Work: Flaws of 'bias' metric, how can we actually quantify the amount of shape information contained in CNNs?/ 
Conclusion

### Aeroplanes and ConvNets

When we design systems using AI, it is important that our models are learning *exactly* what we want them to learn. Particularly in systems for which failure is an absolute disaster (e.g., nuclear power plants, data centers, self driving car visual systems, etc.), it is crucial to understand where failure cases are prone to occur and why they occur. Only when we understand our systems can we focus our energy into designing them in a safe way, robust to failures. In the case of neural networks, we are still finding that our models are acting in very unexpected ways even for the simplest of tasks. It happens time and time again, where it seems like the community has a grasp on how and what neural networks are learning in their internal representations, followed by a revelation that neural networks might not be understanding what we think they are.

### CNNs Learn Hierarchical Features

It's well studied that both convolutional neural networks (CNNs) and biological neural networks learn visual features in a hierarchical fashion (see [Visualizing Higher-Layer Features of a Deep Network](https://pdfs.semanticscholar.org/65d9/94fb778a8d9e0f632659fb33a082949a50d3.pdf) and [Neuroanatomy, Visual Cortex
](https://www.ncbi.nlm.nih.gov/books/NBK482504/)). Earlier layers learn filters that activate from lower level stimuli like edges and blobs, while deeper layers learn to activate from more complex shapes and objects (depending on the task at hand). For example, take a look at the following image.

<p align="center">
  <img src="/images/feature_hierarchy.png"> Figure 1. This is what you get when you maximize the activation of certain layers. Later layers are highly activated by increasingly complex visual stimuli. (Images are [from Google](https://distill.pub/2017/feature-visualization/)).
</p>

You can clearly see that different layers are maximally activated by a wide range of inputs, which become increasingly complex as the layers get deeper. Now although this seems like strong evidence that neural networks trained for image classification learn complex shapes (see layers Mixed4d and Mixed4e), it is not quite that simple. 

### Bag-of-Local Features

The receptive field of a neural network, is the [region in the input space that a particular CNN’s feature is looking at (i.e. be affected by)](https://medium.com/mlreview/a-guide-to-receptive-field-arithmetic-for-convolutional-neural-networks-e0f514068807). Many of the state-of-the-art neural networks used these days have large receptive fields, larger than the actual image in fact, which allow the networks to learn correlations between large features in the image space. For example, if you want your network to be able to destinguish between a dog, a sled, and a dog sled, it clearly need to be able to identify that there is both a dog, or a sled, or both in the image, and identify the spatial relationship to the other objects in the image, in this case it could be that the dog is attached to the sled with reigns. 

<p align="center">
  <img src="/images/receptive_field_table.png"> 
  Figure 2. Receptive fields for various networks from the past few years. (Images are [from Google](https://distill.pub/2019/computing-receptive-fields/).
</p>

Given this understanding of a receptive field, what do you think the performance of a neural network would be when trained on image classification, if you restricted the receptive field to a tiny fraction (< ~15%) of the total image size? That is, if the model can only make predictions based on tiny image patches, how well do we expect the model to perform? The intuitive answer says the model will perform poorly: it is restricted from viewing any object in its entirety and therefore has no reasoning about object shapes, and can only see textures in the form of image patches. However, a recent paper ([Approximating CNNs with Bag-of-local-Features models works surprisingly well on ImageNet
](https://arxiv.org/abs/1904.00760)) showed that these receptive field-restricted models (named 'BagNets' due to the similarity with the [bag-of-words](https://machinelearningmastery.com/gentle-introduction-bag-words-model/) algorithm) actually perform close to the models that have a full receptive field and make predictions based on the entire image. 

This result was quite shocking: it is possible to achieve comparable image recognition performance by using only local patches! A natural question to ask is "how much global vs local information is contained in CNNs?". The rest of this blog post is dedicated to a comparison of three techniques which aim to answer this question, the last of which is written by (shameless plug) yours truly :) .

The first [paper](https://arxiv.org/abs/1811.12231) which quantifies this local vs global representation bias did so in the context of shape (global) and texture (local). They showed that given a cue-conflict image, which has two labels corresponding to its texture and shape (generated through the use of [neural style transfer](https://towardsdatascience.com/light-on-math-machine-learning-intuitive-guide-to-neural-style-transfer-ef88e46697ee), see Figure 3 below for an example of the so called Stylized ImageNet), neural networks will almost always lean towards predicting the texture in an image instead of the shape. They call networks which predict texture more often than shape 'Texture Biased' while humans are 'Shape Biased'. Moreover, they demonstrated that by training a network to predict the shape label of these stylized images, they increase the shape-bias of the models which occur other nice properties, such as robustness to corruptions.

<p align="center">
  <img src="/images/texture_shape_example.png"> Figure 3. A cue-conflict image with two labels. Figure taken [from the paper](https://arxiv.org/abs/1811.12231).
</p>

By demonstrating that all CNNs are texture biased, the belief that our networks do not contain much shape information grew. However, the question still remains: "how much shape information do CNNs encode in their internal representations?". One way to answer this question was done in the study: [Exploring the Origins and Prevalence of Texture Bias in Convolutional Neural
Networks](https://arxiv.org/pdf/1911.09071.pdf).

- EXPLANATION OF LINEAR CLASSIFIER ON learned features
One way to answer this question was done in ["Exploring the Origins and Prevalence of Texture Bias in Convolutional Neural
Networks"](https://arxiv.org/pdf/1911.09071.pdf). They first train a network on ImageNet. Then, using
