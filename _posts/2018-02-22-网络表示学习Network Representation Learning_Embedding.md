---
layout: post
title: '网络表示学习Network Representation Learning/Embedding'
date: 2018-02-22 13:47:00 +0800
category: from_cnblogs
slug: p20180222134700
---
<p>&nbsp;</p>
<h2>网络表示学习相关资料</h2>
<p>网络表示学习（network representation learning，NRL）,也被称为图嵌入方法（graph embedding method，GEM）是这两年兴起的工作，目前很热，许多直接研究网络表示学习的工作和同时优化网络表示+下游任务的工作正在进行中。</p>
<ol><ol>
<li>
<p>清华大学计算机系的一个学习组 新浪微博@涂存超 整理的论文列表：<a href="https://github.com/thunlp/NRLpapers" target="_blank">https://github.com/thunlp/NRLpapers</a>，并一直持续更新着，里面详细的列举了最近几年有关网络表示学习（network representation learning/network embedding）比较有代表性的论文列表及其代码。</p>
</li>
<li>
<p><span style="color: #ff0000;">☆</span>陈启明整理的github列表&nbsp; <a href="https://github.com/chihming/awesome-network-embedding" target="_blank">https://github.com/chihming/awesome-network-embedding</a></p>
</li>
<li>
<p>一篇综述性文章（University of Southern California (USC)）及其code： <br />
（1）文章： Goyal P, Ferrara E. Graph Embedding Techniques, Applications, and 
Performance: A Survey[J]. arXiv preprint arXiv:1705.02801, 2017. <br />
（2）代码： <a href="https://github.com/palash1992/GEM" target="_blank">https://github.com/palash1992/GEM</a></p>





</li>
<li>
<p>一篇博客： <br />
<a href="http://blog.csdn.net/Dark_Scope/article/details/74279582#0-tsina-1-3919-397232819ff9a47a7b7e80a40613cfe1" target="_blank">http://blog.csdn.net/Dark_Scope/article/details/74279582#0-tsina-1-3919-397232819ff9a47a7b7e80a40613cfe1</a></p>





</li>
<li>
<p>一个github资料，里面有部分论文+code（大多数是python实现，matlab次之）： <br />
</p>





</li>
<li>
<p>四个slides：</p>
<ol>
<li>
<p>&nbsp;[(MLSS2017)网络表示学习]《Representation Learning with Networks》by Jure Leskovec [Stanford University] Part1:<a href="http://i.stanford.edu/~jure/pub/talks2/leskovec-networks-01-nodes.pdf" target="_blank">网页链接</a> Part2:<a href="http://i.stanford.edu/~jure/pub/talks2/leskovec-networks-02-communities.pdf" target="_blank">网页链接</a> Part3:<a href="http://i.stanford.edu/~jure/pub/talks2/leskovec-networks-03-time.pdf" target="_blank">网页链接</a> Part4:<a href="http://i.stanford.edu/~jure/pub/talks2/leskovec-networks-04-ML_for_decision_making.pdf" target="_blank">网页链接</a> ​​​​</p>
</li>
<li>&nbsp; <a href="https://pan.baidu.com/s/1nuB5Rex" target="_blank">https://pan.baidu.com/s/1nuB5Rex</a></li>
<li>&nbsp; <a href="https://pan.baidu.com/s/1geUHeQB" target="_blank">https://pan.baidu.com/s/1geUHeQB</a></li>
<li>&nbsp; <a href="https://pan.baidu.com/s/1cwB7pc" target="_blank">https://pan.baidu.com/s/1cwB7pc</a></li>
</ol></li>
</ol></ol>
<p>&nbsp;</p>
<h2 class="csdn_top">网络表示学习（DeepWalk，LINE，node2vec，SDNE）</h2>
<div class="article_bar clearfix">
<div class="artical_tag"><span class="original"> 原创 <span class="time">2017年07月24日 12:49:01 </span></span></div>




