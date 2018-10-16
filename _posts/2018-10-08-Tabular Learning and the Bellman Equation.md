---
layout: post
title: Tabular Learning and the Bellman Equation
img: aigame.png
date: 2018-10-08 20:00:00
tag: [machine learning]

---



## Value of state

In a formal way, the value of the state is: $$V(s) = \mathbb E[\sum_{t=0}^{\infin} r_{t}\gamma^{t}]$$ ,  where $$r_{t}$$ is the local reward obtained at the step $$t$$ of the episode.

## The Bellman equation

Using a recursive format define the value of state.

$$V_{s^{*}}(a)=\mathbb E_{s \sim S}[r_{s, a} + \gamma V_{s}]=\sum_{s \in S} p_{a, s^{*} \to s} (r_{s,a} + \gamma V_{s})$$

**Bellman optimality equation**

By combining the Bellman equation, for a deterministic case, with a value for stochastic actions, we get the Bellman optimality equation for a general case:

$$V_{s^{*}}(a)=\max_{a \in A}\mathbb E_{s \sim S}[r_{s, a} + \gamma V_{s}]=\max_{a \in A} \sum_{s \in S} p_{a, s^{*} \to s} (r_{s,a} + \gamma V_{s})$$

The optimal value of state is equal to the action, which gives us the maximum possible expected immediate reward plus discounted long-term reward for the next state.

## Value of action

It's not as fundamental as value, but we need it for our convenience. Value of action $$Q_{s, a}$$. Basically, it equals the **total reward** we can get by executing action $$a$$ in state $$s$$ and can be defined via $$V_{s}$$.  It is slightly more convenient in practice. In these methods, our primary objective is to get values of $$Q$$ for every pair of state and action.

$$Q_{s,a}=\mathbb E_{s^{*}\sim S}[r_{s,a}+\gamma V_{s^{*}}]=\sum_{s^{*}\in S}p_{a,s \to s^{*}}{r_{s,a}+\gamma V_{s^{*}}}$$ 

$$s$$ is current state, $$s^{*}$$ is next state. 

$$Q$$ for this state s and action $$a$$ equals the expected immediate reward and the discounted long-term reward of the destination state. We also can define $$V_{s}$$ via $$Q_{s,a}$$:

 $$V_{s}=\max_{a\in A}Q_{s,a}$$

This just means that the value of some state equals to the value of the maximum action we can execute from this state. we can also express $$Q(s, a)$$ via itself.

$$Q(s,a)=r_{s,a}+\gamma \max_{a^* \in A}Q(s^*,a^*)$$ 

$$s$$ is current state, $$s^{*}$$ is next state. 

## The Value iteration method

The procedure (for values of states) includes the following steps:

1. Initialize values of all states $$V_{i}$$ to some initial value (usually zero)

2. For every state $$s$$ in the **MDP**, perform the Bellman update:

    $$V_{s} \leftarrow \max_{a} \sum_{s^{*}}p_{a, s \to s^{*}}(r_{s,a}+\gamma V_{s^{*}}) $$  

3. Repeat step 2 for some large number of steps or until changes become too small   


In the case of action values (that is Q):

1. Initialize all $$Q_{s,a}$$ to zero

2. For every state s and every action a in this state,  perform update: 

   $$Q_{s,a} \leftarrow \sum_{s^{*}}p_{a, s \to s^{*}}(r_{s,a} + \gamma\max_{a^{*}}Q_{s^{*},a^{*}})$$  

3. Repeat step 2

