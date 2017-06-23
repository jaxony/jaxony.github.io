---
layout: post
title: "Machine Learning Made Simple: Depthwise Separable Convolutions"
---

In the *Machine Learning Made Simple* series, I explain the basic idea behind a concept and why one might find it useful in practical ML applications.

---

Recently I've been interested in efficient deep learning models that don't require lots of computing power and memory to run. Lowering the computational and memory bandwidths is important for running models on mobile and embedded devices.

It turns out that **Depthwise Separable Convolutions** (DSCs), a twist on the traditional convolutional operations that we know and love, offer an easy way to decrease memory usage while maintaining the performance of convolutional models.

## Why should I care?
Before we get into how depthwise separable convolutions work, there are (at least) two reasons why you should care about using them in the first place.

**Memory efficiency.** DSCs use much less parameters than traditional convolutions.

**Still performs well.** DSCs perform comparable to traditional convolutions in many machine learning tasks, including object recognition, finegrained classification, etc.

## What is a Depthwise Separable Convolution?
A traditional convolution treats an input 'block' (more formally called a tensor) as just that: a big 3D block of numbers. Then, a 3-dimensional filter of shape ```(K, K, C)``` is gradually convolved (i.e. dot-producted) with the entire input block by striding through all possible positions along the height and width axes of the input tensor. (The depth axis doesn't require any striding in a traditional convolution because the conv filter's depth ```C``` covers the whole depth of the input tensor, which is always ```C```.) Then, out we get a single activation map. This process is repeated ```N``` times for the ```N``` convolutional filters being learned at this layer.

A depthwise separable convolution is different from traditional convolutions in one main way:

>A depthwise 'separable' convolution is actually *separated* into two sequential convolutions: a depthwise convolution followed by a 1-by-1 (pointwise) convolution.

### Depthwise separable convolution in 5 steps
1. Split an input tensor channelwise or depthwise. In other words, break the 3D input tensor into ```C``` 2D tensors.
2. Apply a traditional convolution at every depth, i.e. on each of these 2D tensors. Each depth / channel has its own convolutional weights.
3. By convolving separately at every individual depth, or channel, we now have a stack of ```C``` 2D convolutional outputs. These channel-specific spatial activations are concatenated together to form a 3D tensor.
4. Finally, a 1x1 or pointwise convolution is applied to the 3D tensor; this is equivalent to a linear combination of features across channels.
5. The output is a 2D tensor whose values can be interpreted as being inter-channel (merged together by the pointwise convolution) spatial features (discovered by depthwise convolutions). Repeat the pointwise convolution ```N``` times on ```N``` distinct 1x1 convolutions to get ```N``` of such maps.

At the end, we have a 3D convolutional output, just like from a traditional convolution. The differences are:

- The above 5 steps of DSCs use much less parameters.
- Much less computation is used.

## Why does the Depthwise Separable Convolution even work?

**More efficient use of parameters.** Traditional convolutions find spatial features while merging them across channels, which may not be the most efficient purposing of its parameters. Depthwise separable convolutions simply decouples this process: DSCs filter spatial features first and then merge them across channels.

- Spatial and depth are independent of each other.
- draft version... still editing!

## References
This post is a summary of key points from the following papers:
1. Xception by Chollet (2016)
2. Google's MobileNets paper (2017)
3. ...'s paper: Neural Machine Translation with DSCs (2017)