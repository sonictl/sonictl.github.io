---
layout: post
title:  "Getting started with Graph Convolutional Networks as an Amateur"
date: 2021-07-14 07:53:38 +0800
categories: GCN AI
slug: p20210714075338
---

# Getting started with Graph Convolutional Networks as an Amateur

https://zhuanlan.zhihu.com/p/27587371



https://www.cnblogs.com/wangxiaocvpr/p/8306519.html


https://www.cnblogs.com/wangxiaocvpr/p/8299336.html



https://towardsdatascience.com/how-to-do-deep-learning-on-graphs-with-graph-convolutional-networks-7d2250723780



GCN是一种强大的神经网络，旨在直接在图上工作并利用其结构信息。这篇文章是关于如何用图卷积网络（GCN）在图上进行深度学习的系列文章的第一篇。该系列的包含：

1. 本篇：一个高层级的GCN介绍
2. 谱图卷积的半监督学习

## 最基本的代码实现GCN -- 一个高层级的GCN介绍
In this post, I will illustrate how information is propagated through the hidden layers of a GCN using coding examples. 看看GCN是如何从前几层聚合信息的，以及这种机制是如何产生图中节点的有用特征表示的。
[ref link in Chinese中文](https://cloud.tencent.com/developer/article/1400504)

### 看看GCN的输入是啥
Given a graph $G = (V, E)$ , a GCN takes as input:

- adjacency matrix $\mathbf{A}$ of $G$.   *N* × *N* 
- an input feature matrix, $N × F⁰$ feature matrix, $\mathbf{X}$, 
  - where *N* is the number of nodes and
  -  *F⁰* is the number of input features for each node



![image-20210717215412877](/assets/images/image-20210717215412877.png)

图示是一个例子



### 对图中示例的解释：

以一个包含4个节点的图为例，圆括号是每个节点的属性信息。有向边（v,n）表示从v->n的边，此时定义n是节点v的邻居。

GCN的隐藏层记作： $H^{i} = f(H^{i-1}, \mathbf{A})$ 

如果将 $f$ 这个传播规则定义为最简单的  $f(\cdot) = \mathbf{A} \times H^{i-1}$ 

$$
\mathbf{A} = \begin{bmatrix} 0&1&0&0 \\ 0&0&1&1 \\0&1&0&0 \\1&0&1&0 \\ \end{bmatrix}
$$


$$
H^{i-1} = \begin{bmatrix} 0&0 \\ 1&-1 \\ 2&-2 \\ 3&-3 \\  \end{bmatrix}
$$



$$
H^{i} = \mathbf{A} \times H^{i-1} = \begin{bmatrix} 1 & -1 \\ 5 & -5 \\ 1 & -1 \\ 2 & -2 \end{bmatrix}
$$



通过一次传播，得到了节点的新的特征$H^i$ .

如果我们还有节点的度矩阵： $D$ ，利用 $D$ 可以对 $H^i$ 进行归一化： $f(X,A) = D^{-1} \mathbf{A} X$ or  $f(X,A) = D^{-1} \mathbf{A} H^{i}$

此例中，$$D = \begin{bmatrix} 1&0&0&0 \\ 0&2&0&0 \\0&0&2&0 \\0&0&0&1 \\ \end{bmatrix}$$

归一化后，上面的 $H^i$ 变为：$$ H^i = \begin{bmatrix} 1&-1 \\ 2.5&-2.5 \\ 0.5&-0.5 \\ 2&-2 \end{bmatrix} $$



###  几个问题

**问题1：**该表征是相邻节点的特征聚合， 节点的聚合表征不包含它⾃⼰的特征！只有具有⾃环（self-loop）的节点才会在该聚合中包含⾃⼰的特征。在邻接矩阵A中，对角线上为1的节点才是具有自环的节点。

**问题2：**度⼤的节点在其特征表征中将具有较⼤的值，度⼩的节点将具有较⼩的值。会影响随机梯度下降算法，比如梯度爆炸。

#### 解决：

1. 增加自环，在A中加入eye矩阵 $I$：np.eye(A.shape[0])
2. 对表征进行归一化。如：$f(X,A) = D^{-1} \mathbf{A} X$

---

## 2. 整合

这一节要把前面讨论的增加自环、归一化加入，还要加入前面省略的权重和激活函数。

即： 自环 - 归一化 - 权重 - 激活函数

### 2.1 应用权重

一个定义，添加了自环的adj_Matrix：$\hat{\mathbf{A}} = \mathbf{A} + I$ 。 $D$ 是 $\mathbf{A}$ 的度矩阵，定义$\hat{D}$ 是 $\hat{\mathbf{A}}$ 的度矩阵。

权重矩阵： $$W = \begin{bmatrix} 1&-1 \\ -1&1 \end{bmatrix}$$ 

计算此时的属性传播：$\hat{D}^{-1} \times \hat{\mathbf{A}} \times X \times W$

上面的 $H^i$ 即为：$$ H^{i} = \begin{bmatrix} 1  \\ 4 \\ 2 \\ 5 \end{bmatrix} $$

再用ReLu函数套一下：`relu(D_hat**-1 * A_hat * X * W)`  



## 3. 应用于 Karate 网络数据


### 在 Zachary’s Karate Club 网络上进行GCN

![Zachary's Karate Club](https://miro.medium.com/max/1400/1*d62WDGX4uf6bwlu0KyfRsQ.png)

**Zachary’s Karate Club:**
![img](https://pic4.zhimg.com/80/v2-a506ceeb70e6719aecdf5d09c43c49db_720w.jpg)

先随机初始化一把：计算A_hat 和 D_hat 矩阵


```
# 计算A_hat 和 D_hat 矩阵
from networkx import karate_club_graph, to_numpy_matrix
zkc = karate_club_graph()
order = sorted(list(zkc.nodes()))  # order = [0,1,2,...]

A = to_numpy_matrix(zkc, nodelist=order)   # 输入一个nx.graph, this shows 如何生成它的 adj_Matrix
I = np.eye(zkc.number_of_nodes())
A_hat = A + I
D_hat = np.array(np.sum(A_hat, axis=0))[0]
D_hat = np.matrix(np.diag(D_hat))

```



```
# initialize the weights randomly
W_1 = np.random.normal(loc=0, scale=1, size=(zkc.number_of_nodes(), 4))   # W1: shape: (34,4)
W_2 = np.random.normal(loc=0, size=(W_1.shape[1], 2))  # W2 shape: (4,2)

```

**Stack the GCN layers.**

We here use just the identity matrix as feature representation, that is, each node is represented as a one-hot encoded categorical variable.

feature representation will be like:

```
node_0: 1,0,0,...,0
node_1: 0,1,0,...,0
node_2: 0,0,1,0,..,0
...
```



```
# Each transmit is a layer, so defines it as a function
def gcn_layer(A_hat, D_hat, X, W):
    return relu(D_hat**-1 * A_hat * X * W)

# H1 <= A_hat + D_hat + H_0(I) + weight(W_1)
H_1 = gcn_layer(A_hat, D_hat, I, W_1)     # note the I here: I = np.eye(zkc.number_of_nodes())

# H2 <= A_hat + D_hat + H_1 + W_1
H_2 = gcn_layer(A_hat, D_hat, H_1, W_2)

output = H_2
```


```
# We extract the feature representations.
feature_representations = {
    node: np.array(output)[node] 
    for node in zkc.nodes()}

feature_representations
```


```
{0: array([0.27514556, 0.2916475 ]),
 1: array([0.23384102, 0.12333202]),
 2: array([0.10573154, 0.19859599]),
 3: array([0.10044765, 0.15261897]),
 4: array([0.23252655, 0.68635458]),
 5: array([0.41926146, 1.2925271 ]),
 6: array([0.45833935, 1.42640262]),
 7: array([0.06380073, 0.13444717]),
 8: array([0.17890381, 0.37274954]),
 9: array([0.12122856, 0.20078968]),
 10: array([0.23332865, 0.6970111 ]),
 11: array([0.42113657, 0.21505439]),
 12: array([0.17455961, 0.33643646]),
 13: array([0.10565966, 0.20771884]),
 14: array([0.33195037, 0.53988095]),
 15: array([0.49140489, 1.08604872]),
 16: array([0.60948432, 1.93432754]),
 17: array([0.43220848, 0.29479237]),
 18: array([0.28285175, 0.55598298]),
 19: array([0.17600684, 0.31744565]),
 20: array([0.33020733, 0.73801137]),
 21: array([0.36867983, 0.3119505 ]),
 22: array([0.34193455, 0.50004196]),
 23: array([0.46985141, 0.7935709 ]),
 24: array([0.79867382, 1.25776893]),
 25: array([0.81112201, 1.25608014]),
 26: array([0.13385231, 0.19571042]),
 27: array([0.47025238, 0.76962093]),
 28: array([0.1175102 , 0.15650574]),
 29: array([0.23946535, 0.42422421]),
 30: array([0.1985622 , 0.32838823]),
 31: array([0.5660882 , 0.98039276]),
 32: array([0.18968005, 0.30977425]),
 33: array([0.15852075, 0.25247645])}
```


```
# visualize the feature
import matplotlib.pyplot as pltt

def plot_embeddings(M_reduced, word2Ind, words):
    """ 
        Plot in a scatterplot the embeddings of the words specified in the list "words".
        Include a label next to each point.
    """
    for word in words:
        x, y = M_reduced[word2Ind[word]]
        pltt.scatter(x, y, marker='o', color='red')
        pltt.text(x+.006, y+.006, word, fontsize=9)
    pltt.show()

"""
M_reduced_plot_test = np.array([[1, 1], [-1, -1], [1, -1], [-1, 1], [0, 0]])
word2Ind_plot_test = {'test1': 0, 'test2': 1, 'test3': 2, 'test4': 3, 'test5': 4}
words = ['test1', 'test2', 'test3', 'test4', 'test5']
plot_embeddings(M_reduced_plot_test, word2Ind_plot_test, words)

"""

N = len(feature_representations)
M_reduced_plot = np.stack([feature_representations[key_i] for key_i in feature_representations])
word2Ind_plot = {}
words = []
for i in range(N):
    label_i = str(i)
    word2Ind_plot[label_i] = i
    words.append(label_i)

plot_embeddings(M_reduced_plot, word2Ind_plot, words)

    
```

![img](data:image/png;base64,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)

这个结果看起来还是不够直观，常常遇到x坐标为零，或者y坐标为零。 在这个例子中，随机初始化的权重很可能因为ReLU函数而在x轴或y轴上给出0值，所以需要随机初始化几次才能产生上图。

### 总结

在这篇文章中，我对图卷积网络进行了高层次的介绍，并说明了GCN中每一层的节点的特征表示是如何基于其邻域的集合的。我们看到了如何使用numpy构建这些网络，以及它们有多么强大：即使是随机初始化的GCN也能在Zachary的空手道俱乐部中分离出社区。 在下一篇文章中，我将对技术细节进行深入探讨，并展示如何使用半监督学习实现和训练一个最近发表的GCN。你可以在这里找到这个系列的下一篇文章。 喜欢你读的东西吗？可以考虑在Twitter上关注我，除了我自己的文章之外，我还会分享与数据科学和机器学习的实践、理论和伦理学有关的论文、视频和文章。 有关专业咨询，请在LinkedIn上联系我，或在Twitter上直接留言。



参考下一节： [Part 2: Semi-Supervised Learning with Spectral Graph Convolutions](https://towardsdatascience.com/how-to-do-deep-learning-on-graphs-with-graph-convolutional-networks-62acf5b143d0)

