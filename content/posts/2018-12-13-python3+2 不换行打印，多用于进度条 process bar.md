---
layout: post
title: 'python3+2 不换行打印，多用于进度条 process bar'
date: 2018-12-13 13:28:00 +0800
category: from_cnblogs
slug: p20181213132800
---
## 用tqdm包实现进度条：
tqdm基本用法：
```python
from time import sleep
from tqdm import tqdm
for i in tqdm(range(10000)):
     sleep(0.01)

```
tqdm更多用法：https://blog.csdn.net/langb2014/article/details/54798823


## python3 不换行打印，多用于进度条 process bar

```python
    process = 0  # process bar
    for i in user:  
        process += 1
        print("\rProcess: %f " % (process/len(user)), end='')
        process_code()
```

更多样式：
```python
    proc += 1
    print('\r loading... %.2f %%' % (process/len(user)*100), end='')
```

## python2:
对于py2来讲，主要起作用的是末尾的逗号：
```python
    print '\r loading... {:.3f}' .format( (float(proces))/len(user)*100 ) ,
  print('%.3f' % 1.12345678)     # Let's take 3 decimal places
  print'{:.3f}'.format(3.14159)
```