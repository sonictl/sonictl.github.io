---
layout: post
title: 'Notes on Noise Contrastive Estimation and Negative Sampling'
date: 2018-04-04 08:18:00 +0800
category: from_cnblogs
slug: p20180404081800
---
<div style="left: 815.1999999999999px; top: 127.48966666666654px; font-size: 23.91033333333333px; font-family: serif;">Notes on Noise Contrastive Estimation and Negative Sampling</div>
<p>## 生成负样本&nbsp;</p>
<p>在常见的关系抽取应用中，我们经常需要生成负样本来训练一个好的系统。如果没有负样本，系统会趋向于把所有的变量分类成正类。但是，在关系抽取中，并不容易找到足够的高质量的负样本（ground truth）。这种情况下，我们通常需要使用distant supervision来生成负样本。<br /><br />&nbsp; 负样本的生成多少可看成是一种艺术。以下讨论了几种常用的方法，还有些方法没有列出。<br />&nbsp; - random sampling<br />&nbsp; - incompatible relations<br />&nbsp; - domain-specific knowledge<br /><br /></p>
<p>## 随机抽样 Random samples<br />&nbsp; 另一种产生负面证据的方法是在所有变量中随机抽取一小部分(people mention pairs in our spouse example),并将其标记为负面证据。<br />&nbsp; 这可能会产生一些错误的负面例子，但是如果统计变量更有可能是错误的，那么随机抽样就会起作用。<br />&nbsp; 例如，大多数人在句子中提到成对，但他们不是配偶，我们就可以在提及成对的人群中，随机抽取一小部分的，并把它们标记为错误的配偶关系的例子。</p>
<p>&nbsp;</p>
<p>## 不相容关系<br />&nbsp; 不相容关系总是或常常是与我们想要抽取的关系冲突的。比如我们有2个实体，x &amp; y. 我们想抽取A关系，而B是与A不相容关系，我们有：<br />&nbsp; &gt;&gt; B(x,y) =&gt; not A(x,y)<br />&nbsp; 比如，我们要为"spouse"（配偶）关系生成负样本，我们可以使用非配偶关系来作为与之不相容的关系，比如parents, children, or siblings: 如果 x 是 y 的父母，那么x和y不能是夫妻。<br /><br />## 特定领域规则<br />&nbsp; 有时，我们可以利用其他领域特定的知识来生成负样本。这些规则的设计很大程度上依赖于应用场景。例如，对于配偶关系，一个使用时间信息的领域特定规则是&ldquo;不同时活着的人不可能是配偶&rdquo;。Specifically, if a person x has birth_date later than y's death_date, then x and y cannot be spouses.<br /><br /><br />This is the video of Negative Sampling in Natural Language Process Course in Coursea.com:<br />https://www.coursera.org/learn/nlp-sequence-models/lecture/Iwx0e/negative-sampling<br /><br /></p>
<p>&nbsp;</p>
<h2 id="related-paper">Related Papers</h2>
<p>[Noise-Contrastive Estimation of Unnormalized Statistical Models with Applications to Natural Image Statistics]</p>
<p>[Word2vec Parameter Learning Explained]</p>
<p>[Efficient Estimation of Word Representation in Vector Space]</p>
<p>[Distributed Representations of Words and Phrases and their Compositionality]</p>
<p>[Notes on Noise Contrastive Estimation and Negative Sampling]</p>