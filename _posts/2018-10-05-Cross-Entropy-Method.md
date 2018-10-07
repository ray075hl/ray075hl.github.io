---
layout: post
title: Cross Entropy Method
date: 2018-10-05 18:00:00
img: aigame.png
tag: [machine learning]
---

## Taxonomy of RL methods

* Model-free or model -based

* Value-based or policy-based

* On-policy or off-policy  

  ​


The term model-free means that the method doesn't build a model of the environment or reward; it just directly connects observations to actions (or values that are related to actions). In contrast, model-based methods try to predict what the next observation and/or reward will be.   

Policy-based methods are directly approximating the policy of the agent, that is, what actions the agent should carry out at every step. Policy is usually represented by probability distribution over the available actions. In contrast, the method could be value-based. In this case, instead of the probability of actions, the	agent calculates the value of every possible action and chooses the action with the best value.

Off-policy as the ability of the method to learn on old historical data (obtained by a previous version of the agent or recorded by human demonstration or just seen by the same agent several episodes ago). On-policy: it requires fresh data obtained from the environment.

***

The core of the cross-entropy method is to throw away bad episodes and train on better ones.

**Limitations of cross-entropy method:**

* For training, our episodes have to be finite and, preferably, short

* The total reward for the episodes should have enough variability to separate good episodes from bad ones

* There is no intermediate indication about whether the agent has succeeded or failed

  ​

## Theoretical background of the cross-entropy method

$$E_{x-p(x)}[H(x)]=\int_{x}p(x)H(x)dx=\int_{x}q(x)\frac{p(x)}{q(x)}H(x)dx=E_{x-q(x)}[\frac{p(x)}{q(x)}H(x)]$$

In our RL case, H(x) is a reward value obtained by some policy x and p(x) is a distribution  

of all possible policies. We don't want to maximize our reward by searching all possible policies, 

instead we want to find a way to approximate p(x)H(x) by q(x), iteratively minimizing the distance

between them.



**Kullback-Leibler** divergence

$$KL(p_{1}(x)||p_{2}(x))=E_{x-p_{1}(x)}\log \frac{p_{1}(x)}{p_{2}(x)}=E_{x-p_{1}}(x)[\log p_{1}(x)]-E_{x-p_{1}}(x)[\log p_{2}(x)]$$



