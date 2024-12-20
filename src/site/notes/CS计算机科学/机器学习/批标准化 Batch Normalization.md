---
{"dg-publish":true,"permalink":"/CS计算机科学/机器学习/批标准化 Batch Normalization/","noteIcon":"","created":"2024-08-28T17:27:38.000+08:00","updated":"2024-04-27T01:22:12.000+08:00"}
---


原文： https://zhuanlan.zhihu.com/p/24810318

**数学是达成目的的工具, 理解才是达成目的桥梁, 所以这篇文章用浅显易懂的动画阐述了复杂的机器学习概念.**  

强烈推荐通过动画的形式了解.

所以首先放视频链接: [Youtube](https://link.zhihu.com/?target=https%3A//www.youtube.com/watch%3Fv%3D-5hESl-Lj-4) 或者 [优酷](https://link.zhihu.com/?target=http%3A//v.youku.com/v_show/id_XMTg1MTYwNDg2OA%3D%3D).

代码实现请来这里看: [Python 实现](https://link.zhihu.com/?target=https%3A//morvanzhou.github.io/tutorials/machine-learning/torch/5-04-batch-normalization/)

## 普通数据标准化

![](/img/user/Z-attach/v2-7e50afe8a4840dfda6f3d2a64c817fd3_r.jpg.png)

Batch Normalization, 批标准化, 和普通的数据标准化类似, 是将分散的数据统一的一种做法, 也是优化神经网络的一种方法. 在之前 Normalization 的简介视频中我们一提到, 具有统一规格的数据, 能让机器学习更容易学习到数据之中的规律.

## 每层都做标准化

![](/img/user/Z-attach/v2-8cdb76b58fb2e84a7c59fcf15e0dfda3_r.jpg.png)

在神经网络中, 数据分布对训练会产生影响. 比如某个神经元 x 的值为 1, 某个 Weights 的初始值为 0.1, 这样后一层神经元计算结果就是 Wx = 0.1; 又或者 x = 20, 这样 Wx 的结果就为 2. 现在还不能看出什么问题, 但是, 当我们加上一层激励函数, 激活这个 Wx 值的时候, 问题就来了. 如果使用 像 tanh 的激励函数, Wx 的激活值就变成了 ~0.1 和 ~1, 接近于 1 的部已经处在了 激励函数的饱和阶段, 也就是如果 x 无论再怎么扩大, tanh 激励函数输出值也还是 接近 1. 换句话说, 神经网络在初始阶段已经不对那些比较大的 x 特征范围 敏感了. 这样很糟糕, 想象我轻轻拍自己的感觉和重重打自己的感觉居然没什么差别, 这就证明我的感官系统失效了. 当然我们是可以用之前提到的对数据做 normalization 预处理, 使得输入的 x 变化范围不会太大, 让输入值经过激励函数的敏感部分. 但刚刚这个不敏感问题不仅仅发生在神经网络的输入层, 而且在隐藏层中也经常会发生.

![](/img/user/Z-attach/v2-2b4d8694d6e6c6f42ecbaee95ae40eed_r.jpg.png)

只是时候 x 换到了隐藏层当中, 我们能不能对隐藏层的输入结果进行像之前那样的 normalization 处理呢? 答案是可以的, 因为大牛们发明了一种技术, 叫做 batch normalization, 正是处理这种情况.

## BN 添加位置

Batch normalization 的 batch 是批数据, 把数据分成小批小批进行 stochastic gradient descent. 而且在每批数据进行前向传递 forward propagation 的时候, 对每一层都进行 normalization 的处理,

![](/img/user/Z-attach/v2-d3ccd01453f215cf3357192debd14489_r.jpg.png)

## BN 效果

Batch normalization 也可以被看做一个层面。 在一层层的添加神经网络的时候, 我们先有数据 X, 再添加全连接层, 全连接层的计算结果会经过 激励函数 成为下一层的输入, 接着重复之前的操作. Batch Normalization (BN) 就被添加在每一个全连接和激励函数之间.

![](/img/user/Z-attach/v2-95f654fdf99999db3fa7dab0bbfbc358_r.jpg.png)

之前说过, 计算结果在进入激励函数前的值很重要, 如果我们不单单看一个值, 我们可以说, 计算结果值的分布对于激励函数很重要. 对于数据值大多分布在这个区间的数据, 才能进行更有效的传递. 对比这两个在激活之前的值的分布. 上者没有进行 normalization, 下者进行了 normalization, 这样当然是下者能够更有效地利用 tanh 进行非线性化的过程.

![](/img/user/Z-attach/v2-b31f7d863179f5f0b93d40c4fabbc31a_r.jpg.png)

没有 normalize 的数据 使用 tanh 激活以后, 激活值大部分都分布到了饱和阶段, 也就是大部分的激活值不是 - 1, 就是 1, 而 normalize 以后, 大部分的激活值在每个分布区间都还有存在. 再将这个激活后的分布传递到下一层神经网络进行后续计算, 每个区间都有分布的这一种对于神经网络就会更加有价值. Batch normalization 不仅仅 normalize 了一下数据, 他还进行了反 normalize 的手续. 为什么要这样呢?

## BN 算法

![](/img/user/Z-attach/v2-083ca0bcd0749fd0f236a690b50442e6_r.jpg.png)

我们引入一些 batch normalization 的公式. 这三步就是我们在刚刚一直说的 normalization 工序, 但是公式的后面还有一个反向操作, 将 normalize 后的数据再扩展和平移. 原来这是为了让神经网络自己去学着使用和修改这个扩展参数 gamma, 和 平移参数 β, 这样神经网络就能自己慢慢琢磨出前面的 normalization 操作到底有没有起到优化的作用, 如果没有起到作用, 我就使用 gamma 和 belt 来抵消一些 normalization 的操作.

![](/img/user/Z-attach/v2-546e36bdd09fff718bbf46143ba5bf75_r.jpg.png)

最后我们来看看一张神经网络训练到最后, 代表了每层输出值的结果的分布图. 这样我们就能一眼看出 Batch normalization 的功效啦. 让每一层的值在有效的范围内传递下去.

如果你对 Batch Normalization 感兴趣, 也想实际动手做做看, 这还有更多使用 python 来编写 Batch Normalization 的教程: [莫烦 Python](https://link.zhihu.com/?target=https%3A//morvanzhou.github.io/tutorials/)