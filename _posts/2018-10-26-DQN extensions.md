---
layout: post
img: aigame.png
date: 2018-10-26 18:00:00
tags: [machine learning]
title: DQN的扩展
---

为了让**DQN**的训练变得稳定,以及加速收敛,可以采用以下的一些改进技巧.

* **N-steps DQN:** How to improve convergence speed and stability with a 

  simple unrolling of the Bellman equation and why it's not an ultimate solution

* **Double DQN:** How to deal with **DQN** overestimation of the values of actions

* **Noisy networks:** How to make exploration more efficient by adding noise to 

  the network weights

* **Prioritized replay buffer:**  Why Uniform sampling of our experience is not the 

  best way to train

* **Dueling DQN:** How to improve convergence speed by making our network's 

  architecture closer represent the problem we're solving

* **Categorical DQN:** How to go beyond the single expected value of action and 

  work with full distributions