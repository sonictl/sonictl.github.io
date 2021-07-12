---
layout: post
title: 'python标签值标准化到[0-(#class-1)]（重新编码标签）'
date: 2019-04-02 11:50:00 +0800
category: from_cnblogs
slug: p20190402115000
---
python 处理标签常常需要将一组标签映射到一组数字，数字还要求连续。
比如 ['a', 'b', 'c', 'a', 'a', 'b', 'c'] ==(a->0, b->1, c->2)=> [0, 1, 2, 0, 0, 1, 2]。 为了便于本文被搜索，加个关键词：重新编码

可以用`sklearn.preprocessing.LabelEncoder()`这个函数。
### 以数字标签为例：
```
from sklearn import preprocessing
le = preprocessing.LabelEncoder()
le.fit([1,2,2,6,3])
```


#获取标签值#
```
In [2]: le.classes_
Out[2]: array([1, 2, 3, 6])
```



#将标签值标准化#
```
In [3]: le.transform([1,1,3,6,2])
Out[3]: array([0, 0, 2, 3, 1], dtype=int64)
```

#将标准化的标签值反转#
即“反向编码”：
```
In [4]: le.inverse_transform([0, 0, 2, 3, 1])
Out[4]: array([1, 1, 3, 6, 2])
```


### 非数字型标签值标准化：
```
In [5]: from sklearn import preprocessing
   ...: le =preprocessing.LabelEncoder()
   ...: le.fit(["paris", "paris", "tokyo", "amsterdam"])
   ...: print('标签个数:%s'% le.classes_)
   ...: print('标签值标准化:%s' % le.transform(["tokyo", "tokyo", "paris"]))
   ...: print('标准化标签值反转:%s' % le.inverse_transform([2, 2, 1]))
   ...:

标签个数:['amsterdam' 'paris' 'tokyo']
标签值标准化:[2 2 1]
标准化标签值反转:['tokyo' 'tokyo' 'paris']
```