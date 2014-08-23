title: 'bubbles bling bling'
date: 2014-06-21 17:03:29
tags: 
- js
- bubble
- python
- 汉字点阵
categories:
- python
- js
---
> 
在很久很久以前，你拥有我，我拥有你
今天发现《外面的世界》真好听 (*^__^*) 

<!-- more -->

<script type="text/javascript" src="/js/jquery-2.1.0.min.js"></script>
<script type="text/javascript" src="/js/customize/bubble/udalpha.js"></script>
<input type="hidden" id="bubbleword" value="南开大学">
<canvas id="myCanvas"></canvas>
<script type="text/javascript" src="/js/customize/bubble/bubbles.js"></script>
<script type="text/javascript" src="/js/customize/bubble/main.js"></script>

明天是本科学校的毕业晚会
"聚散天涯，依依南开"
![](/img/0621/1.PNG)
那天很激动，感觉离开很舍不得，非常的“依依”。到现在没有人拿着这个状态质问我到底回不回去，反倒是当我说我想回去看看的时候，都在问我：你回去干嘛？
嗯，又不是杰出校友。
不过作为一个杰出菜逼校友，悄悄的溜回去看一下，还是请大家高抬贵手，轻喷
有大半年没回去了（每次我这么说都很忐忑，因为总感觉有种“你爱多久没回去就没回去呗，学校又不在乎你”的感觉，不过你知道的，鹏宝是一个感性的人捂脸笑），跟小马哥说好了要回去，小马哥表示欢迎，并表示可以住在他们宿舍，我非常开心，上一次回去也是在他宿舍睡的，但现在头发有点长，不知道能不能进去呢。
其实主要是感觉头发不知道什么时候就剪掉了，赶紧抓紧时间回去得瑟一下，要不白留了（我这人，太丁日虚荣捂脸笑）

---------------

