#简便快捷：如何在Wwise中管理您的SoundBanks

![](https://github.com/akchina/learnwwisecn/blob/master/QuickAndEasy_HowToManageYourSoundBanksInWwise/images/DifferentSoundBankStrategies.png?raw=true)

您在创建SoundBanks（声音库）时做出的选择会影响到管理它们时的工作量，也会影响到游戏的性能。我们建议声音设计师和程序员一起建立一条最合适你们游戏的策略。

以下是在Wwise中管理SoundBanks几种不同方式的概览。

##用一个SoundBank来管理全部

这样做之后的大部分情况下，您的游戏可能不会有多余的内存，但把所有的声音放在同一个位置有个很大的优势：使用和维护起来非常简单。

优点

* 对声音设计师来说，这是最简单的维护SoundBanks内容的方法 
* 改变SoundBanks的内容时无需重新编译游戏 
* 不需要在游戏内进行复杂的SoundBanks载入和卸载 
* 不需要在游戏内追查声音是否可用

缺点

* 因为整个游戏的所有Events（事件）、结构和需驻留内存的媒体一直保持载入状态，导致内存使用效率较低

##基于关卡/场景的多个完整SoundBanks

这一方法对于单人模式游戏最为适合，因为所有的声音和振动效果（译注：Wwise Motion 插件）都是同一位置驱动的。把内容分割到不同的SoundBanks之中，相比只使用一个SoundBank来说，更有助于高效地管理游戏内存。

优点 

* 一般来说比“一体化”法需要的内存更少 
* 在游戏中做整合会比较简单

缺点 

* 对于有复杂音频或动作要求的在线游戏或基于Event的游戏来说，适应性不佳 
* 可能导致内存中重复载入一些媒体文件的复本 
* 可能增加SoundBanks在磁盘上占用的总空间

##按媒体种类

游戏可能很复杂，多种因素会影响到声音触发的方式。比如，在一个基于Event或者基于对象的环境中，根据听者与游戏中其他对象的距离，在内存中可能载入不同的声音。因此，当游戏中的对象在听觉范围内，或者只要存在，它就可能载入一系列的SoundBanks。

优点 

* 优化内存使用 
* 让您能控制游戏中任何点上应载入哪个媒体


缺点 

* 需要声音设计师和游戏开发人员间做非常多的沟通来确认哪些SoundBank需要载入，何时载入


##动态式

这一方法让您能更好地控制每当Events准备好时（译注：即当游戏预知需要发送某些Events并调用 API 来做好“准备”时）要载入的媒体。只有同时关联到准备好的Events和目前活跃Game Syncs（游戏同步器）的媒体才会载入内存。您要做的只是指定可能会用到的Events和Game Syncs，游戏就会载入合适的媒体。

优点 

* 简化音频包生成流程 
* 实现最细的媒体粒度水平 
* 保持较低的整体内存占用 
* 使流程的自动化更为便捷  
* 只载入有用的媒体

缺点 

* 可能增加在磁盘上的读取和搜索次数 
* 难以控制使用的内存总量
* 在准备很多Events时可能导致流播放带宽较高

##离线解压缩

可以预先解压Vorbis音频文件。这一动作会在增加一点SoundBanks大小的同时，减少触发声音所需的CPU运算时间。它可以结合以上任意一种方法来优化管理您的声音资源。

###文档

[管理SoundBanks](https://www.audiokinetic.com/library/2016.1.0_5775/?source=Help&id=strategies_for_managing_soundbanks)

###视频教程

[Wwise 快速上手-创建SoundBanks](https://www.youtube.com/watch?v=VhQ_nQQT--g)

[Wwise教程15-创建和管理SoundBanks](https://www.youtube.com/watch?v=ldzQvQeWtno)