</div>
<div class="article_bar clearfix">&nbsp;</div>
<p><a name="t0"></a><strong><span style="font-family: Times New Roman; font-size: 24px;"><span lang="en-US">详细的资料可以参考：<a style="text-decoration: none; color: #000000; font-family: 'microsoft yahei'; font-size: 18px; font-weight: bold;" href="http://blog.csdn.net/u013527419/article/details/74853633" target="_blank">网络表示学习相关资料</a></span></span></strong></p>
<h3><a name="t1"></a><strong><span style="font-family: Times New Roman; font-size: 24px;"><span lang="en-US"><br />
</span></span></strong></h3>
<h3><a name="t2"></a><strong><span style="font-family: Times New Roman; font-size: 24px;"><span lang="en-US">1.<span lang="zh-CN">传统：基于图的表示（又称为基于符号的表示）</span></span></span></strong></h3>
<p><span lang="zh-CN"><span style="font-family: Times New Roman;"><strong><img src="http://img.blog.csdn.net/20170724123001202?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMzUyNzQxOQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" alt="" width="150" height="150" align="left" /></strong></span></span></p>
<p style="margin: 0in;"><span style="font-size: 18px;"><span lang="zh-CN"><br />
</span></span></p>
<p style="margin: 0in;"><span style="font-size: 18px;"><span lang="zh-CN">如左图<span lang="en-US"><span lang="en-US">G =<span lang="zh-CN">（<span lang="en-US">V<span lang="zh-CN">，<span lang="en-US">E<span lang="zh-CN">），用不同的符号命名不同的节点，用二维数组（邻接矩阵）的存储结构表示两节点间是否存在连边，存在为<span lang="en-US">1<span lang="zh-CN">，否则为<span lang="en-US">0<span lang="zh-CN">。</span></span></span></span></span></span></span></span></span></span></span></span></span></p>
<p style="margin: 0in;"><span style="font-size: 18px;">&nbsp;</span></p>
<p style="margin: 0in;"><span style="font-size: 18px;">缺点：长尾分布下大部分节点间没有关系，所以邻接矩阵非常稀疏，不利于存储计算。</span></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<h3 style="margin: 0in;"><a name="t3"></a><span style="font-family: Times New Roman;"><span style="font-size: 14pt;" lang="en-US"><strong><br />
</strong></span></span></h3>
<p style="margin: 0in;"><span style="font-family: SimSun; font-size: 24px;"><span lang="en-US">2.&nbsp;<span lang="zh-CN">网络表示学习（<span lang="en-US">Network Representation Learning<span lang="zh-CN">，<span lang="en-US">NRL<span lang="zh-CN">），也称为图嵌入法（<span lang="en-US">G<span lang="en-US">raph
 Embedding Method<span lang="zh-CN">，<span lang="en-US">GEM<span lang="zh-CN">）：<span style="font-family: 'Times New Roman'; font-weight: normal;" lang="zh-CN"><span style="font-size: 18px;">用低维、稠密、实值的向量表示网络中的节点（含有语义关系，利于计算存储，不用再手动提特征（自适应性），且可以将异质信息投影到同一个低维空间中方便进行下游计算）。</span></span></span></span></span></span></span></span></span></span></span></span></span></span></p>
