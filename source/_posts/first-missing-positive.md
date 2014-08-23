title: <leetcode> first missing positive </leetcode>
date: 2014-07-20 22:52:58
tags: 
- leetcode
- python
categories:
- leetcode
- python 
---
> 
Given an unsorted integer array, find the first missing positive integer.
For example,
Given <code>[1,2,0]</code> return <code>3</code>,
and <code>[3,4,-1,1]</code> return <code>2</code>.
Your algorithm should run in O(n)time and uses constant space.

> 
给你一个没有乱序的数组，找到这个数组中，不存在的最小的整数。
比如 <code>[1,2,0]</code> 中，因为是从0开始，而且连续，所以最小的不存在于这个数组中的数是3；
<code>[3,4,-1,1]</code>中，最小的不在这个数组中的数是2。
时间复杂度为O(<l>n</l>)，空间的“constant space”没懂什么意思... 

-------

如果是排序的，那从前面开始数，找到就好了。但是给的数组是乱序的，排个序怎么着也得O(nlogn),时间就超了。
但总感觉最小值不是直接找出来的，而是从1开始，一个一个试出来的。
所以就按照这个思路，用非常脏的“算法”写的。
{% codeblock lang:python firstmissingpositive.py %}
#!/usr/bin/env python
# -*- coding: utf-8 -*-
# @Author: LiSnB
# @Date:   2014-07-19 23:17:10
# @Last Modified by:   LiSnB
# @Last Modified time: 2014-07-20 22:39:49
# @Email: lisnb.h@gmail.com

"""
# @comment here:

Given [1,2,0] return 3,
and [3,4,-1,1] return 2.
and [7,3,4,9,0,1,2] return 5.

"""
class Solution:
    # @param A, a list of integers
    # @return an integer
    
    def firstMissingPositive(self, A):
        if len(A) is 0:
            return 1
        ma = max(A)
        if ma <=0:
            return 1
        m=dict.fromkeys(A)
        mi=1
        for i in range(1,ma):
            if m.get(i,0) is 0:
                return i
        return ma+1
        
if __name__ == '__main__':
    s= Solution()
    print s.firstMissingPositive([7,8,9,1,2,3,4,5,6])

# ~$  10

{% endcodeblock %}

从1开始，逐步增加，第一个不出现在给定数组中的就是要求的值，但如果每次都要遍历判断是否在数组中，那就是O(n^2)的了
所以就把... 数组弄到字典里了... 
这样每次判断就只有O(1)... 

第<code>50,51</code>行用来搞定给定数组是空的情况
第<code>52-54</code>行用来获得数组最大值，确定递增的边界，并且如果整个数组都是负值（也就是最大值是负值），那就返回<code>1</code>

如果直到数组最大值所有的数都在数组中，那就类似给的第一个例子那样，应该返回<code>最大值+1</code>
注释比代码都多... _(:з」∠)_

> 
这不是解题报告，这是心路历程... 
甜言蜜语就那么好听？不懂。



