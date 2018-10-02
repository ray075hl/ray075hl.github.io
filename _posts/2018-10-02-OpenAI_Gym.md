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



## 3. OpenAI Gym API



## 4. Action space



## 5. Observation space



## 6. The environment



## 7. Creation of the environment



## 8. The CartPole session



###8.1 The random CartPole agent



## 9. The extra Gym functionality - wrappers and monitors



### 9.1 Wrappers



### 9.2 Monitor





