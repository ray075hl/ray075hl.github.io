---
layout: post
title: Open-AI Gym
data: 2018-10-02 23:59:00
img: aigame.png
tag: [machine learning]
---

## 1. The anatomy of the agent

* **Agent** 
* **Environment**

```python
import random

class Environment:
    def __init__(self):
        self.steps_left = 10

    def get_observation(self):
        return [0.0, 0.0, 0.0]

    def get_actions(self):
        return [0, 1]

    def is_done(self):
        return self.steps_left == 0

    def action(self, action):
        """
        This method is the central piece in the
        environment's functionality. It does two things
        * handles the agent's action
        * returns the reward for this action
        """
        if self.is_done():
            raise Exception("Game is over")
        self.steps_left -= 1
        
        return random.random()


class Agent:
    def __init__(self):
        self.total_reward = 0.0

    def step(self, env):
        """
        * Observe the environment
        * Make a decision about the action to take based
          on the observations
        * Submit the action to the environment
        * Get the reward for the current step
        """
        current_obs = env.get_observation()
        actions = env.get_actions()
        reward = env.action(random.choice(actions))
        
        self.total_reward += reward

if __name__ == "__main__":
    env = Environment()
    agent = Agent()
    while not env.is_done():
        agent.step(env)

    print("Total reward got: {:4}".format(agent.total_reward))
```

On every step, an agent takes some observations from the environment, does its  

calculations, and selects the action to issue. The result of this action is a **reward** and    

**new observation**.



## 2. Hardware and software requirements

**Software**:

- Numpy
- OpenCV Python bindings
- Gym
- PyTorch
- Ptan           

**Hardware**:

* GPU

  

## 3. OpenAI Gym API

The main goal of Gym is to provide a rich collection of environments for RL experiments  

for RL experiments using a unified interface. So, it's not surprising that the central class in the library is an   

an environment, which is called Env. From high level, every environment provides you with these  

pieces of information and functionality:

* A set of actions that are allowed to be executed in an environment. Gym supports both  discrete and continuous actions, as well as their combination. 
* The shape and bondaries of the observations that an environment provides the agent with.
* A method called step to execute an action, which returns the current observation, reward, and indication that the episode is over.
* A method called reset to return the environment to its initial state and to obtain the first observation.

## 4. Action space

**Discrete actions**, for example, directions in a grid like left, right, up, or down.

**Continuous action**, for example,  an accelerator pedal, it's usually from 0 to 1; in the case of

a steering wheel, it could be from -720 degrees to 720 degrees.

## 5. Observation space

Could be any thing, such as an image (3 dimensional tensors) or a scalar and an Boolean value. 

## 6. The environment

The environment is represented in Gym by the Env class, which has the following members:

* action_space: This is the field of the Space class, providing a specification for allowed actions in the  environment.

* observation_space: This field has the same Space class, but specifies the observations provided by the environment.

* reset() : This resets the environment to its initial state, returning the initial observation vector

* step() : This method allows the agent to give the action and returns the information about the outcome of the action: the next observation, local reward, and end-of-episode flag.

  ​

## 7. Creation of the environment

To create the environment, the Gym package provides the make(env_name) function with the only argument of the environment's name in the string form.

## 8. The CartPole session

```python
import gym
e = gym.make('CartPole-v0')
obs = e.reset(); print(obs)
print(e.action_space)
print(e.observation_space)
print(e.step(0))
print(e.action_space.sample())
print(e.observation_space.sample())
```

###8.1 The random CartPole agent

```python
import gym
if __name__ == "__main__":
  env = gym.make("CartPole-v0")
  total_reward = 0.0
  total_steps = 0
  obs = env.reset()
  while True:
    action = env.action_space.sample()
    obs, reward, done, _ = env.step(action)
    total_reward += reward
    total_steps += 1
    if done:
      break
  print("Episode done in {} steps, total reward {:4}".format(total_steps, total_reward))
```

## 9. The extra Gym functionality - wrappers and monitors



### 9.1 Wrappers

The Wrapper class inherits the Env class. Its constructor accepts the only argument: the instance of the Env  

class to be wrapped. About environment.

### 9.2 Monitor

Monitor class is implemented for recording agent's performance.
