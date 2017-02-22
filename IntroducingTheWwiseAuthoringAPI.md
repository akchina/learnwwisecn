# 隆重推出 Wwise Authoring API

![](http://info.audiokinetic.com/hubfs/Blog.png)

大家知道 Wwise 有一套音频引擎 API，但是如果 Wwise 设计工具也有一套 API 能让外部应用程序跟它对话不是会更好一些吗？

Wwise Authoring API（Wwise 设计工具应用编程接口）就是用来干这个的！你知道 Wwise 中的这套 API 已经有些年头了吗？没错！它的前身叫作 SoundFrame，但其功能有限而且不太好用；现在情况大大改善了。

我们在 Wwise 2017.1 版中重新设计了 Wwise Authoring API，着重突破原来的局限性并把它提升到了新的高度。本文率先挖掘一下 Wwise Authoring API 到底能用来干些什么事，然后具体讨论一下这次全新版本支持的各种应用场景。

## Wwise Authoring API 能做些什么

Wwise Authoring API 的功能分为两层。

### Wwise 核心层

Wwise core（Wwise 核心）是 Wwise 的心脏。你可以用核心层来操控 Wwise 的数据模型并执行类似如下的任务：

- 导入音频文件；
- 生成 SoundBank；
- 对音频文件做转码；
- 获取性能分析器的数据；
- 播放 Wwise 对象。

### Wwise 用户界面层

User Interface layer（用户界面层）可以用来获取和控制用户界面。

比如，你可以执行下列操作：

- 打开视图；
- 获取当前选中项并更改之；
- 察看对象；
- 还有很多事情可做 ...

这两层中的所有这些功能有广泛的应用前景。我们一个个地来看看。

## Wwise Unrela／Unity 集成

可能 Wwise Authoring API 最明显的应用场合就是将 Wwise Unity 或 Unreal 集成和 Wwise 连接起来。直到 Wwise 2016.2 版，这几方只是通过文件系统相连的。Wwise 和引擎集成之间的通信是单向的；集成从 Wwise 工程文件夹中读取 Wwise 的 Work Unit。集成不可能对工程做出改动。

如果能在 Unity 或 Unreal 编辑器里导入新声音和新的容器，创建新 Event 和 SoundBank，设置音量电平或者其它属性，更改 Attenuation，... 就太好了，对吧？用户在使用引擎集成的过程中想要直接支持的功能要列出来一定很长很长。现在想象一下，每当你想要更改 Wwise 工程中什么东西的时候，不用从游戏引擎切换到 Wwise 设计工具时会是什么样子！

准备好了吗？我们甚至自己做了一点集成示例打算在 GDC17 上秀给大家看呢。来 Audiokinetic 的 GDC 展台现场看看我们自己用 Wwise Authoring API 整的 Unreal 集成吧！

## 自动化

自动化处理可以通过编程来执行大量操作，做到批量高效。可以取代重复性的手工任务。比如，你可以通过程序创建完整的工程或者只创建少数几个对象。你还可以导入音频文件，创建 Wwise 声音和容器，指派总线并设置属性，比如音量。以上全部都是通过  Wwise Authoring API 连接到 Wwise 设计工具实时进行。

这种自动化操作可以集成到你自己的工具中去：你的本命 DAW，你的录音管线，你的素材管理工具 ... 随你喜欢。

## 用移动设备遥控

另一个有趣的应用是用平板或者手机这样的移动设备来遥控 Wwise。比如，这些设备可以实现 Transport Control，用来控制对象的播放，或者实现一个花哨的 X-Y pad 来用一根手指来控制两个 Game Parameter？你需要用平板来做一块远程调音台吗？又或者你想在手机上显示响度表？大胆开脑洞吧。

## 实现自定义视图

Wwise 已经自带了很多视图，但也许对你来说还差点什么？你是不是总想给 Event 实现一个时间线编辑器视图呢？或者也许你想收集性能分析数据和某些对象的播放统计数据，比如始终没有播放和播放太频繁的对象？你想给 Wwise 的参数均衡器效果器实现一个图形编辑器吗？

想象一下，你现在有能力拿到 Wwise 内部的数据，而且还可以更改这些数据。你是不是已经有好多创意可以分享了呢？


## 没有编程语言限制

我们仔细设计了新的 Wwise Authoring API，做到了不吊死在某门编程语言或者某款操作系统上。你其实可以用任何支持 Internet 的语言来调用 API，包括 C++、JavaScript、Python 和 C#，而且支持任何操作系统。你甚至可以在 Web 浏览器里使用它，浏览器如今是最跨平台的环境了。

唯一的限制是你的想象力。请继续关注 Wwise Authoring API，最近有机会来 GDC17 的话，可以随时来问我们问题。



[点我参加 GDC17 活动](https://cta-service-cms2.hubspot.com/ctas/v2/public/cs/c/?cta_guid=18e05218-0ee9-4f54-89fc-8fe543148bb0&placement_guid=ad2c033a-da82-4250-88b9-10f6dfc50a73&portal_id=1940263&redirect_url=APefjpHpHoVJuCdzfUXNAsVa5n1r5yseW7wWc0sokxXS11p4QwBeuo9yz7vnFBT9z8tpsF-75soZ8gaUw8I-1whXPv8Ti9Yg1KKgKc7VRr3rd4eBJUBy89PBBX58sgelo8CXl8kOaDx7tlY-_K5ftMs1-wFhui7jRpGiz128AmWT1TsGC3kIjeAxZ1DQ_TvEXHJmKmxLdDdT08tKJCIjS13bYqCAQzNCXtwt2NIu2CW1YIskLGhx687wz1EIldckP7ZGpIiH4TpG4mwECRG90y8ieJBSw9z3tQ&hsutk=c1106935c58e1f51be8e04bf173386d6&canon=https%3A%2F%2Fblog.audiokinetic.com%2Fintroducing-the-wwise-authoring-api%2F&utm_referrer=https%3A%2F%2Fblog.audiokinetic.com%2Fen%2F&__hstc=170909823.c1106935c58e1f51be8e04bf173386d6.1454336431772.1487076912294.1487710366310.185&__hssc=170909823.3.1487710366310&__hsfp=3974155380)