<p style="margin: 0in;"><span style="font-family: Times New Roman; font-size: 18px;">&nbsp;</span></p>
<p style="margin: 0in;"><span style="font-family: Times New Roman; font-size: 24px;"><strong><span style="color: #002060;" lang="en-US">DeepWalk<span style="color: #002060;" lang="zh-CN">【<span style="color: #002060;" lang="en-US">1<span style="color: #002060;" lang="zh-CN">】<span lang="zh-CN">：</span></span></span></span></span></strong></span></p>
<p style="margin: 0in;"><span style="font-family: 'Times New Roman';"><span lang="zh-CN"><span style="font-size: 18px;">实现1：<a href="https://github.com/phanein/deepwalk" target="_blank">https://github.com/phanein/deepwalk</a></span></span></span></p>
<p style="margin: 0in;">&nbsp;</p>
<p><span style="font-size: 18px;"><span lang="en-US">&nbsp; <span lang="zh-CN">用<span lang="en-US">SkipGram<span lang="en-US">的方法<span lang="zh-CN">进行网络中节点的表示学习<span lang="en-US">。那么，根据<span lang="en-US">SkipGram<span lang="en-US">的思路，最重要的就是定义<span lang="en-US">Context<span lang="en-US">，<span lang="zh-CN">也就<span lang="en-US">是<span lang="en-US">Neighborhood<span lang="en-US">。​<span lang="en-US">NLP<span lang="zh-CN">中，<span lang="en-US">Neighborhood<span lang="en-US">是当前<span lang="en-US">Word<span lang="en-US">周围的字，<span lang="zh-CN">本文用随机游走得到<span lang="en-US">Graph<span lang="en-US">或者<span lang="en-US">Network<span lang="zh-CN">中节点的<span lang="en-US">Neighborhood<span lang="zh-CN">。</span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></p>
<ul>
<li><span style="font-size: 18px;"><span lang="zh-CN">（<span lang="en-US">1<span lang="zh-CN">）随机游走随机均匀地选取网络节点，并生成固定长度的随机游走序列，将此序列类比为自然语言中的句子（节点序列<span lang="en-US">=<span lang="zh-CN">句子，序列中的节点<span lang="en-US">=<span lang="zh-CN">句子中的单词），应用<span lang="en-US">skip-gram<span lang="zh-CN">模型学习节点的分布式表示，<span lang="en-US">skip-gram<span lang="zh-CN">模型详见：<a href="http://blog.csdn.net/u013527419/article/details/74129996" target="_blank">http://blog.csdn.net/u013527419/article/details/74129996</a></span></span></span></span></span></span></span></span></span></span></span></span></li>
<li><span style="font-size: 18px;"><span lang="zh-CN">（<span lang="en-US">2<span lang="zh-CN">）前提：如果一个网络的节点服从幂律分布，那么节点在随机游走序列中的出现次数也服从幂律分布，并且实证发现<span lang="en-US">NLP<span lang="zh-CN">中单词的出现频率也服从幂律分布。</span></span></span></span></span></span></li>





</ul>
<p>


&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;<img src="http://img.blog.csdn.net/20170724123438004?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMzUyNzQxOQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" alt="" width="400" height="200" /></p>
<p style="margin: 0in;">&nbsp;</p>
<ul>
<li><span style="font-size: 18px;"><span lang="zh-CN">（<span lang="en-US">3<span lang="zh-CN">）大体步骤：</span></span></span></span></li>





</ul>
<p style="margin: 0in;"><span style="font-size: 18px;"><span lang="en-US">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; Network/graph&nbsp;---------random walk&nbsp;---------<span lang="zh-CN">得到节点序列（<span lang="en-US">representation mapping<span lang="zh-CN">）<span lang="en-US">--------&nbsp;<span lang="zh-CN">放到<span lang="en-US">skip-gram<span lang="zh-CN">模型中（中间节点预测上下
 &nbsp; &nbsp; &nbsp; &nbsp;文节点）<span lang="en-US">--------- output<span lang="zh-CN">：<span lang="en-US">representation</span></span></span></span></span></span></span></span></span></span></span></span></p>
