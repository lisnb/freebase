title: 开始学习 Freebase API
date: 2014-10-12 13:53:12
tags:

---


##内容
------
　　[开始](#getting-started)
　　　　[API keys](#api-keys)
　　　　[配给和使用限额](#limits-quotas)
　　　　[库](#libraries)
　　[API 服务](#api-services)
　　[通用惯例](#common-conventions)
　　　　[API 请求](#api-requests)
　　　　[环境和版本](#environments-versions)
　　　　　　[成产环境](#production-environment)
　　　　　　[沙箱](#sandbox-environment)
　　　　[通用参数](#common-parameters)
　　　　　　[使用回调函数](#using-callback)
　　　　[关于安全](#security)

##<span id="getting-started">开始</span>
------
![Graph-API-Services](/img/getting-started/Graph-API-Services.png)

Freebase API是一些列的HTTP API的集合，它提供了对存储在Freebase中的数据的读写方法。不同的API支持不同的场景，从而使得可以通过不同的方法对同样的Freebase数据进行访问。

###<span id="api-keys">API keys和权限验证</span>
使用Freebase API 之前，你必须[获得一个API key](https://code.google.com/apis/console)，也有一些用于调试的无需使用API key的配额。

一旦你已经有了一个`key`,你就可以通过使用URL的`key`参数，将它传给任何一个API：

	https://www.googleapis.com/freebase/v1/search?query=bob&key=<YOUR_API_KEY>
	
如果想获得更多关于API keys的帮助，可以参考关于 [Keys, security, and identity 的文档](https://developers.google.com/console/help/#WhatIsKey)。
对于像[MQL write（未本地化）](#mqlwrite-overview)这样需要额外的认证的API，请参见 [请求的认证（未本地化）](#how-tos/authorizing)。



###<span id="limits-quotas">配给和使用限额</span>


在[使用限制](http://freebase.lixipeng.me/#usage-limits)中可以找到更详细的信息。
Freebase 允许一个用户在一天中，调用`100k`次读取数据的API（每24小时重新计算），调用`10k`次[写数据](#usage-limits-write-quoto)的API.
如果你觉得对于你来说，100k次都不足以满足你的需求，那你可能需要考虑一下Freebase的[数据集](https://developers.google.com/freebase/data)了，这样，你就能在本地访问Freebase了。
当然，你也可以在APIs Console(APIs 控制台)处，申请更高的额度。方法是到[APIs Console](https://code.google.com/apis/console)，然后点击“Quotas”，再点击“Request more... ”，从而获得更多对Freebase API的调用额度。
> 
**鹏宝说**：实际上操作起来要比这个复杂一点。首先这个网址经过了几次重定向，到达[这里](https://console.developers.google.com/project?authuser=0)，然后你需要选择一个你的应用，点击进入，然后再左侧的“APIs&auth”中选择“APIs”，就能看到“Enabled APIs”中有“Freebase”这项，后面还有额度和已经使用了多少以及是否开启这个API的选项。鼠标悬停在那个标示用量的进度条上，在弹出的tip中，有“Request more”的链接，点进去是一个申请的页面，需要填写一些信息，比如你的应用的描述等等。
![request more](/img/getting-started/Request-more.png)


###<span id="libraries">库</span>
与此同时，新设计的Freebase API使得您可以使其同[Google API Client Libraries](https://developers.google.com/freebase/v1/libraries)协作




	


　　　　
　
　　　　
	

