---
layout: post
title: How to wrapper a atari game
date: 2018-12-05 18:00:00
tag: [programming]
img: aigame.png
---

## 如何封装一个atari game环境

Deepmind公司凭借着强化学习在atari game上的优异表现, 闻名于世. 强化学习也一时炙手可热. 想要学习强化学

习的知识, 除了学习理论公式, 那便是算法实作了. 在实作算法的第一步即, 需要一个游戏环境(environment), 假设游戏环境已经存在, 那么接下来就需要对其进行封装(wrapper), 使游戏的按照我们需要的格式与逻辑进行输出.

**以下用代码和注释来详细说明.**

```python
import gym
import cv2

class Game:
    
    def __init__(self, env_id):
		# 假设环境名称为: breakoutnoframeskip-v0
        # 这是一个视频类的游戏, 它的state是一张图片
		env_id = 'breakoutnoframeskip-v0'
        self.skip_frame = 4
        # Deepmind 在做视频类游戏时, 每次输入网络的是4帧图像,
        # 在这4帧图像中, 采取同一个动作, 文献中称为frame skip, 值置为4
        # self.obs2max 存储的是动作与环境交互4次, 产生4个新状态的后两个状态的处理结果
        # 它将作为一个新的训练帧append在self.obs4stack的最后
        self.obs2max = np.zeros((2, 84, 84, 1), np.uint8)
        
        self.obs4stack = np.zeros((84, 84, self.skip_frame))
        
        self.rewards = []
        self.lives = 3	# 游戏开始时 有几条命
    def step(self, action):
        reward = 0.
        done = False
        
        for i in range(self.skip_frame):
            
            obs, r, done, info = self.env.step(action) # 4帧图像中, 采取同一个动作
            
            if i >= self.skip_frame//2:
                self.obs2max[i%2] = self._process_obs(obs)
            
            reward += r
            lives = self.env.unwrapped.ale.lives() # 当前有几条命
            
            if lives < self.lives: # 如果发现命丢了, 说明这个episode结束了
                done = True
                
            self.lives = lives
            
            if done:  # 如果结束了 就不需要继续执行动作了
                break
        
        self.rewards.append(reward)
        
        if done:  # Episode结束了, 返回一些统计信息, 来描述这局游戏, 也可以用作debug.
            episode_info = {"reward": sum(self.rewards),
                           "length": len(self.rewards)}
        else:
            episode_info = None
            obs = self.obs2max.max(axis=0)
            
            # 其实 self.obs4stack 用双向队列(deque)的话, 会显得更加自然
            self.obs4stack = np.roll(self.obs4stack, shift=-1, axis=-1)
            self.obs4stack[..., -1] = obs # 加入新的一帧
        return self.obs4stack, reward, done, episode_info
    
    # 纵观整个step函数, 我们可以描述一下数据的堆栈的情况
    # 游戏帧数    ****|****|****|****|****|****|
    # 执行动作    aaaa|bbbb|vvvv|kkkk|mmmm|jjjj|
    # 训练数据    1234|234F|34FM|4FMB|FMBJ|MBJL|
    
    def reset(self):
        
        obs = self.env.reset()
        
        obs = self._process_obs(obs)
        # 初始化self.obs4stack
        self.obs4stack[..., 0:] = obs
        self.obs4stack[..., 1:] = obs
        self.obs4stack[..., 2:] = obs
        self.obs4stack[..., 3:] = obs
        
        self.rewards = [] # 清空rewards
        
        self.lives = self.env.unwrapped.ale.lives() # 重置生命
        
        return self.obs4stack  # reset 后返回state
    
    @staticmethod
    def _process_obs(obs):
        
        # 彩图变灰度图
        obs = cv2.cvtColor(obs, cv2.COLOR_RGB2GRAY)
        # 84 是deepmind 网络的输入尺寸
        obs = cv2.resize(obs, (84, 84), interpolation=cv2.INTER_AREA)
        return obs[:, :, None]  # shape is [84, 84, 1]
```



最近特别丧, 很久没有更新自己的部落格(虽然只有自己看)了,  不过当自己弄懂了很有用的知识的时候, 还是特别特别想要把它写下来.  

 