<p style="margin: 0in; font-size: 12pt;" lang="en-US">&nbsp;</p>
<p style="margin: 0in; margin-left: .75in;"><img src="http://img.blog.csdn.net/20170724123714819?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMzUyNzQxOQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" alt="计算机生成了可选文字:网 络 随 机 游 走 t-ti 1 1 与 + 1 + 2 〕 Skip-Gram " width="606" height="207" /></p>
<p style="margin: 0in;"><span style="font-size: 14pt; color: #002060;" lang="en-US"><strong>LINE</strong><span style="font-size: 14pt; color: #002060;" lang="zh-CN"><strong>【</strong><span style="font-size: 14pt; color: #002060;" lang="en-US"><strong>2</strong><span style="font-size: 14pt; color: #002060;" lang="zh-CN"><strong>】</strong><span style="font-size: 12pt;" lang="zh-CN">：</span></span></span></span></span></p>
<p style="margin: 0in; margin-left: 1.5in;"><img src="http://img.blog.csdn.net/20170724123949044?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMzUyNzQxOQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" alt="计算机生成了可选文字:10 " width="400" height="275" /></p>
<p style="margin: 0in;"><span style="font-size: 18px;"><span lang="zh-CN">（<span lang="en-US">1<span lang="zh-CN">）先区分两个概念：</span></span></span></span></p>
<p style="margin: 0in;"><span style="font-size: 18px;"><span lang="zh-CN"><strong>一阶相似度：</strong><span lang="zh-CN">直接相连节点间，例如<span lang="en-US">6<span lang="zh-CN">与<span lang="en-US">7<span lang="zh-CN">。</span></span></span></span></span></span></span></p>
<p style="margin: 0in;"><span style="font-size: 18px;"><span lang="zh-CN">定义节点<span lang="en-US">vi<span lang="zh-CN">和<span lang="en-US">vj<span lang="zh-CN">间的联合概率为</span></span></span></span></span></span></p>
<p style="margin: 0in; margin-left: .75in;"><span style="font-size: 18px;"><img src="http://img.blog.csdn.net/20170724124004291?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMzUyNzQxOQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" alt="计算机生成了可选文字:此@，， ， 与 1 + exp(&mdash;&uuml;T &middot; ） " width="453" height="85" /></span></p>
<p style="margin: 0in;"><span style="font-size: 18px;"><span lang="en-US">v<span lang="zh-CN">代表节点，<span lang="en-US">u<span lang="zh-CN">代表节点的<span lang="en-US">embedding<span lang="zh-CN">。上面式子的意思是两节点越相似，內积越大，<span lang="en-US">sigmoid<span lang="zh-CN">映射后的值越大，也就是这两节点相连的权重越大，也就是这两个节点间出现的概率越大？？？。</span></span></span></span></span></span></span></span></span></p>
<p style="margin: 0in;"><span style="font-size: 18px;">&nbsp;</span></p>
<p style="margin: 0in;"><span style="font-size: 18px;"><span lang="zh-CN"><strong>二阶相似度：</strong><span lang="zh-CN">通过其他中介节点相连的节点间例如<span lang="en-US">5<span lang="zh-CN">与<span lang="en-US">6<span lang="zh-CN">。</span></span></span></span></span></span></span></p>
<p style="margin: 0in;"><span style="font-size: 18px;">用的是一个条件概率</span></p>
<p style="margin: 0in;"><span style="font-size: 18px;">&nbsp;</span></p>
<p style="margin: 0in; margin-left: .75in;"><span style="font-size: 18px;"><img src="http://img.blog.csdn.net/20170724124021957?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMzUyNzQxOQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" alt="计算机生成了可选文字:exp(uy 乥 〕 k ： 1 exp （ 卩 ' ． 豇 ， ） " width="356" height="95" /></span></p>
<p style="margin: 0in;"><span style="font-size: 18px;"><span lang="zh-CN">（<span lang="en-US">2<span lang="zh-CN">）目标是让<span lang="en-US">NRL<span lang="zh-CN">前后节点间相似度不变，也节点表示学习前如果两个节点比较相似，那么<span lang="en-US">embedding<span lang="zh-CN">后的这两个节点表示向量也要很相似。<span lang="en-US">--<span lang="zh-CN">此文中用的是<span lang="en-US">KL<span lang="zh-CN">散度，度量两个概率分布之间的距离。<span lang="en-US">KL<span lang="zh-CN">散度的相关知识详见：<a href="http://blog.csdn.net/u013527419/article/details/51776786" target="_blank">http://blog.csdn.net/u013527419/article/details/51776786</a></span></span></span></span></span></span></span></span></span></span></span></span></span></span></p>
<p style="margin: 0in;"><span style="font-size: 18px;">以保证其一阶相似度为例子：</span></p>
<p style="margin: 0in;"><span style="font-size: 18px;"><span lang="en-US">embedding<span lang="zh-CN">前：<span lang="zh-CN">节点<span lang="en-US">vi<span lang="zh-CN">和<span lang="en-US">vj<span lang="zh-CN">间的经验联合概率为</span></span></span></span></span></span></span></span></p>
<p style="margin: 0in; margin-left: 2.25in;"><span style="font-size: 18px;"><img src="http://img.blog.csdn.net/20170724124032815?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMzUyNzQxOQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" alt="计算机生成了可选文字:= ． 证 " width="152" height="56" /></span></p>
<p style="margin: 0in;"><span style="font-size: 18px;">所以，最小化：</span></p>
<p style="margin: 0in; margin-left: 1.5in;"><span style="font-size: 18px;"><img src="http://img.blog.csdn.net/20170724124043610?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMzUyNzQxOQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" alt="计算机生成了可选文字:01 乥 〕 丿 log 夕1@， ， ） 力 e " width="341" height="68" /></span></p>
<p style="margin: 0in;"><span style="font-size: 14pt; color: #002060;" lang="en-US"><strong>Node2vec</strong><span style="font-size: 14pt; color: #002060;" lang="zh-CN"><strong>【</strong><span style="font-size: 14pt; color: #002060;" lang="en-US"><strong>3</strong><span style="font-size: 14pt; color: #002060;" lang="zh-CN"><strong>】</strong><span style="font-size: 12pt;" lang="zh-CN">：</span></span></span></span></span></p>
<p style="margin: 0in;"><span style="font-size: 12pt;" lang="zh-CN">论文+实现及其他：<a href="http://snap.stanford.edu/node2vec/" target="_blank">http://snap.stanford.edu/node2vec/</a></span></p>
<p style="margin: 0in;"><span lang="zh-CN">类似于<span lang="en-US">deepwalk<span lang="zh-CN">，主要的创新点在于改进了随机游走的策略，定义了两个参数<span lang="en-US">p<span lang="zh-CN">和<span lang="en-US">q<span lang="zh-CN">，在<span lang="en-US">BFS<span lang="zh-CN">和<span lang="en-US">DFS<span lang="zh-CN">中达到一个平衡，同时考虑到局部和宏观的信息，并且具有很高的适应性。</span></span></span></span></span></span></span></span></span></span></span></p>
<p style="margin: 0in;"><span lang="zh-CN">（<span lang="en-US">1<span lang="zh-CN">）</span></span></span></p>
<p style="margin: 0in; margin-left: .375in;"><img src="http://img.blog.csdn.net/20170724124300672?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMzUyNzQxOQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" alt="计算机生成了可选文字:" width="553" height="163" /></p>
<p style="margin: 0in;"><span lang="zh-CN">（<span lang="en-US">2<span lang="zh-CN">）参数控制跳转概率的随机游走，之前完全随机时，<span lang="en-US">p=q=1.</span></span></span></span></p>
<p style="margin: 0in;"><span lang="en-US">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; --<span lang="zh-CN">返回概率参数（<span lang="zh-CN">Return parameter<span lang="zh-CN">）<span lang="en-US">p<span lang="zh-CN">，对应<span lang="en-US">BFS<span lang="zh-CN">，<span lang="en-US">p<span lang="zh-CN">控制回到原来节点的概率，如图中从<span lang="en-US">t<span lang="zh-CN">跳到<span lang="en-US">v<span lang="zh-CN">以后，有<span lang="en-US">1/p<span lang="zh-CN">的概率在节点<span lang="en-US">v<span lang="zh-CN">处再跳回到<span lang="en-US">t<span lang="zh-CN">。</span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></p>
<p style="margin: 0in;"><span lang="en-US">&nbsp;&nbsp;&nbsp;&nbsp; --<span lang="zh-CN">离开概率参数（<span lang="en-US">In out<span lang="zh-CN">parameter<span lang="zh-CN">）<span lang="en-US">q<span lang="zh-CN">，对应<span lang="en-US">DFS<span lang="zh-CN">，<span lang="en-US">q<span lang="zh-CN">控制跳到其他节点的概率。</span></span></span></span></span></span></span></span></span></span></span></p>
<p style="margin: 0in; margin-left: .75in;"><img src="http://img.blog.csdn.net/20170724124337423?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMzUyNzQxOQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" alt="计算机生成了可选文字:0 = 1 / q 0 = 1 / q -I/p 1 if if dtT if " width="541" height="203" /></p>
<p style="margin: 0in;"><span lang="zh-CN">上图中，刚从<span lang="en-US">edge 
<span lang="zh-CN">（<span lang="en-US">t<span lang="zh-CN">，<span lang="en-US">v<span lang="zh-CN">）过来，现在在节点<span lang="en-US">v<span lang="zh-CN">上，要决定下一步（<span lang="en-US">v<span lang="zh-CN">，<span lang="en-US">x<span lang="zh-CN">）怎么走。其中<span lang="en-US">dtx<span lang="en-US">表示节点<span lang="en-US">t<span lang="en-US">到节点<span lang="en-US">x<span lang="en-US">之间的最短路径，<span lang="en-US">dtx=0<span lang="en-US">表示会回到节点<span lang="en-US">t<span lang="en-US">本身，<span lang="en-US">dtx=1<span lang="en-US">表示节点<span lang="en-US">t<span lang="en-US">和节点<span lang="en-US">x<span lang="en-US">直接相连，但是在上一步却选择了节点<span lang="en-US">v<span lang="en-US">，<span lang="en-US">dtx=2<span lang="en-US">表示节点<span lang="en-US">t<span lang="en-US">不与<span lang="en-US">x<span lang="en-US">直接相连，但节点<span lang="en-US">v<span lang="en-US">与<span lang="en-US">x<span lang="en-US">直接相连。</span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></p>
<p style="margin: 0in;"><span lang="zh-CN">（<span lang="en-US">3<span lang="zh-CN">）在计算广告、推荐领域中，围绕着<span lang="en-US">node2nec<span lang="zh-CN">有俩很有意思的应用：</span></span></span></span></span></p>
<p style="margin: 0in;"><span lang="en-US">&nbsp; &nbsp; &nbsp;Facebook<span lang="zh-CN">：<span lang="zh-CN"><a href="http://geek.csdn.net/news/detail/200138" target="_blank">http://geek.csdn.net/news/detail/200138</a></span></span></span></p>
<p style="margin: 0in;"><span lang="en-US">&nbsp; &nbsp; &nbsp;Tencent<span lang="zh-CN">：<a href="http://www.sohu.com/a/124091440_355140" target="_blank"><span lang="en-US">h<span lang="zh-CN">ttp://www.sohu.com/a/124091440_355140</span></span></a></span></span></p>
<p style="margin: 0in;"><span style="font-size: 12pt;">&nbsp;</span></p>
<p style="font-size: 12pt; margin: 0in;" lang="en-US"><span style="font-size: 14pt; color: #002060;"><strong>SDNE[4]</strong><span style="font-size: 14pt; color: #002060;"><strong>:</strong><span style="font-size: 12pt;">:</span></span></span></p>
<p style="margin: 0in;"><span style="font-size: 12pt;" lang="en-US">&nbsp; <span lang="zh-CN">本文的一大贡献在于提出了一种新的半监督学习模型<span lang="en-US">,<span lang="en-US">结合一阶估计与二阶估计的优点<span lang="en-US">,<span lang="en-US">用于表示网络的全局结构属性和局部结构属性。</span></span></span></span></span></span></p>
<p style="margin: 0in;"><img src="http://img.blog.csdn.net/20170724124522031?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMzUyNzQxOQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" alt="计算机生成了可选文字:10 pr 严 { LO structure 0 ； stwing № b structure 2 ： The frame 、 york the semi 、 upervi 、 ed deep model Of 、 D 、 丆 " width="642" height="475" /></p>
<p style="margin: 0in;"><span lang="zh-CN">对节点的描述特征向量（比如点的「邻接向量」）使用<span lang="en-US">autoencoder编码，取autoencoder中间层作为向量表示，以此来让获得2ndproximity（相似邻居的点相似度较高，因为两个节点的「邻接向量」相似，说明它们共享了很多邻居，最后映射成的向量y也会更接近）。<span lang="zh-CN">总觉得上面图中<span lang="en-US">local<span lang="zh-CN">和<span lang="en-US">global<span lang="zh-CN">写反了。</span></span></span></span></span></span></span></p>
<p style="margin: 0in;">目标函数：</p>
<p style="margin: 0in;"><img src="http://img.blog.csdn.net/20170724124532747?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMzUyNzQxOQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" alt="" width="556" height="105" /></p>
<p style="margin: 0in;">&nbsp;</p>
<p style="margin: 0in;"><span lang="zh-CN">【<span lang="en-US">1<span lang="zh-CN">】<span lang="zh-CN">Perozzi B, Al-Rfou R, Skiena S.Deepwalk: Online learning of social representations[C]<span lang="zh-CN">，<span lang="en-US">KDD<span lang="zh-CN">2014:
 701-710.</span></span></span></span></span></span></span></p>
