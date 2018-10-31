---
layout: post
img: aigame.png
date: 2018-10-31 18:00:00
title: Policy Gradients
tag: [machine learning]
---

## Policy Gradients



### Why policy



>**REINFORCE METHOD**
>
>1. Initialize the network with random weights  
>
>2. Play N full episodes, saving their $$(s, a, r, s')$$ transitions   
>
>3. For every step t of every episode k, calculate the discounted total  
>
>   reward for subsequent steps $$Q_{k, t}=\sum_{i=0} \gamma^{i}r_{i}$$  .
>
>4. Calculate the loss function for all transitions $$L=-\sum_{k,t}Q_{k,t}\log(\pi(s_{k,t}, a_{k,t}))$$   
>
>5. Perform SGD update of weights minimizing the loss   
>
>6. Repeat from step 2 until converged   

**Different form Q-learning**

* No explicit exploration is needed. In Q-learning, we used an epsilon-greedy   

  strategy to explore the environment and prevent our agent from getting stuck  

  with non-optimal policy. Now, with probabilities returned by the network, the 

  exploration is performed automatically. In the beginning, the network is initialized

  with random weights and the network returns uniform probability distribution. 

  This distribution corresponds to random agent behavior.

* No replay buffer is used. PG methods belong to the on-policy methods class, 

  which means that we can't train on data obtained from the old policy. This is 

  both good and bad. The good part is that such methods usually converge faster. 

  The bad side is they usually require much more interaction with the environment

  than off-policy methods such as DQN.

* No target network is needed. Here we use Q-values, but they're obtained from

  from our experience in the environment. In DQN, we used the target network to  

  break the correlation in Q-values approximation, but we're not approximating it  

  anymore. 