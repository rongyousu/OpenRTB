# OpenRTB_API_Specification中文版(V2.3.1)

#名词解释
**IAB:** Interactive Advertising Bureau 交互式广告局。

# 前言
## 简介

RTP工程， 前身为OpenRTB组织， 在2010年11月份组建。致力于为数字媒体自动化交易开发一套全新的开放标准，以应对更大范围的平台，设备以及广告解决方案。本文档就是RTB工程努力的结晶，你可以在[www.iab.net](www.iab.net)找到最新的官方文档。

## 关于IAB的广告网络和交易协会

IAB 广告网络和交易协会主要由广告网络和广告交易平台成员公司的高层领导组成。该协会致力于促进当今世界日益复杂的广告市场中数字生态系统的利益。协会目标是促进完成在广告商，发布者，中间商，代理协会之间专业性和责任心的最高标准， 开发能够保证收入增长的程序以及创建保护消费者和行业的最佳实践。

RTB工程是IAB广告科技理事会下的一个工作小组。

## IAB 联系方式
Melissa Gallo
产品总监 – 数据和程序自动化, IAB 科学实验室
[melissa@iab.net](melissa@iab.net)

## 许可证

OpenRTB 规范是在[Creative Commons Attribution 3.0 License](https://creativecommons.org/licenses/by/3.0/)许可之下，基于[http://openrtb.info](http://openrtb.info)的工作。 在该许可证之外的权限你可以在[http://openrtb.info](http://openrtb.info)找到。如果要查看该许可证的副本， 可以访问[https://creativecommons.org/licenses/by/3.0/](https://creativecommons.org/licenses/by/3.0/)或者写信给Creative Commons, 171 Second Street, Suite 300, San Francisco, CA 94105, USA.

# 新手入门

本规范包含了实时竞价接口的详细解释。以下相关解释中多处涉及“对象”这个概念， 所以这里提前说明以下：并不是所有的对象都是必须的，每个对象可能包含多个可选的属性参数。为了帮助第一次接触本文档的读者，我们已经在文档中标注了不同场景下（比如banner, video等等）要完成一个最小可行的实时竞价接口，哪些参数是不可缺少的以及哪些属性是可忽略的。

最小可行接口应该包括标识有**required**以及**recommended**的参数，但是这些参数的作用范围与其特定的使用场景相关。在这些场景的参数描述表中， [Description] 列可能会进一步的标识及注解参数的**required**或者**recommended**状态。为了尽可能填充对象， 可选参数也是可以被填充的，这都取决于该行为的参与者（对象填充者）。

下表展示了不同修饰符的作用：
* 带有**required**为必须参数，带有**recommended**的选项是推荐参数，其余的（带有**default**修饰符或者什么都不带）则是可选参数。
* 按照习惯，通常会按照 **required-->recommended->default->none** 的顺序防止参数。

| Attribute | Type | Description |
| --- |---| ---|
|id|string;**required**|...|
|site|object;**recommended**|...|
|test|integer;**default** 0|...|
|tmax|integer|...|

> 注意： 由于**recommended**属性不是必须的， 所以并不是从每个数据源都可以获取到的。 所以建议所有的OpenRTB事务参与者完成下页中的集成清单来标识供应方在竞价请求中支持，哪些属性是需求方需要用来做广告决策的。

# 集成清单列表

![](https://raw.githubusercontent.com/leeowenowen/OpenRTB_API_Specification-/master/res/integration_checklist.png)
# 1. 介绍
## 1.1 目标/概览
OpenRtb工程的任务是为广告买家和广告位库存买家之间的通信提供开放的工业标准，以刺激实时竞价市场的增长。标准涉及到很多方面，包括但不限于实时竞价协议，信息分类，离线配置同步以及其他很多。

本文档为从早期”block list"项目和"OpenRTB Mobile"项目发展而来的实时竞价接口规范标准。这些标准旨在简化广告媒体库存的提供商（例如，广告交易， 代表发布商利益的广告网络， 卖方平台） 与其有竞争力的买家（例如，竞价者， 需求方平台以及代表广告商利益的广告网络）之间的关系。

![](https://raw.githubusercontent.com/leeowenowen/OpenRTB_API_Specification-/master/res/openrtb_ecosystem.png)

OpenRTB总的目标是为买家和卖家之间的沟通创造一种通用语言，这种倾向不是精确的规定每个交易的操作。 作为一个工程， 我们的目标是让参与的各方可以更容易的集成， 从而促进生态系统中交易的各方都能够进行更深层次的创新。

## 1.2 OpenRTB的历史
OpenRTB于2010年的11月份在三个需求方平台（DataXu, MediaMath和Turn）以及三个卖方平台（Admeld, PubMatic以及The Rubicon Project)上作为试点项目启动的。 第一个目标是使交易block lists各方的沟通标准化。OpenRTB 1.0 block list规范在2010年的12月发布。

在得到了业界普遍的正面响应之后， Nexage 介入 OpenRTB工程，并建议在实时竞价请求和响应协议上为OpenRTB创建API规范，尤其是为支持手机移动广告。 移动小组委员会在买方公司（DataXu, Fiksu以及[X+1]和卖方公司(Nexage, Pubmatic, Smaato以及Jumptap)之间形成了。这个项目促成了OpenRTB Mobile 1.0标准， 该标准在2011年2月发布。

随着移动规范的发布， 在与DataXu和ContextWeb协作的视频广告交易（BrightRoll以及Adap.tv)中，一个视频小组委员会也形成了，其目标在于合作支持将普通展示型广告，视频广告以及移动广告规范在一个文档中。这个努力最终促成了一个统一的标准-OpenRTB2.0,在2011年6月发布。

由于业界的广泛采纳， 随着OpenRTB在2012年1月2.1版本的发布，它被采纳为美国互动广告局（Interactive Advertising Bureau）的一项标准。

## 1.3 版本历史
**OpenRTB 实时竞价API**

 - 2.3 支持原生广告以及多个微小改动
 - 2.2 为私有市场直接交易，视频，移动广告以及regulatory signals做出的一些新的改动
 - 2.1 按照QAG规范进行一些修正，一些小的改动以及错误修正。
 - 2.0 将展示广告，移动广告以及视频广告标准归一到一个统一的标准
 - 1.0 OpenRTB 移动版的最初发布版

**OpenRTB 展示 Block List分支**

 - 1.2 发布商API
 - 1.1 为包含creative attributes的实时交易进行小改动
 - 1.0 OpenRTB block list规范的最初发布。


## 1.4 资源
 - OpenRTB Google Groups: [http://openrtb.info](http://openrtb.info)
 - OpenRTB Github 地址 [http://github.com/openrtb/OpenRTB](http://github.com/openrtb/OpenRTB)
 - 开发者社区邮件列表 [http://groups.google.com/group/openrtb-dev](http://groups.google.com/group/openrtb-dev)
 - 用户社区邮件列表 [http://groups.google.com/group/openrtb-user](http://groups.google.com/group/openrtb-user)

## 1.5 术语
| 术语 | 解释 |
| --- |---|
| RTB |实时竞价 Real-Time Bidding|
| Exchange | 为一次展现管理不同竞价者之间的竞拍行为的服务 |
| Bidder | 在实时竞拍中为获取一次展示参与竞价的实体 |
| Seat | 期望获取展示并使用竞拍者代表他们进行竞拍的实体 |
| Publisher | 操作一个或者多个网站的实体 |
| Site | 如果没有特殊说明，指的是一个支持广告内容的网页或者应用 |
| Deal ID | 标识在特定场景下Publisher与Seat之间预先安排用于购买展示的协议 |

# 2. OpenRTB 基础知识

下图展示了广告交易平台和它的竞拍者之间的交互行为。
广告请求首先从发布者的站点发起。
对于每一个请求，广告交易平台都会向多个竞拍者广播竞价请求，从竞拍者返回的竞拍响应信息会按照常用的竞拍规则进行决策，获胜者会被通知， 广告审定结果被返回。获胜通知地址以及广告审定结果可能包含任何标准宏以保证广告交易平台能够与竞拍者之间沟通竞拍相关的关键数据。

![](https://raw.githubusercontent.com/leeowenowen/OpenRTB_API_Specification-/master/res/rtb_sequence.png)

注意对于通知是否会丢失并没有显式规定。 这是由于该操作会消耗一定的系统性能和带宽。建议广告交易平台在请求/响应协议之外通过离线或者独立的处理流程来提供丢失的竞拍数据信息。

本规范专注于实时竞价交互中的竞价请求和响应以及胜出通知及其响应。其他的交互（比如阻塞列表同步， 流量控制）要么是未来执行的可选行为，要么是已经被OpenRTB定义的。

## 2.1 数据传输

交易平台和它的竞拍者之间的基础通讯协议是HTTP. 特别要说明的是，在发起竞价请求的时候需要使用HTTP POST方式，因为它比HTTP GET可以附带更多的内容，并且也更容易支持二进制数据。 胜出通知则可以使用HTTP POST 或者 GET, 这个是由交易平台决定的。除了空的竞价响应应当返回HTTP 204之外， 所有的调用都应当返回HTTP 200（例如，推荐的方式是在响应体中指明"no bid"), 

> **最佳实践:** 一种最简单和最有效的提高连接性能的方式是启用HTTP长连接， 也就是我们通常说的Keep-Alive. 通过减少两端的连接管理以及CPU使用， 对整个过程会有明显的性能提升。

## 2.2 安全性

SSL(Secure Socket Layer) 不是必须的， 因为这些调用都是服务器之间的调用， 可以用其他方式对数据的安全进行保护。除此之外， SSL 不被推荐使用也由于它额外的处理负载。

## 2.3 数据格式

JSON(Javascript Object Notation) 是竞价请求和响应的推荐数据格式。 JSON被选择是因为它的可读性以及紧凑性。数据负载的相关信息我们会在第3节和第4节详细描述。

交易平台可能需要提供额外的内容信息给有需要的竞价者。这些内容可能包括一个JSON, XML, Apache Avro, ProtoBuf, Thrift以及其他种类的格式的压缩形式。

竞价请求需要使用HTTP的Content-Type头在mime-type中指明内容的类型。标准JSON类型的mime-type是"application/json". 竞价响应的数据格式必须跟竞价请求相同。
···
 Content-Type: application/json
···

如果要使用其他可选的二进制类型， 交易平台或者SSP应当适当的指明Content-Type, 例如"Content-Type: avro/binary" 或者 "Content-Type:application/x-protobuf", 如果没有content-type头， 竞拍者应当认为类型是"application/json", 除非交易平台选择了另一种不同的默认类型。

作为约定，某个属性的确实有其确定的意义，大多数情况下，这标识该属性值是未知的，除非被特殊指明。

## 2.4 OpenRTB 版本号 HTTP 头

OpenRTB版本号应当在竞价请求的头中使用自定义头传递。这可以让竞价者在解析请求体之前判断消息的版本号。
此外，建议竞价者在响应头中使用相同的格式信息标识自己实现的协议版本，这个版本号可能与请求头中的版本号是不同的。

```
 x-openrtb-version: 2.3
```
版本号应当以 <主版本号>.<次版本号>的格式（例如， 2.3). 第一位或者第二位数字的变动标识着协议的改变。通常，第二位版本的改变应当向兼容，第一位版本的改动不需要向后兼容。任何第三位版本号的改动（例如2.0.1)都不应当改变协议自身， 而仅仅改变不影响协议内容的描述和注释。第三位的版本号不应当在HTTP头中包含，因为他们对整个实现不会有技术上的影响。

## 2.5 隐私设计

OpenRTB工程支持广告中卖方和买方之间的隐私策略。特别是OpenRTB支持 do-not-track(3.2.11节）， COPPA restriction signaling(7.1节）， 以及从卖方向买房通过User　Object传递用户数据（3.2.13节）的能力。

## 2.6 与 IAB 质量保证指南 之间的关系

OpenRTB与IAP的质量保证指南（QAG)完全兼容， 你可以在这里找到[http://www.iab.net/ne_guidelines](http://www.iab.net/ne_guidelines). 特别是本文档中的分类标准是从QAG继承而来。

## 2.7 定制与扩展

OpenRTB规范允许交易病态对其进行定制和扩展。每一个对象都可以包含扩展。为了保证扩展字段的一致性，扩展字段应当始终使用"ext"命名。

# 3. 竞价请求规范

RTB 事务在交易平台或者其他广告提供源向竞价者发起竞价请求是被发起。竞价请求由顶级请求对象组成，至少一个展示对象， 以及可能有选择性的包含一些其他的对象用于提供展示相关的上下文信息。

## 3.1 对象模型。

下图展示了竞价请求的对象模型。顶级对象（例如， 在JSON中没有命名的最外层对象）在这个模型中是表示`BidRequest`的。在它的直属子对象中，只有`Imp`是技术上必须的，因为它是描述一次展示的基础信息，必须使用`Banner`, `Video`以及`Native`中的至少一个来定义展示的类型（例如， 无论一个或者多个展示者想要接收；尽管一次竞价是这些指明类型中的明确的一种）。一个展示对象可能包含一组私有市场。

![](https://raw.githubusercontent.com/leeowenowen/OpenRTB_API_Specification-/master/res/bid_request_object_model.png)

竞价请求的其他直属子对象用于为竞价者的用户定向及广告定价提供多种形式的辅助信息。这包括用户的详细信息以及他们使用的设备，位置信息，监管约束条件以及广告的展示载体。

对于后者，在站点（即web站点）以及应用（即手机中安装的非浏览器应用）有一个区别。抽象类`DistributionChannel` 仅仅是一个模型上的抽象， 用于表明一个`BidRequest`与一个`Site`或者`App`相关联，但不会同时与他们关联（即一个发布渠道是一个抽象的站点或者一个应用). 站点和应用都可以被更进一步的描述， 比如他们的发布者， 内容，内容产生者。

上图中没有展示的是扩展对象。这是一个未定义的但是可以附加到任意其他对象中用于传达交易相关的扩展信息的对象。使用这些对象的交易平台有责任将这些扩展信息传递给竞拍者。

下表总结了竞价请求模型中用到的对象，可以作为以下子章节的索引。

| 对象 | 章节 | 描述 |
| --- | --- | --- |
| BidRequest | 3.2.1 | 顶级对象 |
| Imp | 3.2.2 | 包含某个指定展示的详细信息， 每个请求至少一个 |
| Banner | 3.2.3 | 一个Banner展示（包括banner视频）或者有视频的广告的详细信息 |
| Video | 3.2.4 | 一个视频展示或者Native展示的视频物料的详细信息 | 
| Native | 3.2.5 | 符合Native广告规范的Native展示的容器 | 
| Site | 3.2.6 | 调用展示的站点的详细信息 |
| App | 3.2.7 | 调用展示的应用的详细信息　|
| Publisher | 3.2.8 | 控制发布站点或应用内容的实体 |
| Content | 3.2.9 | 被发布内容的详细信息， 广告将会在其中展示 |
| Producer | 3.2.10 | 内容的产生着， 不一定是发布者（例如联合组织） |
| Device | 3.2.11 | 展示广告内容的设备的详细信息 |
| Geo | 3.2.12 | 设备的位置或者用户家的位置，由其父对象决定 |
| User | 3.2.13 | 设备的用户， 广告的受众 |
| Data | 3.2.14 | 来自特定数据源的附加的用户定向数据集合 |
| Segment | 3.2.15 | 来自特定数据源的关于一个用户的某个数据点 | 
| Regs | 3.2.16 | 影响该竞价请求中所有展示的监管条件 |
| Pmp | 3.2.17 | 适用于此次展示的一组私有市场协议 |
| Deal | 3.2.18 | 本次展示中买家和卖家之间的交易条款 |


##　3.2 对象规范

以下部分将会详细介绍竞价请求模型中的每个对象。 首先介绍下适用于所有如下内容的公约：

- 带有`required`修饰的属性的缺失将会技术上导致协议的破坏。
- 处于某些可选属性的业务重要性， 会被标识为`recommended`。
- 除非默认值被显示指定，一个缺失的属性被解释为`unknown`。

### 3.2.1 BidRequest

顶层竞价请求对象， 包含一个全局唯一的竞价请求或拍卖ID。 `id`属性是必须的因为其中至少包含一个展示对象（见3.2.2节）。 在这个对象中的所有其他对象所建立的规则和限制适用于其中的所有展示对象。

有一些子对象用于提供潜在买家的详细信息，在这些对象中个，`Site`和`App`对象，用于描述展示广告的发布媒体的类型。这些对象是高度推荐使用的， 但是每个竞价请求中只能使用一个，这取决于发起广告请求的是基于浏览器的页面内容或者非浏览器应用。

| 属性 | 类型 | 描述 |
| --- | --- | --- |
| id | string;**required** | 竞价请求的唯一ID, 由广告交易平台提供 |
| imp | object array; **required** | 代表提供的展示信息的数组， 要求至少有一个 |
| site | object; **recommended** | `Site`对象，表示发布者站点相关的详细信息， 仅仅对站点适用且推荐填充 |
| app | object; **recommended** | `App`对象，表示发布应用的详细信息。仅对应用适用且推荐填充 |
| device | object; **recommended** | `Device`对象， 表示展示将要被发送到的用户设备的信息 |
| user | object;**recommended** | `User`对象， 表示使用设备的对象， 广告的受众 |
| test | integer; default 0 | 标识测试模式，拍卖不计价。 0表示实况（非测试）模式，1表示测试模式 |
| at | integer; default 2 | 拍卖类型（胜出策略）1表示第一价格 ，2标识第二价格。交易特定的拍卖类型可以用大于500的值定义 |
| tmax | integer | 用于在提交竞价请求时避免超时的最大时间，以毫秒为单位，这个值通常是线下沟通的 |
| wseat | string array | 允许在本次展现上进行竞拍的买家白Seat名单。 交易平台和竞拍者必须提前协商好Seat IDs |
| allimps | integer; default 0 | 用于标识交易平台是否可以验证当前的展示列表包含了当前上下文中所有展示。（例如，一个页面上的所有广告位，所有的视频广告点（视频前，视频中，时候后））用于支持路由封锁。 0表示不支持或未知， 1表示提供的展示列表代表所有可用的展示。 |
| cur | string array | 本次竞价请求允许的货币列表， 使用ISO-4217 字母码。 仅在交易平台接收多种货币的时候推荐填充。
| bcat | string array | 被封锁的广告主类别，使用IAB 内容类别，参考列表5.1。 |
| badv | string array | 域名封锁列表（比如 ford.com) |
| regs | object | `Reg`对象， 指明对本次请求有效的工业，法律或政府条例 |

### 3.2.2 Imp

这个对象描述了一个广告位或者将要参与竞拍的展现。一个竞价请求可以包含多个`Imp`	对象，这种状况的一个示例是一个交易平台支持售卖一个页面的所有广告位。 为了便于竞拍者区分， 每一个`Imp`对象都要有一个唯一标识（ID).

`Banner`, `Video`以及`Native`对象都属于`Imp`对象，只是标明了各自的展示类型。 展示者可以选择其中的一种类型或者混合使用多种类型。 但是，对于展示的任何给定的竞价请求必须属于提供的类型之一。

| 属性 | 类型 | 描述 |
| --- | --- | --- |
| id | string;**required** | 在当前竞价请求上下文中唯一标识本次展示的标识（通常从1开始并以此递增)|
| banner | object | `Banner`对象， 如果展示需要以banner的形式提供则需要填充 |
| video | object | `Video`对象， 如果展示需要以视频的形式提供则需要填充 |
| native | object | `Native`对象， 如果展示需要以Native广告的形式提供则需要填充 |
| displaymanager | string | 广告媒体合作伙伴的名字， 用于渲染广告的SDK技术或者播放器（通常是视频或者移动广告）某些广告服务需要根据合作伙伴定制广告代码， 推荐在视频广告或应用广告中填充 |
| displaymanagerver | string | 广告媒体合作伙伴， 用于渲染广告的SDK技术或者播放器（通常是视频或者移动广告）的版本号。 某些广告服务需要根据合作伙伴定制广告代码， 推荐在视频广告或应用广告中填充 |
| instl | integer; default 0 | 1标识广告是插屏或者全屏广告， 0表示不是插屏广告 |
| tagid | string | 某个特定广告位或者广告标签的标识，用于发起竞拍。 为了方便调试问题或者进行买方优化 |
| bidfloor | float; default 0 | 本次展示的最低竞拍价格， 以CPM表示 |
| bidfloorcur | string; default "USD" | ISO-4217规定的字母码表标识的货币类型。 如果交易平台允许，可能与从竞拍者返回的竞价货币不同。|
| secure | integer | b标识展示请求是否需要使用HTTPS加密物料信息以及markup以保证安全， 0标识不需要使用安全链路， 1标识需要使用安全链路， 如果不填充，则表示未知， 可以认为是不需要使用安全链路。|
| iframebuster | string array | 特定交易支持的iframe buster的名字数组 |
| pmp | object | `Pmp`对象， 包含对本次展示生效的任何私有市场交易 |
| ext | object | 特定交易的OpenRTB协议的扩展信息占位符 |

### 3.2.3 Banner

Banner是最常见的展示类型。 虽然“banner”这个名词在其他的场景有很特别的意思， 在这里它可以是包括静态图像， 可扩展的广告单元或者一个在banner中播放的视频在内的很多东西。一组Banner对象可以出现在Video对象中来描述可选择在VAST贵方中定义的附加广告。

在Imp对象中出现Banner表示广告展示类型是一个banner. 同样的展示也可以是一个视频或者Native广告， 只要包含Video对象或者Native对象。然而， 任何为展示给定的竞价请求必须符合提供类型中的一个。

| 属性 | 类型 | 描述 |
| --- | --- | --- |
| w | integer; **recommended** | 展示的宽度，以像素为单位，如果没有指定`wmin`以及`wmax`, 这个值指的就是需要的展示宽度，否则指的是一个期望宽度 |
| h | integer; **recommended** | 展示的高度，以像素为党委，如果没有指定`hmin`以及`hmax`, 这个值指的就是需要的展示高度， 否则指的是一个期望高度 |
| wmax | integer | 展示宽度的最大值，以像素为单位， 如果和`w`一起出现， 则`w`应该被解释为推荐宽度或者期望宽度 |
| wmin | integer | 展示宽度的最小值，以像素为单位， 如果和`w`一起出现， 则`w`应该被解释为推荐宽度或者期望宽度 |
| hmax | integer | 展示高度的最大值，以像素为单位， 如果和`h`一起出现， 则`h`应该被解释为推荐宽度或者期望宽度 |
| hmin | integer | 展示高度的最小值，以像素为单位， 如果和`h`一起出现， 则`h`应该被解释为推荐宽度或者期望宽度 |
| id | string | banner对象的唯一标识。在一个Ad中包含Banner与Video的时候使用。值往往从1开始并依次递增，在依次展示中应当是唯一的 |
| btype | integer array | 限制的banner类型。参考表5.2 |
| battr | integer array | 限制的物料属性， 参考表5.3 |
| pos | integer | 广告在屏幕上的位置，参考表5.4 |
| mimes | string array | 支持的内容mime-type. 常用的mime-type包括application/x-shockwave-flash, image/jpg以及 image/gif. |
| topframe | integer | banner是在顶层frame中而不是iframe中， 0表示不是， 1表示是 |
| expdir | integer array | banner可以扩展的方向，参考表5.5 |
| api | integer array | 本次展示支持的API框架列表， 参考表5.6. 如果一个API没有被显式在列表中指明，则表示不支持|
| ext | object | 特定交易的OpenRTB协议的扩展信息占位符 |

### 3.2.4 Video

这个对象表示一个流式视频展示。 许多属性对于最小可用功能不是必须的，但是为了在需要的时候提供更好的控制能力会被使用。OpenRTB中的视频通常都是与标准一致的，复合广告的概念也是支持的， 可以包含用于定义复合广告的一组Banner对象。

Video作为Imp的子对象出现表示它是一个具有视频类型的展示对象。 同样的展示也可以是一个Banner或者Native广告， 只要包含Banner对象或者Native对象。然而， 任何为展示给定的竞价请求必须符合提供类型中的一个。

| 属性 | 类型 | 描述 |
| --- | --- | --- |
| mimes | string array; **required** | 支持的内容mime-type， 常用的类型包括用于windows媒体的video/x-ms-wmv以及用于Flash视频的video/x-flv. |
| minduration | integer; **recommended** | 最小的视频长度， 以秒为单位 |
| maxduration | integer; **recommended** | 最大的视频长度， 以秒为单位 |
| protocol | integer; **DEPRECATED** | 注意，使用`protocols`是高度推荐的。 支持的视频竞价响应协议。参考表5.8.至少一个支持的协议必须在protocol或者protocols属性中被指定 |
| protocols | integer array; **recommended** | 支持的视频竞价响应协议数组。参考表5.8.至少一个支持的协议必须在protocol或者protocols属性中被指定 |
| w | integer; **recommeded** | 视频播放器的宽度，像素为单位 |
| h | integer; **recommeded** | 视频播放器的高度，像素为单位 |
| startdelay | integer; **recommended** | 视频前，中及之后的广告位中视频广告的启动延时，以秒为单位, 参考表5.10|
| linearity | integer | 展示是否必须是线性的， 如果没有指定，则标识都是被允许的，参考表5.11 |
| sequence | integer | 如果在同一个竞价请求中提供了多个展示， 则需要考虑多个物料传输的顺序 |
| battr | integer array | 限制的物料属性，参考表5.3 |
| maxextended | integer | 最大的视频广告延长时间长度（如果支持延长）。如果为空或者0，表示不允许延长， 如果为-1，表示允许延时，且没有时间限制， 如果为大于0的数字， 则表示可以延长的时间长度比maxduration大的值 |
| minbitrate | integer | 最小的比特率，以Kbps为单位。交易平台可以动态的设置这个值或者为所有发布者统一设置该值 |
| maxbitrate | integer | 最大的比特率，以Kbps为单位。交易平台可以动态的设置这个值或者为所有发布者统一设置该值 |
| boxingallowed | integer; default 1 | 是否允许将4：3的内容展示在16：9的窗口， 0表示不允许，1表示允许 |
| playbackmethod | integer array | 允许的播放方式， 如果没有指定，表示支持全部，参考表5.9 |
| delivery | integer array | 支持的传输方式 （例如流式传输，逐步传输），如果没有指定，表示全部支持，参考表5.13|
| pos | integer | 广告在屏幕上的位置，参考表5.4 |
| companionad | object array | 如果支持复合广告，表示一组Banner对象 |
| api | integer array | 本次展示支持的API框架列表， 参考表5.6. 如果一个API没有被显式在列表中指明，则表示不支持|
| companiontype | integer array | 支持的VAST companion 广告类型， 参考表5.12。 如果在companionad中填充了Banner对象则推荐使用 |
| ext | object | 特定交易的OpenRTB协议的扩展信息占位符 |

### 3.2.5 Native

表示一个Native类型的展示。Native广告单元需要无缝的插入其周围的内容中（例如， 一个对Twitter或Facebook赞助）。 因此，响应必须具有良好的结构， 让展示者能够在细粒度上控制渲染过程。

Native小组委员会为OpenRTB开发了一个组合规范，名为Native Ad规范。 定义了Native广告的请求参数以及响应结构。这个对象以字符串的形式提供请求参数， 这样的话具体的请求参数就可以按照Native Ad规范独立演进。同样的， 广告markup也会按照该文档指定的结构提供。

Native作为Imp的子对象出现表示它是一个具有native类型的展示对象。 同样的展示也可以是一个Banner或者Video广告， 只要包含Banner对象或者Video对象。然而， 任何为展示给定的竞价请求必须符合提供类型中的一个。

| 属性 | 类型 | 描述 |
| --- | --- | --- |
| request | string; **required** | 遵守Native Ad规范的请求体 |
| ver | string; **recommended** | Native Ad规范的版本， 为了高效解析强烈推荐 |
| api | integer array | 本次展示支持的API框架列表， 参考表5.6. 如果一个API没有被显式在列表中指明，则表示不支持|
| battr | integer array | 限制的物料属性，参考表5.3 |
| ext | object | 特定交易的OpenRTB协议的扩展信息占位符 |

### 3.2.6 Site

如果广告载体是一个网站时应该包含这个对象，如果是非浏览器应用时则不需要。 一个竞价请求一定不能同时包含Site对象和App对象。 提供一个站点标识或者页面地址是很有用的， 但是不是严格必须的。
| 属性 | 类型 | 描述 |
| --- | --- | --- |
| id | string; **recommended** | 交易特定的站点标识|
| name | string | 站点名称 （可以在展示者请求中作为别名标识）|
| domain | string | 站点的域名 （例如，"mysite.foo.com") |
| cat | string array | 站点的一组IAB 内容类型，参考表5.1 |
| sectioncat | string array | 描述站点当前部分的一组IAB内容类型，参考表5.1 |
| pagecat | string array | 描述站点当前视图的一组IAB内容类型，参考表5.1 |
| page | string | 展示广告将要被展示的页面地址 |
| ref | string | 引导到当前页面的referrer地址 |
| search | string | 引导到当前页面的搜索字符串 |
| mobile | integer | 移动优化标志， 0表示否，1表示是 |
| privacypolicy | integer | 表示该站点是否有隐私策略， 0表示没有， 1表示有 |
| publisher | Object |`Publisher`对象， 站点发布者的详细信息 |
| content | Object | `Content`对象， 该站点内容的详细信息 |
| keywords | string | 逗号分隔的站点的关键字信息 |
| ext | object | 特定交易的OpenRTB协议的扩展信息占位符 |

### 3.2.7 App

如果广告载体是非浏览器应用（通常是移动设备）时应该包含该对象， 网站则不需要包含。一个竞价请求一定不能同时包含Site对象和App对象。 提供一个App标识或者`bundle`是很有用的， 但是不是严格必须的。

| 属性 | 类型 | 描述 |
| --- | --- | --- |
| id | string; **recommended** | 交易特定的应用标识|
| name | string | 应用名称 （可以在展示者请求中作为别名标识）|
| bundle | string | 应用信息或者包名（例如， com.foo.mygame); 需要是在整个交易过程中唯一的标识 |
| domain | string | 应用的域名（例如， "mygame.foo.com" ) |
| storeurl | string | 应用的商店地址， 遵循AQG 1.5 |
| cat | string array | 应用的IAB内容类型数组， 参考表5.1 |
| sectioncat | string array | 应用当前部分的IAB内容类型数组，参考表5.1 |
| pagecat | string array | 应用当前视图的IAB内容类型数组，参考表5.1 |
| ver | string | 应用版本号 |
| privacypolicy | integer | 表示该应用是否有隐私策略， 0表示没有， 1表示有 |
| paid | integer | 应用是否需要付费， 0表示免费， 1表示付费 |
| publisher | Object |`Publisher`对象， 应用发布者的详细信息 |
| content | Object | `Content`对象， 该应用内容的详细信息 |
| keywords | string | 逗号分隔的应用的关键字信息 |
| ext | object | 特定交易的OpenRTB协议的扩展信息占位符 |

