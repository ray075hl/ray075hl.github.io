---
layout: post
title: Markov decision processes
data: 2018-09-30 16"30:00
img: aigame.png
tag: [machine learning]
---
## 1. Markov process

### 1.1 Markov property

To call such a system a MP, it needs to fulfil the Markov property,  
which means that the future system dynamics from any state have    
to depend on this state only.

### 1.2 Transition matrix

|       | sunny       | rainy       |
| ----- | ----------- | ----------- |
| sunny | <center>0.8 | <center>0.2 |
| rainy | <center>0.1 | <center>0.9 |

In practice, we rarely have the luxury of knowing the exact transition   
matrix. A much more real-world situation is when we have only observations   
of our systems's states, which are also called episodes:   

* sunny -> sunny -> rainy -> sunny
* rainy -> sunny -> sunny 
* sunny -> rainy -> rainy -> rainy -> rainy  

It's not complicated to estimate the transition matrix by our observation;  
we just count all the transitions from every state and normalize them to a  
sum of 1. The more observation data we have, the closer our estimation will  
be to the true underlying model.
### 1.3 Formal definition of Markov process

* A set of state (***S***) that a system can be in.
* A transition matrix (***T***), with transition probabilities,    
  which defines the system dynamics.

## 2. Markov reward process(MRP)

Add value to our transition from state to state. This value can be positive 
or negtive, large or small-it's just a number. This number we call it  
"reward".   


**Discount factor** $$\gamma{}$$ (gamma), a single number from 0 to 1(inclusive).

For every episode, we define **return** at the time t as this quantity:  
$$G_{t}=R_{t+1} + \gamma R_{t+2}+...=\sum_{k=0}^{\infty}r^{k}R_{t+k+1}$$

If $$\gamma{}$$ equals to 1, then return $$G_{t}$$ just equals a sum of all subsequent rewards, consider  

more about futures. If $$\gamma{}$$ equals 0,   

our return $$G_{t}$$ will be just immediate reward without any subsequent  
state and correspond to absolute short-sightedness.  

$$G_{t}$$ is not very useful in practive, as it was defined for every specific  
chain we observed from our Markov reward process, so it can vary widely even  
for the same state. However, if we go to the extremes and calculate the   
mathematical expectation of return for any state (by averaging large amount of  
chains), we'll get a much more useful quantity, called a **value of state**:  

$$V(s)=E[G|S_{t}=s]$$

## 3. Markov decision process

To turn MRP into an MDP, we need to add actions to our reward matrix in the same way we  

did with the transition matrix: our reward matrix will depend not only state but also on action.  

In other words, it means that the reward the agent obtains now depends not only no the state  

it ends up in but also on the action that leads to this state.

### 3.1 Policy

The intuitive definition of policy is that it is some set of rules that controls the agent's behavior.   

Policy is defined as the probability distribution over actions for every possible state:   

$$\pi(a|s)=P[A_{t}=a|S_{t}=s]$$   

This is defined as probability, not as a concrete action, to introduce randomness into an agent's   

behavior.