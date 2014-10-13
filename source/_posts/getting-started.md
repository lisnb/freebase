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
如果你觉得对于你来说，100k次都不足以满足你的需求，那你可能需要考虑一下Freebase的[本地数据集](https://developers.google.com/freebase/data)了，这样，你就能在本地访问Freebase了。
当然，你也可以在APIs Console(APIs 控制台)处，申请更高的额度。方法是到[APIs Console](https://code.google.com/apis/console)，然后点击“Quotas”，再点击“Request more... ”，从而获得更多对Freebase API的调用额度。
> 
**鹏宝说**：实际上操作起来要比这个复杂一点。首先这个网址经过了几次重定向，到达[这里](https://console.developers.google.com/project?authuser=0)，然后你需要选择一个你的应用，点击进入，然后再左侧的“APIs&auth”中选择“APIs”，就能看到“Enabled APIs”中有“Freebase”这项，后面还有额度和已经使用了多少以及是否开启这个API的选项。鼠标悬停在那个标示用量的进度条上，在弹出的tip中，有“Request more”的链接，点进去是一个申请的页面，需要填写一些信息，比如你的应用的描述等等。
![request more](/img/getting-started/Request-more.png)


###<span id="libraries">库</span>
与此同时，新设计的Freebase API使得您可以使其同[Google API Client Libraries](https://developers.google.com/freebase/v1/libraries)协作


##<span id="api-services">API 服务</span>
----
Freebase API的设计使得开发者可以在Freebase中查询关键词，结构化数据，文本数据和图像数据。

[**Search API**](#search-overview)
　　通过给定的关键词和其他约束，查找实体
[**Topic API**](#topic-overview)　
　　获得关于一个给定实体的综述信息（summary）
[**MQL Read/Write APIs**](#mql-overview)
　　检索或者写入一个实体或者一个实体集合的详细的结构化数据
[**Freebase Suggest Widget**](#suggest)
　　在网页上使用这个JQuery部件以选择实体
　　
Search API 根据给定的任意的文本查询给出Freebase中的相关数据。`查询(search)`服务不仅会在Freebase中索引给定的实体的数据，同样也会在其他的数据源中进行索引，比如维基百科的数据。

Topic API 是一个能够根据给定的主题，返回相关的包括图像和文字在内的所有已知信息的网络服务。

MQL Read/Write API 使得开发者能够使用 [MetaWeb Query Language](http://mql.freebaseapps.com/) 在 Freebase中进行结构化的查询。如果你像要获得Freebase中存储的图的数据，或者想要查找一些关于“*directors of French comedies*” 或者 “*mountains in California*”的信息，那么你就应该使用这个服务。

另外，如果你愿意，你也可以使用 Freebase Suggest Widget 在你的网页上添加可以查找实体的可视化控件。


##<span id="common-conventions">通用惯例</span>
------

###<span id="api-requests">API 请求</span>

调用API的URI格式是这样的：

	GET https://www.googleapis.com/freebase/<version>/<apiname> <path>? <urlparams> 
	
所有的请求都必须使用`HTTPS`协议。

- `version`版本，参数的取值只能是`v1`（代表生产环境）和`v1sandbox`（代表沙箱环境）中的一个。（关于生产环境和沙箱环境的更多信息，可以参考[环境和版本](#environments-versions)。）
- `apiname`参数是用来指定调用的API，比如`topic`和`mqlread`
- `path`和`urlparams`是由调用的API来决定的

###<span id="environments-versions">环境和版本</span>

Freebase提供了两种不同的数据环境。（你也可以认为是两个不同的数据库。）

####<span id="production-environment">生产环境<span>

Freebase的成产环境意味着它将在正式的产品应用中被使用。这就对生产环境有以下的要求：

- 必须稳定。因为API调用是在其基础上运行的。
- 数据质量要高。只有QAed和最终的数据才能被添加进来。

在你的应用中使用生产环境的Freebase服务：

	GET https://www.googleapis.com/freebase/v1/search?query=bob
	
####<span id="sandbox-environment">沙箱<span>

为了提供一个实验和测试的环境和数据，我们采用了沙箱机制。
开发者可以使用沙箱做以下事情：

- 针对这个环境开发您的应用
- 在这个环境下实验心地数据和模式

每个月的第一个周一，沙箱依据生产环境处更新，所以，你可以尽情的使用这个环境和其中的数据进行实验，但需要牢记的是，您的数据会被定期清除。

	GET https://www.googleapis.com/freebase/v1sandbox/search?query=bob
	
注：当需要选择沙箱环境的时候，Freebase将环境和版本的表示结合到一起，成为URI的一个参数。就是说，沙箱环境的版本参数取值为`v1sandbox`，而生产环境的参数取值为`v1`。

###<span id="common-parameters">通用参数</span>

像其他的Google APIs一样，Freebase支持很多标准的参数（包括下面会提到的`callback`参数），如果想获得更多细节，请参考[Freebase Standard Parameters](#v1-parameters)。

####<span id="using-callback">使用回调函数</span>

URL中的`callback`参数使得返回JSON格式的数据成为指定的该函数的参数，这样就可以支持[JSONP](http://en.wikipedia.org/wiki/JSONP)，这种设计使得当你希望在网页上直接嵌入Freebase的返回数据时可以做跨域访问。当然，这个参数只能应用于读取JSON的API中。

例如：

	GET https://www.googleapis.com/freebase/v1/search?query=bob&callback=funcname
	
当请求结束后，你代码中的回调函数就能够执行，如下：

{%codeblock lang:javascript %}
<script>
function myfuncname(result) {
    alert(result[‘name’]);
}
</script>
{%endcodeblock%}

###<span id="security">关于安全</span>

<div style="background:#eb7a77;color:white;">
**Freebase中的所有内容都是由用户生成的。**当你想要在你的应用中使用Freebase APIs的时候，请确定你完全了解这其中包含的安全性问题。

</div>

正因为Freebase中的所有内容都是用户生成的，所以你不能未经处理就将这些内容显示在网页上。`处理（Cleaning-up）`是一个比较模糊的概念，所以这里总结了几种不同的解决这个问题的机制：

- **Sanitization** 去除一些存在潜在威胁的HTML的标签。比如：将`<p><script>alsert(1)</script></p>`处理为`<p></p>`
- **Stripping** 直接去除所有的HTML标签。比如：将`<p><script>alsert(1)</script></p>`处理为空字符串
- **Unicode Encoding** 对于一些特定的字符，比如`<`,`>`和`&`等，使用它们的Unicode编码来表示它们：`\u003c`,`\u003e`,`\u0026`。所有的JSON解析器都能自动的将它们重新转义回来
- **HTML-escaping** 将特定的HTML字符替换为HTML实体。比如`<`,`>`会被替换成为`&lt;`,`&gt;`

在默认情况下，所有的JSON APIs的输出都使用Unicode编码，每个Freebase API都有一个可以用来执行是否开启采用这个安全机制的参数。

| API Name | Parameter | Description |
|:---------|:----------|:------------|
| [Search API](#search) | `encode` | 字符串型参数。可选值为html和off,默认为off。 | 
| [Topic API](#topic) | `N/A` | (没有可用的编码支持) |
| [MQL Read API](#mqlread) | `html_encode` | 布尔型参数。默认是 TRUE。|

关于每个API所支持的所有参数的介绍，可以参考文档[Freebase API Reference](https://developers.google.com/freebase/v1/)。


------


本文所有内容均翻译自[Google Freebase API 官方文档](https://developers.google.com/freebase/v1/getting-started)
同原文档相同，本文也使用[知识共享署名3.0许可](http://creativecommons.org/licenses/by/3.0/)
如有疑问或者交流，欢迎发送邮件至`lisnb.h@hotmail.com`联系作者~ 














　　





	


　　　　
　
　　　　
	

