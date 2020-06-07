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


When we design systems using AI, it is important that our models are learning *exactly* what we want them to learn. Particularly for systems in which failure is an absolute disaster (e.g., nuclear power plants, data centers, self driving car visual systems, etc.), it is crucial to understand exactly where failure cases are prone to occur and why do they occur in the first place. Only when we understand our systems can we focus our energy into designing them in a safe way, robust to failures. Yann LeCunn recently [made a twitter thread](https://twitter.com/ylecun/status/1225122824039342081) about how we don't actually fully understand the physics behind the reason why airplanes can fly, pointing out that there isn't a "casual" explination for it. While I appreciate his point, I think there is a large difference between the level of our understanding of airplanes and our level of understand of neural networks. 

It's true that we don't have the full casual mechanisms behind everything behind why airplane can fly, but we havn't been finding out massive revelations in the physics of existing commercial aeroplanes every year since their creation. Yes, there are new designs and research being done in aerodynamics, but I am reffering to the fact that that we rarely see airplanes flying in unexpected ways, even though there are thousands of flights a day. In the case of neural networks, we are still finding that our models are acting in very unexpected ways even for the simplest of tasks, let alone real world mission critical applications like autonomous vehicles. 

 -NEED TRANSISSIION PARAGRAPH

It's well studied that both artificial convolutional neural networks and biological neural networks learn visual features in a hierarchical fashion (see [Visualizing Higher-Layer Features of a Deep Network](https://pdfs.semanticscholar.org/65d9/94fb778a8d9e0f632659fb33a082949a50d3.pdf) and [Neuroanatomy, Visual Cortex
](https://www.ncbi.nlm.nih.gov/books/NBK482504/)). Earlier layers learn kernels that activate from lower level stimuli like edges and blobs, while deeper layers learn to activate from more complex shapes and objects (depending on the task at hand). For example, take a look at the following image.

<p align="center">
  <img src="/images/feature_hierarchy.png"> This is what you get when you maximize the activation of certain layers. Later layers are highly activated by increasingly complex visual stimuli. (Images are [from Google](https://distill.pub/2017/feature-visualization/)).
</p>

You can clearly see that different layers are maximally activated by a wide range of inputs, which become increasingly complex as the layers get deeper. Now although this seems like sufficient evidence that neural networks trained for image classification learn complex shapes (see layers Mixed4d and Mixed4e), it is not quite that simple. 

### Bag-of-Local Features

The receptive field of a neural network, is the [region in the input space that a particular CNN’s feature is looking at (i.e. be affected by).](https://medium.com/mlreview/a-guide-to-receptive-field-arithmetic-for-convolutional-neural-networks-e0f514068807) Many of the state-of-the-art neural networks used these days have large receptive fields, larger than the actual image in fact, which allow the networks to learn correlations between large features in the image space. For example, if you want your network to be able to destinguish between a dog, a sled, and a dog sled, it clearly need to be able to identify that there is both a dog, or a sled, or both in the image, and identify the spatial relationship to the other objects in the image, in this case it could be that the dog is attached to the sled with reigns. 

<p align="center">
  <img src="/images/receptive_field_table.png"> 
  Receptive fields for various networks from the past few years. (Images are [from Google](https://distill.pub/2019/computing-receptive-fields/).
</p>

So given this understanding of a receptive field, what do you think the performance of a neural network would be when trained on image classification, if you restricted the receptive field to a tiny fraction (< ~15%) of the total image size? That is, if the model can only make predictions based on tiny image patches, how well do we expect the model to perform? The intuitive answer says the model will perform poorly, as it is restricted from viewing any object in its entirety and therefore has no reasoning about object shapes, and can only see textures in the form of image patches. However, a recent paper ([Approximating CNNs with Bag-of-local-Features models works surprisingly well on ImageNet
](https://arxiv.org/abs/1904.00760)) showed that these receptive field-restricted models (named 'BagNets' due to the similarity with the [bag-of-words](https://machinelearningmastery.com/gentle-introduction-bag-words-model/) algorithm) actually perform close to the models that have a full receptive field and make predictions based on the entire image. 

Another [paper](https://arxiv.org/abs/1811.12231) came out shortly after which showed that given the choice between predicting an image given its texture and shape, neural networks will almost always lean towards predicting the texture in an image instead of the shape. They did this by using [neural style transfer](https://towardsdatascience.com/light-on-math-machine-learning-intuitive-guide-to-neural-style-transfer-ef88e46697ee) to generate ImageNet images with a given texture (see Figure below for example of the so called Stylized ImageNet). This further promoted the belief that our networks are not learning as much shape information as previously thought. They then go on to show that encouraging neural networks to predict the shape of an image instead of texture, by means of pre-training on Stylized ImageNet, improves the performance on the original classification task with real world examples! However, we still need to know: how much do our networks learn about shape when trained for image classification?

<p align="center">
  <img src="/images/texture_shape_example.png"> (Figure taken [from the paper](https://arxiv.org/abs/1811.12231)).
</p>

- EXPLANATION OF LINEAR CLASSIFIER ON learned features
One way to answer this question was done in ["Exploring the Origins and Prevalence of Texture Bias in Convolutional Neural
Networks"](https://arxiv.org/pdf/1911.09071.pdf). They first train a network on ImageNet. Then, using