前几天翻到[果壳任意门][2]，在编程语言那个tab下面看到了“Codecademy”（没错就这点出息了），一直想学点js，css什么的，因为鹏宝对美好的事物总是有些追求尽管鹏宝自己并不那么美好。
它的第一个课程是“[Animate Your Name][1]”，就用一些小泡泡把你输入的东西画出来，鼠标划上去就动，跟上面的一样，我觉得太漂亮了因为鹏宝没见过什么世面，正好还是个教程，所以那天晚上毅然放下了手里的活计，开始教程。
![](/img/0621/2.PNG)
![](/img/0621/3.PNG)
结果教程只是在说一些基本的js，定义变量啊，列表啊什么的，根本没有讲怎么画这些小泡泡并且让它动起来，我很惋惜，所以就找到了它的源文件，想自己看一下。
主要有三个源文件
- main.js  这个里面就是定义显示什么文字还有泡泡的颜色的
- bubble.js  主要的文件，画泡泡和接收鼠标事件然后做出动作的
- alphabet.js  一开始没看懂，后来明白了。是把文字转换成点阵坐标的
看到最后的文件，我想起来大一在上C++的上机课的时候，王超老师让我们写一个小程序，输入一个字母，然后用这个字母作为“像素”，把这个字母表示出来
大概就是这个样子：
![](/img/0621/4.PNG)
后来老师想了想，算了吧，太难了，固定输出一个字母的形状就行了。
当时好像也觉得，给一个字母，不知道字母各个像素的坐标是什么，怎么能表示出来呢
所以这个教程里引入了一个js文件，内容大概就是：
{% codeblock lang:javascript alphabet.js %}
document.alphabet={
                A79:{
                        W:75,P:[[64,89,9,-102],[57,103,9,-102],[5,89,9,-79],
                                [16,104,8,-35],[51,122,8,-35],[23,118,8,-35],
                                [31,133,8,50],[46,136,8,50],[34,153,8,69],
                                [28,168,7,112],[21,183,7,112]]},
                A78:{
                        W:85,P:[[10,148,8,-103],[21,137,8,-92],[33,125,7,-79],
                                [50,124,7,-35],[58,135,7,-35],[68,148,7,-35],
                                [40,111,7,51],[33,103,7,51],[21,86,7,51],
                                [56,106,7,51],[67,92,7,112]]},
                ...};
{% endcodeblock %}
上面的这段表示的是字母“O”和“N”的各个点的坐标，键值是“A”+字母的ASCII（英语发音：/'æski/ ASS-kee，以后我读这个的时候不要纠正我这个是 “ask” 或者 “ask2”了行吗），W的值是这个字母的宽度，用于显示多个字符的时候计算offsite.
后面的P就是各个点的坐标了。“O”的第一个点的坐标[64, 89, 9, -102]，64和89分别是横纵坐标（咦，这两个数怎么这么奇怪？不要查我水表... ），9是这个点的大小，半径之类的，你看到那些点的大小都不是一样的，第四个数字好像是标示这个点的透明度之类的，原来的代码里用这个数字的部分被注释掉了，但看起来就是比如字母的末端可能fade了之类的。
所以通过这个文件就解决了当拥有了泡泡的时候，怎么组成字母的问题。
{% codeblock lang:javascript bubble.js %}
...
function phraseToHex(phrase) {
    var hexphrase = "";
    for (var i = 0; i < phrase.length; i++) {
        hexphrase += phrase.charCodeAt(i).toString(16);
    }
    return hexphrase;
}
function drawName(name,colors){
    function addLetter(cc_hex, ix, letterCols) {
    //画字符的函数
    ...
    }
    var hexphrase = phraseToHex(name);
    var col_ix = -1;
    for (var i = 0; i < hexphrase.length; i += 2) {
        var cc_hex = "A" + hexphrase.charAt(i) + hexphrase.charAt(i + 1);
        if (cc_hex != "A20") {
            col_ix++;
        }
        addLetter(cc_hex, col_ix, letterColors);
    }
}
...
{% endcodeblock %}
先用phraseToHex函数把输入的字母字符串，转换成ASCII的字符串，然后每两个前面加上一个“A”去document.alphabet里找字符的点阵坐标，画出。
我一开始觉得这个屌啊，这岂不是可以表示一切了么，所以就写了几个汉字，结果表示出来的并不是汉字，而是几个字母，后来我才想起来，这个文件里没有汉字的点阵坐标，所以表示不出来汉字。
那个bubble.js我还没有完全看懂，尤其是语法，因为javascrip的函数定义方式感觉很奇怪，我主要想解决的问题是，怎么能够让这个玩意显示汉字呢。
最主要的就是1、给出汉字的坐标；2、以汉字为键值，找到这个坐标。
对于第一个问题：怎样给出汉字的坐标
我的解决思路是，手工的去写，这样能保证非常精准。开玩笑的，怎么可能，多傻逼啊... 
之前写过一个把图片转换成字符画的小程序
![](/img/0621/ali.jpg)
![](/img/0621/5.PNG)
呃...虽然失真比较严重，是因为阈值没有仔细设置而且字符比较单一，知道可以实现就放在那了。
就是把图片灰度化，然后根据像素的数值[110,255]的就是空白，[40,110)的就是"."，[0,40)的就是"@"。
那如果有文字的图片就也可以这么做，所以就把文字先生成图片，然后根据图片的像素标出一些点
但实际上，如果以像素为单位的话，那点就太多了，所以就把图片分块，在一个块里，有多少个像素小于一定阈值（就是黑到一定程度，证明这个块是组成字的一部分），这个块就可以形成一个点，再算一下坐标什么的，就可以了。
最后根据输入的字符串，生成一个js文件，替换原来的alphabet.js，就搞定了。

{% codeblock lang:python chartojs.py %}
#!/usr/bin/env python
# -*- coding: utf-8 -*-
# @Author: LiSnB
# @Date:   2014-06-20 18:36:07
# @Last Modified by:   LiSnB
# @Last Modified time: 2014-06-21 19:25:43
# @Email: lisnb.h@gmail.com

#import os
from PIL import Image, ImageFont, ImageDraw
import numpy as np 
import math 
#import matplotlib.pyplot as plt
import random
#import chardet

