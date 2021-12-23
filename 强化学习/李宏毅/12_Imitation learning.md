# Imitation learning 模仿学习IL

IL也叫：**示范学习（learning from demonstration），学徒学习（apprenticeship learning），观察学习（learning by watching）**。

很多时候是无法吃环境中得到明确的reward 的。

例子：自动驾驶撞人的reward 和撞动物的reward如何定很难，但是收集很多类的开车记录是可以做到的。

在模仿学习里面，我们介绍两个方法：**行为克隆（behavior cloning，BC）**和**逆强化学习（inverse reinforcement learning，IRL**），逆强化学习也被称为**逆最优控制（inverse optimalcontrol）**。



## Behavior Cloning行为克隆

BC和监督学习是一模一样的，即：尽量让机器学会和专家一样的行为。举例：

<img src="images/image-20211222161004115.png" alt="image-20211222161004115" style="zoom:67%;" />



### DAgger数据集聚合

但是光做BC也是不够的，原因是，专家的行为可能不够，可能agent永远不知道撞墙如何处理，因为训练数据里从来没有撞墙。

**解决方法：数据集聚合(dataset aggregation , DAgger)**

<img src="images/image-20211222162646648.png" alt="image-20211222162646648" style="zoom: 80%;" />

解释：

![image-20211222162827867](images/image-20211222162827867.png)

### BC的其他问题

- 完全模仿专家的行为，不知道哪些是重要的（举例：shedon学中文）

![image-20211222163052381](images/image-20211222163052381.png)

- 在做行为克隆的时候，训练数据跟测试数据是不匹配的。

在RL中，采取的action是会影响到接下来见到的state（但是一般的监督学习的训练数据都是独立的）。但是在做BC的时候，只能看到专家π hat 的一堆状态-动作对。

![image-20211222163604362](images/image-20211222163604362.png)

![image-20211222163628326](images/image-20211222163628326.png)



## Inverse RL逆强化学习

没有reward function。因为现实中的很多问题就是不好定义reward的。

### 做法

用 Inverse RL反推出reward function，然后再用RL去找最好的actor

![image-20211208232137023](images/image-20211208232137023.png)

![image-20211223113811136](images/image-20211223113811136.png)

### 原理

指导思想：“expert is always right”，也就是先射箭再画靶

逆强化学习的原理框架：

![image-20211223114043445](images/image-20211223114043445.png)

具体如何让专家得到的奖励大于演员呢？这边其实没有很具体的算法操作，一种做法可以是：

![image-20211223114337659](images/image-20211223114337659.png)

### 💡与GAN的联系

![image-20211223114808563](images/image-20211223114808563.png)

![image-20211223114921914](images/image-20211223114921914.png)

### 应用

#### 自动驾驶的停车

文献上真实的例子。在这个例子里面，逆强化学习有一个有趣的地方，通常你不需要太多的训练数据，因为训练数据往往都是个位数。

用不同的范例能使得汽车学到不同的开车风格：

![image-20211223115250087](images/image-20211223115250087.png)

（能不能够压到线，能不能够倒退，要不要遵守交通规则等等）

#### 教机械手臂特定动作

以前可能需要很多programming的动作，现在可以使用人的示范，直接教机器人特定动作。



## Third Person IL第三人称视角模仿学习

![image-20211223115854630](images/image-20211223115854630.png)

![image-20211223115903114](images/image-20211223115903114.png)

![image-20211223120000655](images/image-20211223120000655.png)

### 回顾：序列生成与聊天机器人

![image-20211223160254985](images/image-20211223160254985.png)
