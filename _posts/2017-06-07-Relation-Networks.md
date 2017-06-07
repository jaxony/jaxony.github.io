---
layout: post
title: Relation Networks
---

![Relation Network architecture](/images/relation-network-architecture.png "Relation Network architecture for Visual Question Answering task, taken from the original paper Santoro et al. (2017)")

I came across DeepMind Co-Founder Shane Legg's [tweet](https://twitter.com/ShaneLegg/status/872008396748337152) about the Relation Network (RNs), a novel neural network architecture that does extremely well (*super-human-level* "well"!) at the task of reasoning about relationships between objects--a task that deep learning has struggled to surmount until now.

The unique intersection of incredible performance with elegant design and novel thinking really gave me no choice but to blog about Relation Networks.

> When something this simple is this powerful, you're probably onto something (Shane Legg's tweet).

You can find the paper, published on 7 June 2017, over [at arXiv](https://arxiv.org/abs/1706.01427).

# What is Relational Reasoning?

> "What's the colour of the car that is closest to the white truck?"

An answer to a question like this requires you to identify the `white truck` and consider the spatial relationship between this truck and the other cars around it, and then output a colour.

This involves relational reasoning, because we are thinking about objects in relation to each other (in this case, the relationship just so happens to be a spatial relationship).

In the paper, the RN was asked to answer questions of the following types:

## Query Questions
These are easier to answer as they require simpler reasoning.

> "How many blue balls are there?" Answer: "3"
> "What is the colour of the cylinder?" Answer: "Red"

## Comparison Questions
These questions require the agent to consider relationships between multiple objects. Previous state-of-the-art systems, which were already extremely complex, struggled to deal with these types of questions, and this is where RNs really kick ass.

> "What is the size of the sphere farthest from the yellow cube?" Answer: "Large"

# What are the Key Contributions of RNs?
Santoro et al. at Deepmind have come up with an intuitive and elegant way to feed data into a multilayer perceptron (MLP) that makes sense for the relational reasoning task. All the other parts of the network, such as CNN and LSTM embeddings, are more or less conventional in visual question answering.

- Past approach: throw `n` objects as inputs into the MLP.
- Deepmind approach: explicitly construct pairwise relationships between `n` objects in the input to the MLP, such that there are `O(n**2)` inputs (as there are `O(n**2)` pairwise relations in a group of `n` objects).

This is so simple and intuitive, and yet so genius. I'm not if this has been done before, but it's elegant nevertheless upon first reading about it in the paper.

Why is this way of structuring the problem revolutionary? As the authors explain in their paper:

> [For the past approach:] An MLP would receive all objects from the object set simultaneously as its input. It must then learn and embed `n**2` (where `n` is the number of objects) identical functions within its weight parameters to account for all possible object pairings. This quickly becomes intractable as the number of objects grows.

In other words, it's inefficient and difficult for a neural network to learn `n**2` functions, one function for each object-object relationship, with the limited number of parameters that it has. So Deepmind is like: Why don't we just learn one universal relationship function that takes all object-object pairs as inputs by **feeding object pair as input** instead of the **whole set of objects** as input?

> [For the new approach:] Therefore, the cost of learning a relation function `n**2` times using a single feedforward pass per sample, as in an MLP, is replaced by the cost of `n**2` feedforward passes **per object set** (i.e., for each possible object pair in the set) and learning a relation function just once, as in an RN.

# Quantitative Improvements
I won't paste the whole paper here, but these are the two take-aways for performance.

- Relation Networks have improved on the state-of-the-art performance by **120%**. The second-best result was achieved by the authors' reimplementation of a highly complex architecture involving ResNet, LSTM and SA (selective attention).
- Relation Networks out-perform humans overall and in many of the relational reasoning tasks!
