title: gdb-coredumped
date: 2014-06-19 23:33:40
tags:
- linux
- gdb
- cpp
- coredumped
categories: 
- linux
- cpp
---
> 
说的都没错，但听起来怪怪的

今天晚上想把代码在服务器上走一下形式，编译运行一下，通过了就收拾收拾回宿舍了。
结果编译没有问题，运行的时候突然报了“段错误 (core dumped)”，当时就尿了，呵呵，学长学姐的“legacy”。

<!--more-->

代码需要在windows和linux上跨平台运行，为了避免使用ftp工具（一开始一直使用psftp, 虽然命令行看起来有点炫酷但实在是太不方便了，后来找到了一个FileZilla, 挺好用的，但也挺麻烦），就开始用git。这几天还和范老师猛烈的研究git怎么用，幸好有林大神。
所以代码都是在Windows上写的，用Visual Studio。Visual Studio 我装了两个版本，2008 和 2013 Express。因为实验室里之前的组件都是用2008编译的，所以依赖那些的组件在2013上编译通不过，也就没法调试。但2008写代码实在是太难看了，尤其是配合上Visual Assist，简直难看，学长又拒绝使用2013重新编译原来的组件，所以机智的鹏宝会有一个build目录和一个build_2013目录，cmake两次，一次用2008的，一次用2013的，用2013的看代码写代码写注释，用2008的编译调试，诶，疲劳。
怎么说那去了。
总之发现了错误我很慌张，因为在windows上是没有问题的（诶，以后被测试的抓到可能说的最多的一句话了），所以也就没法在VS里调试，项目又很大，用gdb一点一点调试肯定会很难过。所以就去网上找，然后发现可以利用出错之后的core文件使用gdb找到出问题的地方。
然后就用 -DCMAKE_BUILD_TYPE=Debug 给生成的可执行文件添加了调试信息（这也解决了我之前的一个问题，我之前问范老师，为啥windows上的大家都提供一个release的lib，一个debug的lib，但linux上的只有release的呢？范老师说，linux上可能只有release吧。嘻嘻嘻嘻嘻，才不是...）
然后调整了core文件的大小限制，生成了core文件，最后用gdb找到了出问题的位置，卧槽这的代码我还没开始看呢... 烦死了。
值得庆幸的是，鹏宝机智的发现，原来是输入的文件的格式不对，程序没有问题，但其实这也算有问题，鲁棒性不强... robustness哈哈，这个单词好有意思... 
所以问题就解决了... 

-----------------------

在程序运行出错时，linux会生成一个core文件，用以记录系统运行的错误，这个文件的大小不一，依据当时的环境而定，可以设置这个文件的大小上限，如果上限小于当时实际的大小，那么这个文件就不会生成。
可以使用：
{% codeblock lang:bash %}
ulimit -c unlimited
{% endcodeblock %}
取消对下一次运行时，对core文件大小的限制，也可以在系统的配置文件中修改
为了尝试，我们写一个小程序
代码如下：
{% codeblock lang:cpp foo.cpp %}
#include <iostream>
#include <vector>
//using namespace std;



void baz()
{
	vector< char> vc;
	std::cout<< vc[2]<< std::endl;
}

void biz()
{
	baz();
}

void bar()
{
	biz();
}

void foo()
{
	bar();
}

int main()
{
	foo();
	return 0;
}
{% endcodeblock %}

写了这么多函数是为了突出调用关系。
很显然，在baz()函数中，访问了不存在的vc[2]会发生错误
先设置core文件的大小上限，然后使用g++编译代码，运行，发现产生了段错误，coredumped，并且在该目录下生成了core文件。
![](/img/0619/1.PNG)
随后，为了找到问题出现在哪里，使用gdb
{% codeblock lang:bash %}
gdb ./foo core
{% endcodeblock %}
然后输入
{% codeblock lang:bash %}
(gdb) where
{% endcodeblock %}
![](/img/0619/2.PNG)
就能看到堆栈和调用关系，找到出现问题的位置了，比如在这里我们就把问题定位在baz()函数里了，但问题是，如果baz()函数很复杂，那么能不能找到更精准的定位呢？
我们需要在编译的时候使用-g选项为生成的可执行文件添加调试信息：
{% codeblock lang:bash %}
g++ foo.cpp -o foo -g
{% endcodeblock %}
然后再按照上面的流程，就会发现，问题出现在baz()函数的第10行
![](/img/0619/3.PNG)