<p style="margin: 0in;"><span lang="zh-CN">【<span lang="en-US">2<span lang="zh-CN">】<span lang="zh-CN">LINE<span lang="zh-CN">：<span lang="zh-CN">Large-scaleInformation Network Embedding<span lang="zh-CN">。<span lang="en-US">WWW<span style="color: #333333;" lang="zh-CN">2015<span style="color: #333333;" lang="zh-CN">，<span style="color: #333333;" lang="zh-CN">JianTang,
 Meng Qu , Mingzhe Wang, Ming Zhang, Jun Yan, Qiaozhu Mei<span style="color: #333333;" lang="zh-CN">，<span style="color: #333333;" lang="zh-CN">MicrosoftResearch Asia;Peking University,China;University of Michigan<span style="color: #333333;" lang="zh-CN">。</span></span></span></span></span></span></span></span></span></span></span></span></span></span></p>
<p style="margin: 0in;"><span style="color: #333333;" lang="zh-CN">【<span style="color: #333333;" lang="en-US">3<span style="color: #333333;" lang="zh-CN">】<span lang="zh-CN">node2vec: Scalable Feature Learning forNetworks<span lang="zh-CN">，<span lang="zh-CN">A
 Grover, J Leskovec [StanfordUniversity] (KDD2016)</span></span></span></span></span></span></p>
