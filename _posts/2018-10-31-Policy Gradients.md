---
layout: post
img: aigame.png
date: 2018-10-31 18:00:00
title: Policy Gradients
tag: [machine learning]
---

## Policy Gradients

### <span style="color:red">Why policy</span> 

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

<span style="color:blue">**Different form Q-learning**</span>

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

### <span style="color:red">Policy-based versus value-based methods</span>

* Policy methods are directly optimizing what we care about: out behavior. The  

  value methods such as DQN are doing the same indirectly, learning the value  

  first and providing to us policy based on this value.

* Policy methods are on-policy and require fresh samples from the environment.  

  The value methods can benefit from old data, obtained from the old policy, human  

  demonstration, and other sources.

* Policy methods are usually less sample-efficient, which means they require more  

  interaction with the environment. The value methods can benefit from the large   

  replay buffers. However, sample efficiency doesn't mean that value methods are  

  more computationally efficient and very often it's the opposite. In the above example,  

  during the training, we need to access our NN only once, to get the probabilities of   

  actions. in DQN, we need to process two batch of states:  one for the current state and  

  another for the next state in the Bellman update.

### <span style="color: red">Limits of REINFOCE method</span>

* <span style="color:blue">Need full episode</span> to complete before we can start training.need more   

  episodes used for training. It's a monte carlo sample!  so it situation is 

  fine for short episodes in the CarPole.  

* <span style="color:blue">High gradients variance</span>. To overcome this, we can subtracting a value called  

  baseline from the $$Q$$. The possible choices of the baseline are as follows:

  * Some constant value, which normally is the mean of the discounted rewards
  * The moving average of the discounted rewards
  * Value of the state $$V(s)$$ 

* <span style="color:blue">Exploration</span>. In DQN we solved this using soft-epsilon action selection. 

  solution: add a entropy loss to punish the absolutely decision

  $$ H(\pi) =  -\sum \pi(a|s) \log\pi(a|s)$$  when policy is uniform(random), the $$H(\pi)$$ have 

  maximum value.

* <span style="color:blue">Correlation between samples</span>. To solve this problem, **parallel environments** are   

  normally used. we use several and use their transitions as training data.






















