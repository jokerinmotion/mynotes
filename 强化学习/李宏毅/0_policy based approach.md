# 入门介绍

![image-20211130211503152](images/image-20211130211503152.png)

## Policy based approach

目的：learning an Actor(也叫policy就是一个function，一般称为$π$​​​)

$π$：Action = $π$​（Observation），如下图

<img src="images/image-20211130212936850.png" alt="image-20211130212936850" style="zoom: 67%;" />

### 原理

![image-20211209222753286](images/image-20211209222753286.png)

注意，policy gradient的参数每更新一次后，就要重新再去和环境互动collect data然后才能再次update 参数。

### 步骤：

- Step 1: define a set of function——用Neural network

输入：observation as a vector or matrix

输出：每个action对应一个**输出神经元**，一般表示为几率

![image-20211130214431170](images/image-20211130214431170.png)

比起传统的表格法，神经网络更能**泛化**

- Step 2: goodness of function 

和监督学习一样，使用代价函数，这里是使用R~θ~的期望值

![image-20211130215538745](images/image-20211130215538745.png)

每个回合表示为一个trajectory（序列）τ

![image-20211130220919970](images/image-20211130220919970.png)

**某个**τ能得到的奖励是

![image-20211130222035679](images/image-20211130222035679.png)

某个τ出现的概率表示为

![image-20211130220534696](images/image-20211130220534696.png)

具体可以写为：

![image-20211209215619287](images/image-20211209215619287.png)

因此，所有可能的τ产生的奖励就是奖励的期望值：

![image-20211130222139852](images/image-20211130222139852.png)

采样使用大数定律：

<img src="images/image-20211130222459864.png" alt="image-20211130222459864" style="zoom:67%;" />

- Step 3: pick the best function

使用gradient ascent（和descent不同，这里是要最大化目标函数R），具体步骤：

<img src="images/image-20211130222846280.png" alt="image-20211130222846280" style="zoom:67%;" />

具体的，如何计算R的微分：

<img src="images/image-20211130223311753.png" alt="image-20211130223311753" style="zoom:67%;" />

其中：

<img src="images/image-20211130223541756.png" alt="image-20211130223541756" style="zoom:67%;" />

然后取log:

<img src="images/image-20211130223627296.png" alt="image-20211130223627296" style="zoom:67%;" />

忽略与参数θ无关的项，求微分:

<img src="images/image-20211130225121044.png" alt="image-20211130225121044" style="zoom:67%;" />

最后的奖励期望值表示为：

![image-20211130225645530](images/image-20211130225645530.png)

上式需要注意的地方：

- 它是符合直觉的，它考虑的是整个trajectory的reward，而不是但一个![image-20211130230634889](images/image-20211130230634889.png)

- 加log是为了归一化

<img src="images/image-20211130231458329.png" alt="image-20211130231458329" style="zoom:67%;" />

### 技巧

#### 添加基线

- **一般可以加一个baseline**，以应对每个R(τ)都是正的情况：

<img src="images/image-20211130231949063.png" alt="image-20211130231949063" style="zoom: 50%;" />

<img src="images/image-20211130232044500.png" alt="image-20211130232044500" style="zoom:67%;" />

> “得到的奖励超过了某个baseline，才将这个动作出现的概率增加”

基线的取值一般为：把 *τ* *n* 的值取期望，算一下 *τ* *n* 的平均值，即：

![image-20211209224743180](images/image-20211209224743180.png)

#### 给每一个动作分配合适的分数

原因： sample的次数有时候不够多，

> 只要在同一个回合里面，在同一场游戏里面，所有的状态跟动作的对都会使用同样的奖励项进行加权，这件事情显然是不公平的，因为在同一场游戏里面也许有些动作是好的，有些动作是不好的。假设整场游戏的结果是好的，并不代表这个游戏里面每一个动作都是对的。若是整场游戏结果不好，但不代表游戏里面的所有动作都是错的。所以我们希望可以给每一个不同的动作前面都乘上不同的权重。每一个动作的不同权重，它反映了每一个动作到底是好还是不好。

本来的权重是整场游戏的奖励的总和，现在改成从某个时间 *t* 开始，假设这个动作是在 *t* 这个时间点所执行的，从 *t* 这个时间点一直到游戏结束所有奖励的总和，才能代表这个动作的好坏。写成式子如下所示：

![image-20211209225746646](images/image-20211209225746646.png)

再进一步，我们把未来的奖励做一个折扣，如下式所示，由此得到的回报被称为**折扣回报**（discounted return)

![image-20211209230447070](images/image-20211209230447070.png)

最后，把 *R* *−* *b* 这一项合起来，我们统称为**优势函数（advantage function）**，用 A^θ^(s~t~ , a~t~) 来代表优势函数。

