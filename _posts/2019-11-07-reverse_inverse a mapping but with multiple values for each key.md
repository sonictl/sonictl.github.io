---
layout: post
title: 'reverse/inverse a mapping but with multiple values for each key'
date: 2019-11-07 14:19:00 +0800
category: from_cnblogs
slug: p20191107141900
---
## reverse/inverse a mapping but with multiple values for each key
multi mappping dictionary , reverse/inverse

```python
'''
mrg_dictionary: {15: {16, 19, 21, 23}, 22: {18}}
inv_merging_led_dict: {16: 15, 19: 15, 21: 15, 23: 15, 18: 22}
    mrg_dictionary --> inv_merging_led_dict
'''
mrg_dictionary = {15: {16, 19, 21, 23}, 22: {18}}
inv_merging_led_dict = {value:key for key in mrg_dictionary for value in mrg_dictionary[key]}
print(inv_merging_led_dict)

'''
inv_merging_led_dict: {16: 15, 19: 15, 21: 15, 23: 15, 18: 22}
mrg_dictionary: {15: {16, 19, 21, 23}, 22: {18}}
    inv_merging_led_dict --> mrg_dictionary
'''
valueset = set(inv_merging_led_dict.values())
mrg_dictionary = {k:set() for k in valueset}    # {15:set, 18:set()}
for key in mrg_dictionary.keys():
    pop_k_list = []
    for k in inv_merging_led_dict:
        if inv_merging_led_dict[k] == key:
            mrg_dictionary[key].add(k)
            pop_k_list.append(k)
    for k in pop_k_list:
        inv_merging_led_dict.pop(k)

print('mrg_dictionary', mrg_dictionary)
```