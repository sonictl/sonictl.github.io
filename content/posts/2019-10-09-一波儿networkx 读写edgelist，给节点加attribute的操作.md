---
layout: post
title: '一波儿networkx 读写edgelist，给节点加attribute的操作'
date: 2019-10-09 11:24:00 +0800
category: networkx
slug: p20191009112400
---
# 一波儿networkx 读写edgelist，给节点加attribute的操作
read more: [nx official: Reading and writing graphs](https://networkx.github.io/documentation/stable/reference/readwrite/index.html)
```python
import numpy as np
import networkx as nx
import operator

G1 = nx.DiGraph(name='network1')  # Directed Graph
#networkx 导入edgelist
with open(net1_path, 'r') as edgeReader:   # 从文件中读取edgelist生成Graph of networkx
    for line in edgeReader.readlines():
        edges_net1.append(tuple(map(int, line.strip().split(' '))))
G1.add_edges_from(edges_net1)
print('\n== info of net1 original: ')
print(nx.info(G1))
triad_list_net1 = []
for i in edges_net1:
  triad = tuple([i[0], i[1], i[0]+i[1]])
  triad_list_net1.append(triad)   # list of triads
  
triad_list_net1.sort(key=operator.itemgetter(2))   # sort the list by the 3rd item of triad
isgt_dict1 = dict(zip(G1.nodes, np.zeros(len(G1.nodes), int)))  # gen the dic for attribute of nodes in G1
nx.set_node_attributes(G1, isgt_dict1, 'Attr_name')

#networkx 导出网络为edgelist
nx.write_edgelist(G1, 'data_G1.txt', delimiter=' ', data=False)  # without properties by 'data=False'
#         将nodes带属性写入文件：
f = open('data_G1_nodes.txt', 'w')
for node_i in G1.nodes:
  f.write(str(node_i)+','+str(G1.nodes[node_i]['Attr_name'])+'\n')


```

2. Use `networkx.read_edgelist()` function: 快速从文件中读取边并创建图
```python
netPath = 'myEdgelist.txt'
G = nx.read_edgelist(netPath)
```
read more: [nx official: Reading and writing graphs](https://networkx.github.io/documentation/stable/reference/readwrite/index.html)