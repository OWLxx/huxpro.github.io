---
layout:     post
title:      "Game AI- Movement"
subtitle:   "第三章"
date:       2018-04-27 22:00:00
author:     "Xin"
tags:
    - AI
    - Algorithms
---

角色AI包括两部分，decision making与movement.  资源管理类与回合制游戏不需要movement algorithm. 本章主要讨论不同的 AI-controlled movement algorithms.

本章包括以下部分。Basic movement algorithms, kinematic(运动学的) movement algorithms, steering behaviors, combining steering behaviors, predicting physics, jumping, coordinated movement, motor control, movement in the third dimension

![](/img/gai2.png)

### 1. Basic of movement algorithms

部分的移动算法只需要简单的输入，比如角色坐标与目标， 部分需要比较复杂的输入，比如需要知道环境建筑物，避免穿墙。

输出同样，某些游戏只有2-3种速度，因此比如看到西面的敌人，输出某一档速度与方向就好， 并不考虑移动的加速与减速。

目前，dynamic movement 逐渐在取代kinematic movement. dynamic 需要考虑物体的动量， 输出为给物体的力，或者加速度。Kinematic只是需要给出物体移动的方向。

[Flocking algorithm](https://en.wikipedia.org/wiki/Flocking_(behavior))同样是一种重要的movement算法

### 2. Kinematic movement algorithms

使用静态数据（位置，朝向，不包括速度），输出满意的速度(full speed), 没有加速度，但是加速减速的过程可以用几帧动画表示。

```python
class Kinematic:
    def __init__(self):
    	self.position = [0, 0]
    	self.orientation = 0.0  # 朝向, 一个角度即可
    	self.velocity = [0, 0]
    	self.rotation = 0.0
        
    def update(self, steering, time):
        self.position += self.velocity * time
        self.orientation += self.rotation * time
        # update velocity and rotation
        self.velocity += steering.linear * time
        self.rotation += sttering.rotation * time
       
```

```python

```