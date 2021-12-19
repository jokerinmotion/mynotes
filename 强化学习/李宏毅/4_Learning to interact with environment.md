# 	Learning to interact with environment

![image-20211207154025948](images/image-20211207154025948.png)

Actor, Environment, Reward三者的关系：

<img src="images/image-20211207162544526.png" alt="image-20211207162544526" style="zoom:50%;" />

## Critic

### 状态价值函数V^π^(s)

![image-20211208200614223](images/image-20211208200614223.png)

注意到，以上是critic的一种，**会随着actor的不同（也就是π），Critics给出的评价也不同**

如何estimate 这个V^π^(s)：

- Monte-Carbo based approach（蒙特卡罗）

<img src="images/image-20211208205252226.png" alt="image-20211208205252226" style="zoom: 67%;" />

- Temporal-difference approach（时序差分）

<img src="images/image-20211208205428046.png" alt="image-20211208205428046" style="zoom:67%;" />

- 两者区别

暂略

### 动作价值函数（Q^π^(s,a)，Q函数）

![image-20211208211626335](images/image-20211208211626335.png)

对于离散的动作，可以改写Q函数：

![image-20211208212307707](images/image-20211208212307707.png)

💡使用Q函数进行策略迭代（Q-Learning）

![image-20211208213119386](images/image-20211208213119386.png)

原理：

![image-20211208213438319](images/image-20211208213438319.png)

注意：这里的action只能是离散的（左移、右移、开火）

💡DQN的七种实现和改进：**论文-Rainbow**



## Actor-Critic

> 之前学习actor的时候是看reward function的输出来看如何update actor，在互动过程中有非常大的随机性，AC的精神在于不去看环境的reward了，因为环境的raward变化太大，而是跟Critic学。



### 优势演员-评论家算法

advantage AC（A2C）

<img src="images/image-20211208220652808.png" alt="image-20211208220652808" style="zoom:50%;" />



### 异步优势演员-评论家算法(A3C)

<img src="images/image-20211208223525632.png" alt="image-20211208223525632" style="zoom:67%;" />

### 特例：路径衍生策略梯度

**pathwise derivative policy gradient**，路径衍生策略梯度，看成DQN解连续动作的一种方法，也是一种特殊的AC方法。

一般Q-Learning只能处理离散的动作，如果是连续的，那就要训练actor π，它输出的action是能让Q函数的值最大。原理图如下：

![image-20211208224421833](images/image-20211208224421833.png)

![image-20211208224440397](images/image-20211208224440397.png)

![image-20211208230158358](images/image-20211208230158358.png)

![image-20211208225532974](images/image-20211208225532974.png)

## Inverse RL

没有reward function。因为现实中的很多问题就是不好定义reward的。

用 Inverse RL推出reward function，然后再用RL去找最好的actor

![image-20211208232137023](images/image-20211208232137023.png)

逆强化学习的原理框架：

![image-20211208232752330](images/image-20211208232752330.png)

![image-20211208232858510](images/image-20211208232858510.png)

