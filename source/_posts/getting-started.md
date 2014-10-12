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
对于像[MQL write（未本地化）](http://freebase.lixipeng.me/#malwrite-overview)这样需要额外的认证的API，请参见 [请求的认证（未本地化）](http://freebase.lixipeng.me/#how-tos/authorizing)。



###<span id="limits-quotas">配给和使用限额</span>
------

	


　　　　
　
　　　　
	

