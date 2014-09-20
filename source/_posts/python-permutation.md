title: python permutation
date: 2014-05-30 11:24:24
tags:
- python
- 全排列
- permutation
categories: 
- python
- 国哥

---
>全排列算法一直以来都是我的一个伤痛

<!--more-->

当然这简单到不能叫做一个算法，同时类似的伤痛还有非常之多
但就是不能跨过去，把我弄的也很头痛

{% codeblock lang:python permutation %}
def permutation(l,i=0):
    if i == len(l)-1:
        print l
    else:
        for j in range(i,len(l)):
            l[i],l[j]=l[j],l[i]
            permute(l, i+1)
            l[i],l[j]=l[j],l[i]

if __name__ == '__main__':
    permutation([1,2,3])
{% endcodeblock %}
这也是我这样一个菜逼选择python的一个重要的原因，因为
{% codeblock lang:python itertools.permutation %}
import itertools
itertools.permutations([1,2,3])
{% endcodeblock %}
应该是一个生成器。



