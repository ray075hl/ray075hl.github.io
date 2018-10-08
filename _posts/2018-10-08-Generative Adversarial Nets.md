---
layout: post
date: 2018-10-08 16:35:00
title: Reading notes of paper "Generative Adversarial Nets"
tag: [paper, algorithm, machine learning]
img: night.jpg
---

## Adversarial nets

The adversarial modeling framework is most straightforward to apply when the models are both multilayer perceptrons. To learn the generator's distribution $$p_{g}$$ over data x, we define a prior on input noise variables $$p_{z}(z)$$, then represent a mapping to data space as $$G(z; \theta_{g})$$, where $$G$$ is a differentiable function represented by a multilayer perceptron with parameters $$\theta_{g}$$.

## Objective Function

$$\min_{\theta_{g}}\max_{\theta_{d}}[\mathbb{E}_{x\sim p_{data}}\log D_{\theta_{d}} + \mathbb{E}_{z\sim p(z)} \log(1-D_{\theta_{d}}(G_{\theta_{g}}(z)))] \tag {1}\\$$ 

A game-theoretic approach is taken, this objective function is represented as a minimax function. The discriminator tries to maximize the objective function, therefore we can perform gradient ascent on the objective function. The generator tries to minimize the objective function, therefore we can perform gradient descent on the objective function. By alternating between gradient ascent and descent, the network can be trained.

**Gradient Ascent on Discriminator**

$$\max_{\theta_{d}}[\mathbb{E}_{x\sim p_{data}}\log D_{\theta_{d}} + \mathbb{E}_{z\sim p(z)} \log(1-D_{\theta_{d}}(G_{\theta_{g}}(z)))]  \tag {2}\\$$ 

**Gradient Descent on Generator**

$$\min_{\theta_{g}}[\mathbb{E}_{z\sim p(z)} \log(1-D_{\theta_{d}}(G_{\theta_{g}}(z)))] \tag{3}\\$$ 

But when applied, it is observed that optimizing the generator objective function does not work so well, because at the beginning of training G is very weak and D can discriminate fake data with high confidence, this lead the gradient for training G is very small(nearly 0 when using sigmoid activate function), G will be hard to train. This phenomenon is called gradient saturate.

**new Generator objective function**

$$\max_{\theta_{g}} \mathbb{E}_{z\sim p(z)} \log (D_{\theta_{d}}(G_{\theta_{g}}(z))) \tag {4}\\$$ 