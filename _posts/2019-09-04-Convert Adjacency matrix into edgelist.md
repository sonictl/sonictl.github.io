---
layout: post
title: 'Convert Adjacency matrix into edgelist'
date: 2019-09-04 12:24:00 +0800
category: from_cnblogs
slug: p20190904122400
---
# Table of Contents
1. [Edge List <-- Adjacency Matrix](# Edge List <-- Adjacency Matrix)
2. [Edge List --> Adjacency Matrix](# Edge List --> Adjacency Matrix)
3. [About Adjacency List](#About Adjacency List)

--------

##  Edge List <-- Adjacency Matrix



```python
'''
ref: https://www.cnblogs.com/sonictl/p/10688533.html 
convert adjMatrix into edgelist: 'data/unweighted_edgelist.number' or 'data/weighted_edgelist.number''

input: adjacency matrix with delimiter=', '
  it can process:
  - Unweighted directed graph
  - Weighted directed graph

output: edgelist (unweighted and weighted)


'''

import numpy as np
import networkx as nx

# -------DIRECTED Graph, Unweighted-----------
# Unweighted directed graph:
a = np.loadtxt('data/test_adjMatrix.txt', delimiter=', ', dtype=int)
D = nx.DiGraph(a)
nx.write_edgelist(D, 'data/unweighted_edgelist.number', data=False)  # output

edges = [(u, v) for (u, v) in D.edges()]
print(edges)

# -------DIRECTED Graph, Weighted------------
# Weighted directed graph (weighted adj_matrix):
a = np.loadtxt('data/adjmatrix_weight_sample.txt', delimiter=', ', dtype=float)
D = nx.DiGraph(a)
nx.write_weighted_edgelist(D, 'data/weighted_edgelist.number')  # write the weighted edgelist into file

# print(D.edges)
elarge = [(u, v, d['weight']) for (u, v, d) in D.edges(data=True) if d['weight'] > 0.]
print(elarge)    # class: list

# -------UNDIRECTED Graph -------------------
# for undirected graph, simply use:
udrtG = D.to_undirected()

'''
test_adjMatrix.txt:  (Symmetric matrices if unweighted graph)
---
0, 1, 1, 1, 0, 1, 1, 0
0, 0, 1, 0, 0, 0, 1, 1
0, 0, 0, 1, 1, 0, 0, 0
0, 1, 0, 0, 1, 1, 0, 0
0, 0, 0, 0, 0, 0, 0, 0
0, 0, 0, 0, 0, 0, 0, 0
0, 0, 0, 0, 0, 0, 0, 0
0, 0, 1, 0, 0, 0, 0, 0
===

adjmatrix_weight_sample.txt：
---
0, 0.5, 0.5, 0.5, 0, 0.5, 0.5, 0
0, 0, 0.5, 0, 0, 0, 0.5, 0.5
0, 0, 0, 0.5, 0.5, 0, 0, 0
0, 0.5, 0, 0, 0.5, 0.5, 0, 0
0, 0, 0, 0, 0, 0, 0, 0
0, 0, 0, 0, 0, 0, 0, 0
0, 0, 0, 0, 0, 0, 0, 0
0, 0, 0.5, 0, 0, 0, 0, 0
===

output:
---
[(0, 1), (0, 2), (0, 3), (0, 5), (0, 6), (1, 2), (1, 6), (1, 7), (2, 3), (2, 4), (3, 1), (3, 4), (3, 5), (7, 2)]
[(0, 1, 0.5), (0, 2, 0.5), (0, 3, 0.5), (0, 5, 0.5), (0, 6, 0.5), (1, 2, 0.5), (1, 6, 0.5), (1, 7, 0.5), (2, 3, 0.5), (2, 4, 0.5), (3, 1, 0.5), (3, 4, 0.5), (3, 5, 0.5), (7, 2, 0.5)]
===
'''


```

## Edge List --> Adjacency Matrix

```python
'''
  https://networkx.github.io/documentation/networkx-2.2/reference/generated/networkx.linalg.graphmatrix.adjacency_matrix.html

'''

import numpy
import networkx as nx

# edgelist to adjacency matrix

# way1: G=nx.read_edgelist
D = nx.read_edgelist('input/edgelist_sample.txt', create_using=nx.DiGraph(), nodetype=int)  # create_using=nx.Graph()
print(D.edges)
print(D.nodes)

# way2:
'''
a = numpy.loadtxt('input/edgelist_sample.txt', dtype=int)
edges = [tuple(e) for e in a]
D = nx.DiGraph()
D.add_edges_from(edges)   # D.add_edges_from(nodes); D.edges; D.nodes
D.name = 'digraph_sample'
print(nx.info(D))

udrtG = D.to_undirected()
udrtG.name = 'udrt'
print(nx.info(udrtG))
'''

# dump to file as adjacency Matrix
A = nx.adjacency_matrix(D, nodelist=list(range(len(D.nodes))))  # nx.adjacency_matrix(D, nodelist=None, weight='weight')   # Return type: SciPy sparse matrix
# print(A)   # type < SciPy sparse matrix >
A_dense = A.todense()   # type-> numpy.matrixlib.defmatrix.matrix
print(A_dense, type(A_dense))

print('--- See two row of matrix equal or not: ---')
print((numpy.equal(A_dense[5], A_dense[6])).all())

# print('to_numpy_array:\n', nx.to_numpy_array(D, nodelist=list(range(len(D.nodes)))))

# print('to_dict_of_dicts:\n', nx.to_dict_of_dicts(D, nodelist=list(range(len(D.nodes)))))

```

## About Adjacency LIST

```
nx.read_adjlist()

```

![](https://img2018.cnblogs.com/blog/780771/201905/780771-20190523193021270-1849833006.png)



## Convert Adjacency matrix into edgelist

```python
import numpy as np

#read matrix without head.
a = np.loadtxt('admatrix.txt', delimiter=', ', dtype=int)  #set the delimiter as you need
print "a:"
print a
print 'shape:',a.shape[0] ,"*", a.shape[1]

num_nodes = a.shape[0] + a.shape[1]

num_edge = 0
edgeSet = set()

for row in range(a.shape[0]):
    for column in range(a.shape[1]):
        if a.item(row,column) == 1 and (column,row) not in edgeSet: #get rid of repeat edge
            num_edge += 1
            edgeSet.add((row,column))

print '\nnum_edge:', num_edge
print 'edge Set:', edgeSet
print ''
for edge in edgeSet:
    print edge[0] , edge[1]
```

Sample Adjacency Matrix Input file:
```
0, 1, 1, 1, 0, 1, 1, 0
0, 0, 1, 0, 0, 0, 1, 1
0, 0, 0, 1, 1, 0, 0, 0
0, 1, 0, 0, 1, 1, 0, 0
0, 0, 0, 0, 0, 0, 0, 0
0, 0, 0, 0, 0, 0, 0, 0
0, 0, 0, 0, 0, 0, 0, 0
0, 0, 1, 0, 0, 0, 0, 0
```