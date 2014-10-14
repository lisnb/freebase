title: 一些基本概念
date: 2014-10-14 14:18:08
tags:

---

对于那些刚刚开始接触Freebase的开发者，这部分内容涵盖了理解Freebase如何运作所需的基本技术和概念。

##<span id="graphs">图（graphs）</span>
------
Freebase中的数据存储在一种叫做[图（graph）](http://en.wikipedia.org/w/index.php?title=Graph_\(abstract_data_type\))的数据结构中。图是由节点和连接这些节点的边共同构成。在Freebase中，使用/type/object来定义节点，使用/type/link来定义边。使用这种方式来存储数据使得Freebase能够快速的随机访问主题之间的关联，并且能够简便的添加新的数据模式而不对已有的数据结构进行更改。

##<span id="topics">主题（topics）</span>
------

在Freebase中，像人物(people)，地点(place)和事物(thing)这些现实生活中的实体有三千九百万个。因为Freebase使用图来表示的，这些主题和图中的节点相关联。然而并不是每个节点都是一个主题。在[CVTs](#cvts)这个部分你可以找到一些不是主题的节点的例子。

下面是一些关于Freebase中主题类型的例子：

- 物理实体，如：Bob Dylan, the Louvre Museum, the Saturn planet 等
- 艺术/多媒体作品，如：The Dark Knight(电影), Hotel California(歌曲)等
- 类别，如：noble gas, Chordate 等
- 抽象概念，如：love 等
- 思想流派或者艺术流派，如：Impressionism.

有些主题因为蕴含非常多的数据（例如：Wal-Mart）显得非常的重要，也有一些主题因为连接了很多其他领域内的主题而显得重要。比如，像love, poverty, chivalry等等这些抽象概念，虽然本身并不包含什么属性，但因为它们通常表现为书籍，诗歌和电影等，所以也显得非常重要。

##<span id="types-properties">类型和属性</span>
------

无论哪个主题，都可以从多个不同的角度来解读，比如：

- Bob Dylan 是一个作家，歌手，演员，书作者，电影导演；
- Leonardo da Vinci 是一个画家，雕塑家，解剖学家，建筑师，工程师；
- Love 是书籍的主题，电影主题，诗歌的主题，演出的主题；
- 任意一个城市都是一个地理位置，也可能是一个游客的目的地或者是一个公务员的雇佣者等等

为了更好的表示很多主题的这种多重意义，Freebase中引入了*类别(types)*的概念。在Freebase中，每个主题可以有个类别信息。Bob Dylan这个主题被赋予以下的类别：作曲家，词作者，音乐家（歌手），书作者，等等。每个类别都会有一些列的跟这个类别密切相关的*属性(properties)*。如：

- 音乐艺术家这个类别包括一个包含Bob Dylan创作的所有专辑的列表和已知的他会使用的乐器列表
- 书作者这个类别包括一个包含Bob Dylan写的所有书的列表和他的写作和思想的流派
- 公司这个类别包括一个包含这个公司所有创建者、董事会成员、母公司、分公司、雇员、产品、年营业额和利润记录的列表等

综上，有一些比较普遍的属性，能够描述一个类别特定方面的信息，而类别就是盛放这些属性的一个概念上的容器。（你可以把类别想象成关系数据库中的表，每个类别的表都有一个指向能唯一定义主题的表的外键。）

##<span id="domains-ids">域和ID</span>
------









