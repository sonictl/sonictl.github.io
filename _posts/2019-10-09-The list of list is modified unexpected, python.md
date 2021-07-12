---
layout: post
title: 'The list of list is modified unexpected, python'
date: 2019-10-09 06:52:00 +0800
category: from_cnblogs
slug: p20191009065200
---
# Be careful!
### The list of list is modified unexpected, python

```python
# code patch A:
list = [1,2,3,4,5,6,7]
print('list A :', list)
for i in list:
    temp = i
    if i > 5:
        temp = i + 10000
print('list A\':', list)


# code patch B:
list = [[1],[2],[3],[4],[5],[6],[7]]
new_list = []
print('\nlist B : ',list)
for i in list:
    temp = i    # will this allocate a new RAM space for var 'temp'?
    if i[0] > 5:
        temp[0] = i[0] + 1000
    new_list.append(temp)
print('list B\': ', list, end='')
print(' <-- you can see the list is changed, which is not i want.')
print('new_list:', new_list)


# code patch C:
list = [[1],[2],[3],[4],[5],[6],[7]]
new_list = []
print('\nlist C : ',list)
for i in list:
    temp = ['']    # only this line is MODIFIED than code patch B.
    temp[0] = i[0]
    if i[0] > 5:
        temp[0] = i[0] + 1000
    new_list.append(temp)
print('list C\': ', list, end='')
print(' <-- you can see the list is NOT changed.')
print('new_list:', new_list)

'''
Output:
---
list A : [1, 2, 3, 4, 5, 6, 7]
list A': [1, 2, 3, 4, 5, 6, 7]

list B :  [[1], [2], [3], [4], [5], [6], [7]]
list B':  [[1], [2], [3], [4], [5], [1006], [1007]] <-- you can see the list is changed. which is not i want.
new_list: [[1], [2], [3], [4], [5], [1006], [1007]]

list C :  [[1], [2], [3], [4], [5], [6], [7]]
list C':  [[1], [2], [3], [4], [5], [6], [7]] <-- you can see the list is NOT changed.
new_list: [[1], [2], [3], [4], [5], [1006], [1007]]

Question:
 why list B' is modified in the for loop?
 
'''

```