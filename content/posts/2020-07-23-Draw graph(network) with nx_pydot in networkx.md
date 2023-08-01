---
layout: post
title: 'Draw graph(network) with nx_pydot in networkx'
date: 2020-07-23 01:23:00 +0800
category: from_cnblogs network
slug: p20200723012300
---
# Draw graph(network) with nx_pydot in networkx

with `nx_pydot` in `nx.drawing`, we can have more plotting style of network/ tree/ graph. 

Here is a simple sample:

```python
import networkx as nx

g=nx.DiGraph()

edgelist = [(0, 1), (1, 12), (2, 12), (3, 17), (4, 11), (5, 4), (6, 10), (7, 12), (8, 14), (9, 14), (10, 11),
            (11, 14), (12, 11), (13, 17), (14, -1), (15, -1), (16, 10), (17, 11), (18, -1)]

g.add_edges_from(edgelist)
p=nx.drawing.nx_pydot.to_pydot(g)
p.write_png('pydot_Tree.png')
```

the plot result:
<img src="https://img2020.cnblogs.com/blog/780771/202007/780771-20200723092116705-652754159.png" alt="" width="350" height="230" />
