---
layout: post
title: 'NEU(Fst Network Embedding Enhancement via High Order Proximity Approximation)'
date: 2018-05-15 08:27:00 +0800
category: from_cnblogs
slug: p20180515082700
---
NEU(Fst Network Embedding Enhancement via High Order Proximity Approximation)

## NEU：通过对高阶相似性的近似，加持快速网络嵌入

## NRL的框架总结
- First, Clarify the notations and formalize the problem of NRL. 
- Then, Introduce the concept of k-order proximity. 
- Finally, Summarize an NRL framework based on proximity matrix factorization and show that the aforementioned NRL methods fall into the category.

定义本文处理的图是无权无向图。这也是他的局限性。<span style="color:red;">这是一个NEU算法的缺点!</span>

对角阵 $D_{ii}=d_i$是$v_i$节点的度。$A=D^{-1} \widetilde A$ ，是对邻接矩阵$\widetilde A$的归一化结果。
Laplacian Matrix: $\widetilde L = D - \widetilde A$, 这是把$\widetilde A$全取反再在对角线上加上$v_i$的度数。
Normalized Laplacian Matrix: $ L = D^{-\frac{1}{2}}\widetilde L D^{-\frac{1}{2}} $ 
> 这俩Laplacian matrix 拿来何用？

### K-order proximity
$ A$和$\widetilde L$ characterize 一阶相似性，建模局部节点对的proximity。
还是沿用GraRep的K-step转移概率矩阵：transition probability matrix 作为**k-order proximity matrix**.
$A^k = \underbrace{A \cdot A ... A}_{k}$

### NRL Framework
**Step1: Proximity Matrix Construction** 相似性矩阵建立
相似性矩阵$M \in \mathbb R^{|V|\times |V|}$编码了 $k$ 阶相似性，$k = 1,2,...,K$ .有$A$是normalized邻接矩阵， $M=\frac{A+A^2+...+A^K}{K}$表示了K阶相似性矩阵的联合再平均。$M$通常是由$A$的$K$级的多项式表示，文章记为$f(A) \in \mathbb R^{|V|\times |V|}$, $K$级是多少，depends on 相似度矩阵proximity matrix要表达的最大的proximity阶数。

**Step2: Dimension Reduction** 维数约减
寻找2个矩阵，$R$ 和 $C$. 
- $R \in \mathbb R^{|V|\times d}$ 是节点的低维向量表达, 
- $C \in \mathbb R^{|V|\times d}$是context角色时，节点的低维向量表达。

矩阵的乘积$R \cdot C^T$就是对原网络的相似性矩阵$M$的近似。这里，不同的算法对$R \cdot C^T$和$M$的距离有不同的描述，employ different distance function. 比如，用$M- R \cdot C^T$ 

**前人的方法与本框架的关系**
Spectral Clustering: 
DeepWalk: 
GraRep: 
TADW: 
LINE: 

### 观察和Problem Formalization
既然是2步框架，第一步是建立proximity matrix，怎么建立一个好的proximity matrix for NRL.在这篇文章里讨论。
至于第二步，维数约减，future Work.

**Observation 1**: 更高阶的，和更精确的proximity matrix可以提升模型的学习效果。也就是说，如果探索一个更高阶的polynomial proximity matrix $f(A)$，NRL可以因此受益。

**Observation 2**：对大规模网络来说，对高阶的proximity matrix的精确计算是不可行的。实际上对proximity matrix的计算takes $O(|V|^2)$ time. SVD的时间复杂度也随k 的增大，get dense，从而增加。

其实Observation1&2是矛盾的，前者要更精确，更高阶。后者又表明越高阶越难算。
因此如何高效地获得高阶的proximity matrix变为一个问题。
文章的解决方案是，先对低阶的proximity matrix的信息进行编码，以此作为一个基础，来避免重复的计算。



**问题的构建**：
有个假设，$R$和$C$是某个NRL算法学到的表达，$R \cdot C^T$ 对$K$阶的多项式proximity matrix $f(A)$ 构成近似。目的就是学到一个更好的$R'$和$C'$，它俩可以构成对$g(A)$的近似，这个$g(A)$比$f(A)$更高阶。并且，算法还要高效，should be efficient in the linear time of $|V|$. 注意，时间复杂度下界是$O(|V|d)$ ，which is the size of embedding matrix $R$.