<p style="margin: 0in;"><span lang="zh-CN">【<span lang="en-US">4<span lang="zh-CN">】Structural Deep Network Embedding，<span lang="en-US">KDD 2016</span></span></span></span></p>
<p style="margin: 0in;" lang="en-US">&nbsp;</p>
<p style="margin: 0in;">上面都是我比较感兴趣一点的，详细的可以参考：https://github.com/thunlp/NRLpapers</p>
<p style="margin: 0in;">&nbsp;</p>
<hr />
<p style="margin: 0in;">&nbsp;</p>
<p><strong>转载自</strong>：蓁蓁尔的博客</p>
<p style="margin: 0in;">&nbsp; <strong>网络表示学习相关资料</strong>&nbsp; <a href="http://blog.csdn.net/u013527419/article/details/74853633">http://blog.csdn.net/u013527419/article/details/74853633</a><br />&nbsp; <strong>网络表示学习</strong>（DeepWalk，LINE，node2vec，SDNE）&nbsp; <a href="%20http://blog.csdn.net/u013527419/article/details/76017528">http://blog.csdn.net/u013527419/article/details/76017528</a></p>
<p style="margin: 0in;">&nbsp;</p>
<p style="margin: 0in;">&nbsp;</p>
<p style="margin: 0in;">&nbsp;</p>
<p style="margin: 0in;">&nbsp;</p>
<hr />
<p style="margin: 0in;">&nbsp;</p>