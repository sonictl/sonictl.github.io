---
layout: post
title: 'Change the environment variable for python code running'
date: 2019-09-26 04:16:00 +0800
category: from_cnblogs
slug: p20190926041600
---
### python程序运行中改变环境变量:
>Trying to change the way the loader works for a running Python is very tricky; probably OS/version dependent; may not work. One work-around that might help in some circumstances is to launch a sub-process that changes the environment parameter using a shell script and then launch a new Python using the shell.

So, before the python code is executed, the env var is loaded. 

Ref: https://stackoverflow.com/questions/1178094/change-current-process-environments-ld-library-path

### But how to launch a sub-process that changes the environment parameter:
You can ref: https://stackoverflow.com/questions/8365394/set-environment-variable-in-python-script?rq=1


## All these env var changing tricks is for frozen the embd gened by Word2Vec each launch:
```python
'''
   one corpus, gen the embd without variance in multiple time.
    run this code by command:
    $ PYTHONHASHSEED=123 python c1_frozEmbd.py
    or by c2_run_c1.py via a subprocess mechanism
   Note:
    Word2Vec(walks, size=16, window=5, min_count=1, workers=1)   # workers = 1 for frozen random seed.
'''

from gensim.models import Word2Vec

corpus_path = 'corpus/my_walks_2.txt'
walks = []

with open(corpus_path, 'r') as cpsReader:
    for line in cpsReader.readlines():
        walks.append(line.strip().split(' '))
cpsReader.close()

# for i in range(20):
for i in range(3):
    w2v_model = Word2Vec(walks, size=16, window=5, min_count=1, workers=1)   # fit the model
    print(w2v_model['17'])
    print(w2v_model.wv.most_similar('17'), '\n')
```