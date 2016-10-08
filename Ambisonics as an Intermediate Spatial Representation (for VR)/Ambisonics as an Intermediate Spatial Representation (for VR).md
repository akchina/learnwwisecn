# Ambisonics作为空间音频的中介表示法（针对VR）

很久以来，“Ambisonics（声场环绕声）”总是被认为是一种创造环境声或者用特殊麦克风录下音频场景、捕获声音空间效果层面的方式。

如今，我们把Ambisonics作为一种空间音频的中介表示法（intermediate spatial representation），它能针对虚拟现实（VR）进行双耳声渲染，同时避免纯粹基于对象音频的缺点。

## 为双耳化服务的空间音频中介表示法

VR游戏需要为耳机进行双耳声（binaural）混音渲染，方式是让声音通过与位置相关的、称作Head-Related Tansfer Functions（头部相关传输函数，HRTF）的滤波器。这些滤波器会模拟声音和头部之间的相互作用，同时能成功地制造出游戏的声音来自于玩家头部之外的印象。

为了应用合适的HRTFs，双耳声处理需要声源传来的方向。有一种实现方式就是独立滤波每个位置的声源。然而每个声源独立进行声像平移和双耳声滤波是有缺陷的（[见我们之前在Ambisonics方面的博客](http://blog.audiokinetic.com/working-with-object-based-audio)）。尤其是它让声音设计师无法对声音分组并做子混音以便应用音频Effects（效果器），比如动态范围压缩。为了弥补这些缺陷，我们需要将声像平移阶段和双耳声处理阶段分离开来。在这两个阶段之间，我们需要一种信号格式来携带信息，告诉我们组成子混音音频的这些声音的空间属性。我们把这种格式称作空间音频的中介表示法，下图中的问号就代表这种格式。

[](https://github.com/akchina/learnwwisecn/blob/master/Ambisonics%20as%20an%20Intermediate%20Spatial%20Representation%20(for%20VR)/images/Screen_Shot_2016-08-01_at_9.41.42_AM.png?raw=true)

*图1-自由地对对象进行子混音，并将其加入一个多声道空间音频表示法之中，这个表示法能够传达出其所有成分的方向密度，同时也可以通过添加（比如）Effects来操纵这一表示。*

相比于通过HRTF处理得到的双耳声信号（该信号也携带空间信息），空间音频的中介表示法是可操纵的；可以对它进行交换和母带处理，而且它某种程度上与渲染环境（扬声器设置，或者HRTF设置）无关。

## “虚拟扬声器”表示法

合适的空间音频的中介表示法会使用虚拟扬声器。虚拟扬声器的概念很简单。它由一个多声道格式组成，这个格式中每个声道都代表一个位置固定且位置已知的虚拟扬声器。于是声音就根据标准声像平移法则进行声像平移并被混音到这个多声道格式中。之后，每个声道的信号将会由相关联的虚拟扬声器对应的HRTF分别滤波。

![](https://github.com/akchina/learnwwisecn/blob/master/Ambisonics%20as%20an%20Intermediate%20Spatial%20Representation%20(for%20VR)/images/Screen_Shot_2016-08-01_at_9.44.13_AM.png?raw=true)

*图2-这是虚拟扬声器表示法，其中每个声道都被描述为环绕听者（如第一人称射击的镜头，或第三人称射击的主角位置）周围的虚拟扬声器。在VR游戏中，每个虚拟扬声器的信号都会通过与其位置（固定）相对应的HRTF，并传给耳机混音的每只耳朵。*

实际上，虚拟扬声器表示法和经典的多声道信号没什么不同。唯一的区别在于它的使用环境。当针对传统的5.1设置混音时，声音会直接被声像平移并混音到5.1总线上，而且这条总线上的每一个声道都被用来驱动一个现实中的扬声器。但是，在我们的例子中，我们进行声像平移时是刻意将声音放在一个特殊设置中，声道是比输出多的（输出只有双声道）。在虚拟扬声器声道中隐含的方向信息就通过滤波的方式嵌入了双耳声信号中。

在Wwise里，您可以将3D声音的通路连接到到一条专门负责空间音频的中介表示法的音频总线上，而且可以让它的父级总线进行双耳声渲染。在这种情况下，后者有双耳声Effect，前者有母带处理Effects（如果需要）。

[![](https://github.com/akchina/learnwwisecn/blob/master/Ambisonics%20as%20an%20Intermediate%20Spatial%20Representation%20(for%20VR)/images/Screen_Shot_2016-08-02_at_10.29.25_AM.png?raw=true)](https://github.com/akchina/learnwwisecn/blob/master/Ambisonics%20as%20an%20Intermediate%20Spatial%20Representation%20(for%20VR)/images/AMB-screenshot-fixedobject.png?raw=true)
[![](https://github.com/akchina/learnwwisecn/blob/master/Ambisonics%20as%20an%20Intermediate%20Spatial%20Representation%20(for%20VR)/images/Screen_Shot_2016-08-02_at_10.29.25_AM.png?raw=true)](https://github.com/akchina/learnwwisecn/blob/master/Ambisonics%20as%20an%20Intermediate%20Spatial%20Representation%20(for%20VR)/images/AMB-screenshot-ISR-fixedobject.png?raw=true)

*图3-在Wwise中使用虚拟扬声器表示法，如Project Explorer（工程管理器）和Schematic View（对象网络视图）所示。两条总线都设成带有多扬声器的标准设置，比如7.1.4。为了要表示来自于听者上方的声音，Height（高度）声道也是需要的*


## Ambisonics

我们可以把Ambisonics视为另一个空间音频的中介表示法。每个声道都携带所谓球谐函数，而且它们会一起模拟出一个声场。一阶Ambisonics由4个声道组成，W，X，Y和Z（在 FuMa convention中）。W叫做omni声道，它携带着方向无关的信号。X，Y和Z携带着在三个轴上（前后，左右和上下）与方向相关的音频。

高阶Ambisonics（HOA）就有些难以说明，但是我们可以把高阶解释为携带额外的数据，以便改善空间精度。二阶和三阶Ambisonics分别由9个和16个声道组成，因此也更为精确。

![](https://github.com/akchina/learnwwisecn/blob/master/Ambisonics%20as%20an%20Intermediate%20Spatial%20Representation%20(for%20VR)/images/Screen_Shot_2016-08-01_at_9.51.01_AM.png?raw=true)

*图4-分别由一阶Ambisonics（左）和三阶Ambisonics（右）表示的点声源。色温和信号能量成正比。
在Ambisonics术语中，声像平移到Ambisonics对应编码，而将Ambisonics转码为双耳声设置或者扬声器馈送就对应解码。*

和像7.1.4设置这种典型的虚拟扬声器表示法不同，Ambisonics在所有方向都是对称和规律的，而且可以表示来自听者下方的声源。但是，Ambisonics比拥有相同数目声道的虚拟扬声器更模糊。Ambisonics的散布是恒定的，而且在虚拟声源正好位于一个扬声器上时，振幅平移会更为精确。但如果声源正好位于三个环绕扬声器中间时就不那么精确了。

在Wwise v2016.1中将Ambisonics实现为空间音频的中介表示法和用虚拟扬声器法一样简单。只要将总线设成Ambisonics配置既可。我们建议使用更高阶的Ambisonics以获得令人满意的空间精度。


[![](https://github.com/akchina/learnwwisecn/blob/master/Ambisonics%20as%20an%20Intermediate%20Spatial%20Representation%20(for%20VR)/images/Screen_Shot_2016-08-02_at_10.33.58_AM.png?raw=true)](https://github.com/akchina/learnwwisecn/blob/master/Ambisonics%20as%20an%20Intermediate%20Spatial%20Representation%20(for%20VR)/images/AMB2_-screenshot-ambisonics.png?raw=true)
[![](https://github.com/akchina/learnwwisecn/blob/master/Ambisonics%20as%20an%20Intermediate%20Spatial%20Representation%20(for%20VR)/images/Screen_Shot_2016-08-02_at_10.33.58_AM.png?raw=true)](https://github.com/akchina/learnwwisecn/blob/master/Ambisonics%20as%20an%20Intermediate%20Spatial%20Representation%20(for%20VR)/images/AMB2-screenshot-ISR-ambisonics.png?raw=true)

*图5-在Wwise中使用空间音频的中介表示法，如Project Explorer和Schematic View所示。两条总线都设成了Ambisonics配置。我们建议使用更高阶的Ambisonics以获得令人满意的空间精度。
*

目前，有些VR供应商已经提供可接受Ambisonics信号并进行双耳声虚拟化的SDKs。随着它们的出现，Wwise也能直接将Ambisonics输送至这些设备上。其他情况下，它也能使用您最喜欢的3D音频插件解码Ambisonics并转码为双耳声。自带Wwise的Auro Headphone就是一个例子。

最后，在工作流程中整合空间音频的中介表示法并通过Ambisonics来做的另一个好处就是它能作为一种交换格式。

## VR之外的Ambisonics

我们最近在iX Symposiumq期间在SAT球幕影院上尝试了同样的工作流程。我们没有把Ambisonics总线解码为双耳声馈送，而是使用Wwise默认算法直接解码到圆顶的扬声器组合上，这个扬声器组合一共有31组扬声器。

我们和听众们做了一个有趣的实验，对比了一架直升机从我们头上飞过时我们采用三种不同空间化策略的声音结果:

* 直接声像平移到31组扬声器；
* 解码到三阶Ambisonics，再解码到31组扬声器；
* 解码到一阶Ambisonics，再解码到31组扬声器。

虽然一阶Ambisonics带来了更为“广阔”的声像，但大部分参与者还是同意三阶Ambisonics在精确度上能和直接声像平移媲美。

在空间音频博客系列的下一篇，我们会探寻Ambisonics管线如何被用于电影VR。所以，敬请期待关于Ambisonics管线优势和使用上的新帖子。

##[进入Audiokinetic Ambisonics管线内容概览](http://cta-service-cms2.hubspot.com/ctas/v2/public/cs/c/?cta_guid=efdd1440-4633-4b15-af65-0f41a1b7ecc8&placement_guid=add2a042-826e-4297-bd15-381212f1192a&portal_id=1940263&redirect_url=APefjpEuqIRh1mvuHErF_SN_Ux_a4A8cN78CPfl4azsOUGIJAgWDwJUjqHYrt44hQJUbRmpHxYk9_CdkuQ0u3MHZumHuFsSMlfwWs5nN4rMlpgbLpkxdwLvDKG5-NA4xarutBglzS5zL3ic7VuseIrymBRSzFnplZbE-neFmJhIA-Ao4wO49FgGyWCu_qN50yZ3OX4REEfqiSMN7BasnP148VbwDFdroRt-QQPihMlpBqpiuTSnX2rt4yIOhWDLpMReWMDi6UXmsbtBeT81ptkfSBpCdKexzm6Ah9Z7vDJIjjfb2FBdeG8s&hsutk=e9b879c3cbe940b558fad4e69db59f76&utm_referrer=http%3A%2F%2Fblog.audiokinetic.com%2Fambisonics-as-an-intermediate-spatial-representation-for-vr&canon=http%3A%2F%2Fblog.audiokinetic.com%2Fambisonics-as-an-intermediate-spatial-representation-for-vr&__hstc=170909823.e9b879c3cbe940b558fad4e69db59f76.1465437682329.1475892260865.1475918243850.205&__hssc=170909823.9502.1475918243850&__hsfp=1874498977)

![](https://github.com/akchina/learnwwisecn/blob/master/Ambisonics%20as%20an%20Intermediate%20Spatial%20Representation%20(for%20VR)/images/Xavier.png?raw=true)

**路易斯-扎威尔 布冯尼**
Audiokinetic 研发总监

扎威尔·布冯尼领导Audiokinetic的空间音频研发。他有电子工程的硕士学位，并折衷音频知识传播，不时撰写互动音乐、HDR动态混音方面的文章并开展演讲活动，最近也开始触及基于对象的声像平移。扎威尔在AES游戏大会、MIGS等业界会议上做过演讲。
