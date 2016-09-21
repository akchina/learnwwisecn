# 使用Wwise分析性能，排除故障和调试

Wwise自带的[性能分析、故障排除和调试工具](https://www.audiokinetic.com/library/2016.1.0_5775/?source=Help&id=profiling)是Wwise的诸多强大优势之一；它们帮助Wwise这一软件成为最为完整强大的游戏音频中间件之一。了解如何充分利用这些功能是体会它们益处的关键。

本文会提供一些见解，介绍如何高效的使用Profiler（性能分析器）和Game Object Profiler Layouts（游戏对象性能分析器布局）、Schematic View（对象网络视图），以及其他可以在对游戏音频系统进行故障排除和调试时可以利用的小工具。

## The Profiler Layout（性能分析器布局）

![](https://github.com/akchina/learnwwisecn/blob/master/ProfilingTroubleshootingandDebuggingUsingWwise/images/Picture1-1.png?raw=true)

最显而易见的性能分析工具自然是Profiler Layout。它由三个视图构成：Capture Log（捕获日志），Perfomance Monitor（性能监视器）和Advanced Profiler（高级性能分析器）。它的功能是监视并记录性能、内存使用和所有来自声音引擎的活动。

通过使用Soundcaster Session（声音选角器会话），可以在进行游戏内的声音整合之前就在本地分析性能。这让声音设计师无需依赖游戏的可运行版本就能对音频系统进行性能分析和调试；这一功能在另一方面也非常有用，就是向开发者展示音频能够保持在既定的内存预算范围内，在整合后不会压垮游戏。当音频加入后，您也可以一边玩游戏一边进行实时性能分析。这项功能十分的好用，您可以确认游戏引擎能否正确发出音频事件，而无需在Soundcaster（声音选角器）内手动触发事件。

它左上角的视图-Capture Log-记录了一切来自声音引擎的活动。但这会产生大量的信息，所以我强烈建议用Capture Log Filter（捕获日志筛选器）根据您的需求来选取感兴趣的部分，比如Events（事件），Actions（动作），States（状态）和Switches（切换开关）。

**小技巧：**我有很多次用Capture Log的时候不是为了调试纠错，而是为了更精准地进行设计。比如，当我知道一个音频事件是在一段游戏内动画的开端触发时，我会用Wwise远程连接到游戏，播放并触发这一动画，并在Capture Log中记录下动画开始和结束的精确时间。我会把整个过程录像，这样在DAW内进行设计的时候就能使用这段视频，并且清晰地看到对应游戏内的时间我应何时开始播放这段动画的声音。

Performance Monitor视图能够展示CPU、流播放和内存使用方面的性能信息。当捕获开始时，它会实时显示每一个声音引擎进行的活动的数据。它对于识别性能中的峰值以及与峰值关联的活动是十分有用的。

**小技巧：**通过在Performance Monitor Settings（性能监视器设置）中设置最大值和最小值，任何超出设定最大值的数值都会显示为一个实心块。这也让用户能很轻松快捷地根据显示的数值（比如声部数目，CPU，或者内存使用）找到音频在哪里超出了最大预算。接着您可以针对某一张图来扩大最大最小值之间的范围，以便针对有问题的区域得到更详细的信息。

**小技巧：**拖拽Performance Monitor Time Cursor（性能监视器时间游标）来精确观察右下角显示的对应值的变化。

在Advanced Profiler视图中，您能看到Log（日志）和Performance Monitor中捕获内容的详细信息。只要您找到问题的位置，对照这些信息去了解具体出现什么问题是非常方便的。在游戏开发中，您很可能在空间、性能和内存使用上都有仔细制定的预算。遵守既定的预算数字是无比重要的，而Advanced Profiler能够保证您做到这一点，同时能提供精确、实时的信息，让您知道正在播放的声部数目，活跃的总线数目，内存使用，流播放，等等。而且，Voice Graph（声部图）选项卡能显示Schematic View的实时版，这样就能更轻松地在不同工程对象层级或者在Game Objects（游戏对象）设置中找到错误。

**小技巧：**在使用Asset Bundling（资源打包）时，需要知道载入的是哪一个SoundBank（音频包），因为每一个SoundBank都很可能指向一个Asset Bundle（资源包）。SoundBank载入时间不正确可能表明Asset Bundle有问题。

**小技巧：**为了得到更准确的性能分析数据，可以使用Profiler Settings（性能分析器设置）对话框来筛选想要捕获的信息。这样就能提升Wwise的Profiler Performance （分析器性能），节约CPU时间，不论是在游戏中还是在Wwise内。

**小技巧：**您可以将一个性能分析会话保存为文件，稍后查看，也可以将文件共享给另一个开发者，以便进行分析。

## Game Object Profiler Layouts

![](https://github.com/akchina/learnwwisecn/blob/master/ProfilingTroubleshootingandDebuggingUsingWwise/images/Picture2-1.png?raw=true)

Game Object Profiler Layouts使用起来没有那么直观，但是在分析游戏中的Game Objects和Listeners（听者）的实时信息时，它还是非常有用的。它由三个视图构成：Game Object Explorer（游戏对象管理器），Game Sync Monitor（游戏同步器监视器）,以及Game Object 3D Viewer（游戏对象3D查看器）。这一布局要在音频已整合到游戏中后使用，使用时需运行游戏并实时连接到游戏中。

就像Profiler Layout一样，它也会分析声音引擎的输出，但它不是从声音引擎本身及其活动的角度，而是通过每个独立的游戏对象进行分析。使用Game Object Profiler Layout（游戏对象性能分析器），您可以分析任何在游戏中发声的对象并且确保它们的表现和设想的一致。

我认为它最擅长的有：

- 显示每个对象的衰减半径以及它们与其他对象间的相互作用；
- 测试移动对象的3D声音设置以及它们与其他对象间的相互作用；
- 监控并分析RTPC（实时参数控制）值的实时变化以及它们对于总体音景的影响；
- 识别所有Game Objects和Listeners，保证应有的元素都存在。

换句话说，我觉得Profiler Layout主要关乎性能，而Game Object Profiler Layouts主要用于游戏的音频机制和保证每件事都按照引擎中的设置进行。它提供了一个途径，让我们能监控在游戏内运转的完整音景，同时不光通过耳听，也能依靠实时数据来进行调试。

Game Object Explorer视图列出了所有由程序员注册的发声Game Objects。您可以选择要观察的对象，方法是右击列表中的任意对象。这些被观察的对象就会出现在Game Object 3D Viewer中，在这里您能观察它们在游戏中是如何表现的。游戏同步监视器会用图形显示任何活跃的RTPC以及它们的实时数值。这三个视图结合在一起，就能让您更轻松地找到声音中哪些数值是不满意的，也可以让您知道RTPC或者其他3D声音设置的实际行为和设计相比的偏差。

## Schematic View

![](https://github.com/akchina/learnwwisecn/blob/master/ProfilingTroubleshootingandDebuggingUsingWwise/images/Picture3-1.png?raw=true)


Schematic View主要的目的不是调试，但是用来查看工程中复杂的对象层级结构是非常方便的，它也可以用来确保合适的路径能够根据正确的条件和设置触发。

它的主打特性是非常直接的工作流程。把任何组件从Project Explorer（工程管理器）中拖拽到Schematic View，它的图像就会显示出来，再点击“+”图标就会展开下一层。

它真正好用的地方就是让您能直接在Schematic View上播放任何对象，这样它就不只是一个游戏音频内容的详细图解，而同时也成为了一种互动型编辑器和混音器：

- 在Schematic View中双击任何对象，您可以无需切换到Designer Layout（设计师布局）就获取对象的属性。同样的，双击一个层级底端的任何声音文件，您可以进入Source Editor（源编辑器）窗口并直接对文件进行基本的编辑。右键菜单的选项也非常方便，比如Find in Project Explorer（在工程管理器中查找）等，都能显著地提高工作流程的速度。
- 通过自定义Schematic View Settings（对象网络设置），您可以显示任何对象的属性，而且可以在播放的过程中调整它的设置，比如音量，音高，滤波器，静音/Solo，等等。通过同时激活属性，您能简单地将Schematic View变为一个相当完整的混音器。比如，我喜欢使用音量，Icon Strips（图标条），静音/Solo和3D定位来迅速有效地控制每个对象的属性，同时监视它在音频管线中的位置。

**小技巧：**使用Schematic View Search box（对象网络视图搜索框）来快速定位对象，或者用General Search box（普通搜索框）（如果对象还没有位于Schematic View中）来寻找对象并直接在Schematic View中打开。

最后，当一个工程达到一定规模时，想要严丝合缝地追溯所有工程对象层级中的组件和对象就可能变得困难，如果您之前尝试过多版本声音和Events就更是如此。Event Viewer（事件查看器）可以非常方便地查看选中的对象在哪些事件中被引用。我建议每过一段时间就检查一遍您的工程对象层级结构，保证里面没有不需要的对象，因为留着它们就有可能会被错误地加入SoundBank里来载入了，这样就会多占用空间，而这些空间本来是可以给真正需要的资源来用的。同样的，不要低估Query（查询）选项卡和Query Editor（查询编辑器），它们能让您在工程中定位任何Object Type（对象类型），比如哪些总线使用了Plugins（插件）或Effects（效果器）。

如果不提到Soundcaster Session肯定也是不对的，因为它本身就是一个测试和调试工具，您几乎可以在这里复现游戏的全部或者一部分，聚焦音频内容。Shift+S的快捷键（显示Soundcaster）绝对是您应该最早记住的快捷键之一，因为这个工具让您能听到各种音频组件的互相作用，并确保您的States、Switches、RTPC和音乐触发器是关联到了正确Events的。

调试愉快：）

![](https://github.com/akchina/learnwwisecn/blob/master/ProfilingTroubleshootingandDebuggingUsingWwise/images/Anne-Sophie_Mongeau_Photo.jpg?raw=true)


### ANNE-SOPHIE MONGEAU

**游戏音频工程师 - DIGIT Game Studios**

Anne-Sophie目前在DIGIT Game Studios担任游戏音频工程师，在都柏林工作。她从2012年开始从事游戏相关工作，帮助独立作品和3A大作进行声音设计和整合。她在音乐研究方面的背景，尤其是在爱丁堡大学的经历，也引领着她参与了很多不同的线性媒体和互动媒体的项目，包括电影短片，纪录片，以及交互式视听作品。分享知识的热情也驱使着Anne-Sophie经常面向大学生组织实践研讨会和大师课，介绍如何使用游戏音频方面的工具。
