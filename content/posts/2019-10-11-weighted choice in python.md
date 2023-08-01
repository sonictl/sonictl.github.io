---
layout: post
title: 'weighted choice in python'
date: 2019-10-11 03:00:00 +0800
category: from_cnblogs
slug: p20191011030000
---
### 对列表按概率采样

 - Input: a collection C of elements and a probability distribution p over C;
 - Output: an element chosen at random from C according to p.

C有n 个元素，1-n, 概率 (p = (p[1], ..., p[n])。 我们只有random.random()函数，它会给我们均匀分布的[0,1]上的一个float. 基本思想是分割[0,1]into n segments of length p[1] ... p[n] ( ∑ p[i] = 1) . 如果均匀地在[0,1]上打点，那它在第i个segment上停住的概率就是p[i]. 因此可以用random.random()函数来实现。查看停止的地方在[0,1]的哪个位置，然后返回其所在的那个segment index. python如下实现：

ref: https://scaron.info/blog/python-weighted-choice.html
### 对列表按概率采样

```python
import random
import collections

def weighted_choice(seq, weights):
    assert len(weights) == len(seq)
    assert abs(1. - sum(weights)) < 1e-6

    x = random.random()
    for i, elmt in enumerate(seq):
        if x <= weights[i]:
            return elmt
        x -= weights[i]

def gen_weight_list(seq, gt_set, incline_ratio):
    '''
    :param seq:
    :param gt_list:
    :param incline_ratio:
    :return:
    seqe = [1,2,3,4,5]
    gt_list = [3,5,7]
    # incline_ratio = 0.9   # allocate this num of prob for random select gt's in sequence
    '''
    len_seq = len(seq)
    # programmatic gen the prob list:
    prob_list = []
    gts_in_seq = [i for i in seq if i in gt_set]
    len_gts_in_seq = len(gts_in_seq)
    # item_ngt_in_seq = [i for i in seqe if i not in gt_list]
    if len_gts_in_seq > 0:
        prob_gt = incline_ratio/len_gts_in_seq
        prob_ngt = (1-incline_ratio)/(len_seq - len_gts_in_seq)
    else:
        prob_gt = 0
        prob_ngt = 1/len_seq

    for idx in range(len_seq):
        if seq[idx] in gts_in_seq:
            # prob_list[idx] = prob_gt
            prob_list.append(prob_gt)
        else:
            # prob_list[idx] = prob_ngt
            prob_list.append(prob_ngt)

    return prob_list

# add prob incline ratio for allocate heavier weight udr some conditions:
seqe = [1,2,3,4,5]
gt_set = set([3,5,7])    # conditions, if item in seq is also in this list, will be allocated higher weight.
inc_ratio = 0.8   # allocate this num of prob for random select gt's in sequence

prob = gen_weight_list(seqe, gt_set, inc_ratio)
select_seq = []

for i in range(10000):
    select_seq.append(weighted_choice(seqe, prob))

# count the item in select_seq:
select_seq.sort(reverse=True)     #optional?
item_Count = collections.Counter(select_seq)
print(item_Count)
```