---
title: "When your RL agent starts gaming instead of learning"
date: 2026-06-23
description: "Your model starts playing the game instead of actual learning"
tags: []
categories: []
draft: false
---
In reinforcement learning, one of the toughest part is to design reward function. I read many papers and even books, there is no framework how you should design a reward function. At the beginning of Deep Reinforcement Learning course from UC Berkeley, Sergey emphasized that one of the unsolved challenges in the field is that "no clear what the rewards should be".

Reward hacking - it's when your RL model starts gaming to get the most rewards instead of trying to learn the thing you want it to learn.
For example, you said that you would reward 10 dollars if your kid arrange all the toys in the room. You check the room and you don't see any toy. Your kid will get that money. However, instead of actually tidying up, the kid hid all the toys under the bed.
It's not doing what it should do, it's just trying to get the reward, any way.

The hardest thing is, we cannot tell the model to learn exactly what we want it to learn. If that's clear, it's a classification problem instead. We give it the environment surrounding, give it good rewards for its good actions, and penalties to make it avoid what risky/bad actions.

That process is a loop. You design the reward, training and evaluating whether it learns on the right direction. Repeat couple of times (or many times, depends on, I don't know :)

There are few ways to avoid reward hacking:

1. Monitor chain of thoughts: Observe and spot cheating during training. 
2. Combine multiple metrics: In reward function, don't just rely on one metric. Should combine multiple proxy metrics.
3. Use adversarial evaluation: Treat your reward function as an adaptive agent itself that actively tries to catch the AI's tricks and exploits.

Read this blog from Lilian Weng if you want to dive deeper in to reward hacking: https://lilianweng.github.io/posts/2024-11-28-reward-hacking/

---