### 3.2.8 Publisher
### 3.2.9 Content
### 3.2.10 Producer
### 3.2.11 Device
### 3.2.12 Geo
### 3.2.13 User
### 3.2.14 Data
### 3.2.15 Segment
### 3.2.16 Regs
### 3.2.17 Pmp
### 3.2.18 Deal

# 4. Bid Response 规范
## 4.1 Object Model
## 4.2 Object Specifications
### 4.2.1 BidResponse
### 4.2.2 SeatBid
### 4.2.3 Bid
## 4.3 Ad Serving Options
### 4.3.1 Markup Served on the Win Notice
### 4.3.2 Markup Served in the Bid
### 4.3.3 Comparison of Ad Serving Approaches
## 4.4 Substitituion Macros

# 5. 规范清单列表
## 5.1 Content Categories
## 5.2 Banner Ad Types
## 5.3 Creative Attributes
## 5.4 Ad Position
## 5.5 Expandable Direction
## 5.6 API Frameworks
## 5.7 Video Linearity
## 5.8 Video Bid Response Protocols
## 5.9 Video Playback Methods
## 5.10 Video Start Delay
## 5.11 Video Quality
## 5.12 VAST Companion Types
## 5.13 Content Delivery Methods
## 5.14 Content Context
## 5.15 QAG Media Rating
## 5.16 Location Type
## 5.17 Device Type
## 5.18 Connection Type
## 5.19 No-Bid Reason Codes

# 6. Bid Request/Response 样例
## 6.1 Github Repository
## 6.2 Validator
## 6.3 Bid Requests
### 6.3.1 Sample Banner
### 6.3.2 Expandable Creative
### 6.3.3 Mobile
### 6.3.4 Video
### 6.3.5 PMP with Direct Deal
### 6.3.6 Native Ad

## 6.4 Bid Responses
### 6.4.1 Ad Served on Win Notice
### 6.4.2 VAST XML Document Returned Inline
### 6.4.3 Direct Deal Ad Served on Win Notice
### 6.4.4 Native Markup Returned Inline

# 7. 实现注意事项
## 7.1 COPPA Regulation Flag
## 7.2 PMP & Direct Deals
## 7.3 No-Bid Signaling

# 附录A. 附加信息
# 附录B. 规范版本更新日志