def getalphabet(text,jspath):
    size = 18
    partical = 2
    p_th = math.pow(partical, 2)-partical
    font = ImageFont.truetype('hwhp.ttf', size+2)
    alphas = []
    for t in text:
        im = Image.new('L', (size,size),255)
        dr = ImageDraw.Draw(im)
        dr.text((0,0), t,font = font,fill='#000000')
        pixel = im.getdata()
        p_array = np.array(pixel).reshape(size,size).T
        # print p_array
        p_array = p_array.tolist()
        points = []
        for i in range(size/partical):
            for j in range(size/partical):
                a = p_array[partical*i][partical*j:partical*j+2]
                a.extend(p_array[partical*i+1][partical*j:partical*j+2])
                if a.count(0)>p_th:
                    tmp_points='[%s, %s, %s, %s]'%( size*i,
                                                    size*j+size,
                                                    random.randint(4,7),
                                                    0)
                    points.append(tmp_points)
        t_key = t.encode('unicode_escape')
        t_key = 'A%s'%t_key[2:]
        P = 'P:[%s]'%(','.join(points))
        alpha = '%s:{W:180,%s}'%(t_key,P)
        alphas.append(alpha)

    content = 'document.alphabet={ %s};'%(','.join(alphas))
    with open(jspath,'wb') as f:
        f.write(content)

if __name__ == '__main__':
    getalphabet(u'允公允能日新月异','udalphabet.js')
{% endcodeblock %}

这个就可以生成一个拥有“允公允能日新月异”的js文件了。
这里有几个问题：
- 文字生成图片使用PIL做的，但官网上的PIL库在编译的时候少了点什么，在使用文字生成图片的时候会报错，我不记得是什么错误了，解决办法是在[Unofficial Windows Binaries for Python Extension Packages][3]这个地方下载非官方的PIL库，并在使用时，由
    import Image
变成
    from PIL import image
- 文字生成图片的时候，需要指定文字的字体，我本来想用比较喜欢的一些字体，但实际上，因为之后会对图像进行分块，所以要尽量使用粗一点的字体，我选择的是华文琥珀，如果你的电脑上装了这个字体（应该是都装了的），那看起来是这个样子的：
<span style="font-family:华文琥珀;font-size:30px">英格兰你伤了哥的心了</span>
但也因为汉字比较复杂，所以最后分辨率比较低，很写意的感觉... 
- 我也不知道原来的那个alphabet.js是怎么规定的，直接按照正常的方向生成的坐标画出来竟然是倒下来的，也懒得研究js的代码了，直接在生成坐标的时候，使用了像素矩阵的转置，就是代码中的第28行
p_array = np.array(pixel).reshape(size,size).T
- 汉字用unicode表示，应该是UTF-8的，所以键值用“A”+t.encode('unicode_escape')[2:]，因为比如“允”的表示是“\u5141”，键值最后是“A5141”。汉字的笔画比较复杂，所以最后生成的点也比较多，结果不在这里放了，可以看这个[udalphabet.js](/js/customize/bubble/udalpha.js)

然后解决第二个问题：以汉字为键值，找到这个坐标
原来的版本因为字母使用ASCII表示的，所以每个字符的长度是2，但现在汉字是4，所以要对bubble.js改一点：
{% codeblock lang:javascript bubble.js %}
...

function drawName(name,colors){
    ...
    for (var i = 0; i < hexphrase.length; i += 4) {
        var cc_hex = "A" + hexphrase.charAt(i) 
                         + hexphrase.charAt(i + 1)
                         + hexphrase.charAt(i + 2)
                         + hexphrase.charAt(i + 3);
        if (cc_hex != "A20") {
            col_ix++;
        }
        addLetter(cc_hex, col_ix, letterColors);
    }
}
...
{% endcodeblock %}

大功告成，最后的效果就是<a href="#myCanvas">这样</a>了。




[1]:http://www.codecademy.com/zh/courses/animate-your-name/0/1 "codecademy: animate your name"
[2]:http://gate.guokr.com/ "果壳任意门"
[3]:http://www.lfd.uci.edu/~gohlke/pythonlibs/ 