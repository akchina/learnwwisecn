# 使用Wwise/Unreal Engine 4/Unity 3D进行脚步材质管理

在前期制作之初，声音设计师需要进行很多系统的原型设定，而且不是总有音频程序员帮忙。

幸运的是，Wwise 结合现今的游戏引擎，比如 Unreal Engine 4 和 Unity 3D，能为此提供很大帮助。

比如，Unreal Engine 4有非常强大的脚本界面，让声音设计师无需程序员的参与也能做很多事。再加上 Wwise 提供的强大功能，如果不用这些超棒的工具就太可惜啦！

接下来，本教程会解释如何检测/管理地面材质并播放合适的脚步声。

主要内容和重点在于：

+ Audiokinetic Wwise 2016.1：Switch Group（切换开关组）、 RTPC、RTPC 控制的 Switch（切换开关），以及 Random Container（随机容器）
+ Unreal Engine 4.12.5：Blueprint、Surface & Physical Materials（表面和物理材质）、AkEvent Notify、Set Switch，以及 Set RTPC
+ Unity 3D 5.1
+ Unreal 和 Unity 商店的免费素材

# Wwise

Unreal 和 Unity 可以针对引擎检测到的地面材质来播放脚步声，而且两个引擎中这个功能的设置基本是一样的。

**Game Syncs（游戏同步器）**

+ 在 Game Sync 选项卡中创建一个新的 Switch Group 并命名为 *Material（材质）*。
+ 在这个 Switch Group 下创建 4 个 Switch 并分别命名为 *Concrete（混凝土）*、*Grass（草地）*、*Tiles（瓷砖）*和 *Wood（木头）*。

![](http://info.audiokinetic.com/hubfs/Picture1-2.png)

*请记住，Switch 是“基于游戏对象”的，也就是说它可以用在不同的场合，而不仅仅是针对玩家的脚步。根据游戏对象不同，具体的 Switch 也可以不同，比如子弹击中和 NPC 脚步。这意味着“全局（global）”的 State（状态）不能用来管理物理材质。*

**Global Setup（全局设置）（Audio/Events/Switch Assignation（音频/事件/切换开关分配））**

![](http://info.audiokinetic.com/hubfs/Picture2-2.png)

+ (1) 创建一个新的 Switch Container 并命名为 *FS\_Pawn*。
+ (2) 对应每种材质，创建 4 个 Random Container：*Walk\_Concrete*、*Walk\_Grass*、*Walk\_Tiles* 和 *Walk\_Wood*。
+ (3) 根据这些容器和材质导入所有的音频文件。
+ (4) 点击 Switch Container 并为其设置之前创建的 Switch：
                                 
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;Group: *Material*（组别：*材质*）    
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;Default Switch/State: *Concrete*（默认切换开关/状态：*混凝土*）

![](http://info.audiokinetic.com/hubfs/Picture3-2.png)

<font size=2>*设置默认值意味着如果引擎发送了一个 Wwise 中没有的材质名，就会默认播放 Concrete（如设置为“无”，则不会播放任何声音）。*</font>

+ (5) 下面在 Assigned Objects（分配的对象） 里，从 Contents Editor（内容编辑器）（或 Project Explorer（工程管理器））中拖拽各 Random Container 到对应的 Switch。 

![](http://info.audiokinetic.com/hubfs/Picture4.png)

+ (6) 为 Switch Container *FS\_Pawn* 创建 New Event Play（新播放事件）并命名为 *Play\_FS\_Pawn*。

+ (7) 您可以在 Transport Control（播放控制）或 Soundcaster Session（声音选角器会话）里测试 Event 并切换 Material。

# Unreal Engine 4

Project Settings（工程设置）

+ (1) 打开 Project Settings 并前往 Engine（引擎）> Physics（物理）> Physical Surface（物理表面）。
+ (2) 添加 4 个新的 Surface Types（表面类型）（使用和 Wwise 里一样的名字）： *Concrete*、*Grass*、*Wood* 和 *Tiles*。

![](http://info.audiokinetic.com/hubfs/Picture5.png)

Physical Material Settings（物理材质设置）

+ (1) 在内容浏览器里创建 4 个新的 Physical Material 并命名为 *PM\_Concrete*、*PM\_Grass*、*PM\_Tiles* 和 *PM\_Wood*。

![](http://info.audiokinetic.com/hubfs/Picture6.png)

+ (2) 分别打开它们并指派相应的 Surface Type。

![](http://info.audiokinetic.com/hubfs/Picture7.png)

Material Settings（材质设置）

+ 打开您地图中使用的每个 Material 并为它们设置正确的 Physical Material：*PM\_Concrete*、*PM\_Grass*、*PM\_Tiles* 和 *PM\_Wood*。

![](http://info.audiokinetic.com/hubfs/Picture8.png)

Map Overview（地图概览）

![](http://info.audiokinetic.com/hubfs/Picture9.png)

Animation Sequence（动画序列）

+ 打开 Animation Sequence，我们要在这里触发 AkEvent 脚步。

![](http://info.audiokinetic.com/hubfs/Picture10.png)

+ (1) 将 AkEvent 通知添加到时间轴上您想要的位置。
+ (2) 在 Anim Notify 中，选择 在Wwise 工程中建立的 Wwise AkEvent *Play\_FS\_Pawn*。

<font size=2>*这时如果播放动画，就会听到默认 Switch 即 Concrete 的脚步声。*</font>

Blueprint

+ Unreal 会在 Blueprint 中“识别” pawn（ Unreal 中的人工智能游戏对象）行走时踩到的不同材质，并向 Wwise 发送正确的 Switch。
+ 在内容浏览器中，创建一个新的 Blueprint Class（Actor Component Class）并将其命名为 *BP\_Wwise\_Footsteps*。

![](http://info.audiokinetic.com/hubfs/Picture11.png)

+ 打开 Blueprint 并撰写脚本。完成后，它看起来应该是这样！

![](http://info.audiokinetic.com/hubfs/Picture12.png)

让我们来仔细看看脚本的各个部分：

+ (1) 该 Blueprint 是基于 tick（时间单位）的，而且它会获取玩家 pawn 的位置。

![](http://info.audiokinetic.com/hubfs/Picture13.png)

+ (2) **LineTraceByChannel** 节点基本就是基于从始到末输入值（ Z 值要根据 pawn 进行调整）的射线追踪 。您可以把 Draw Debug Type（调用调试类型）改成 “For One Frame（针对每帧）”，即可在游戏中将光线追踪可视化；在 pawn 下方会显示一条伸向地面的红线，如果追踪射线碰到了地面，在下方还会显示一个方框。

![](http://info.audiokinetic.com/hubfs/Picture14.png)

+ (3) 第一个 **Branch** 节点用来检测 **LineTraceByChannel** 是否有返回值。

在 Pawn 行走的 Surface Type（表面类型）没有更新时，第二个 Branch 节点用来避免 Blueprint 将 Switch 发送给 Wwise。没有这个门限的话，Wwise 将一直保持每一 tick 都收到 Switch！

**Get Surface Type（获取表面类型）**节点会返回一个和 Project Settings（工程设置）相关联的 Enum（枚举）。

为了获取并设定这一变量，要创建一个名为 CurrentSurfaceType 的变量并将其类型设为 *EPhysical Surface*。

![](http://info.audiokinetic.com/hubfs/Picture15.png)

+ (4) 就像之前说的，Wwise Switch 是“基于游戏对象”的。在这个例子中，发送到 Animation Sequence 里的 AkEvent 通知会创建一个关联到骨骼的 Ak Component，所以我们需要在这个Ak Component 上设置 Set Switch，而不是为 pawn 本身设置。

![](http://info.audiokinetic.com/hubfs/Picture16.png)

+ (5) 这个节点会返回 Surface Type Enum（表面类型枚举）中的相应字符串（*Concrete*,*Grass*, *…*）。
+ (6) 这时，我们就能将信息发送给 Set Switch节点了，它会向 Wwise 发送 pawn 行走时所踩的 Current Surface Type（当前表面类型）！

&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;目标：从 AkEvent 所在的骨骼获取 Ak Component    
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;Switch Group：材质（跟 Wwise 中一样）    
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;Switch State：在 Unreal 中定义的 Surface Type

&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;Print String（打印字符串）节点仅用作调试，可以在 Unreal Screen 上显示 Current Surface。 

![](http://info.audiokinetic.com/hubfs/Picture17.png)

+ 编译 Blueprint 并保存，然后将其作为组件添加到您的 pawn 即可。

![](http://info.audiokinetic.com/hubfs/Picture18.png)

+ 将 Wwise 连接到 Unreal，在 Profiler layout（性能分析器布局）的 Capture log（捕获日志）里就会显示 Switch 了。

![](http://info.audiokinetic.com/hubfs/Picture19.png)

# 更进一步

Wwise 提供的超酷功能之一就是能让 Switch 完全由 RTPC 控制，也就是说 Switch 能根据发送给 RTPC 的参数值而改变！

在本例中，这个功能非常棒，因为我们将用它来根据 pawn 的速度在 Walk（行走）和 Run（奔跑）间切换脚步声。

在 UE4 中，打开 Pawn’s Animation Blueprint 并在 “Update Animation Event（更新动画事件）”（基于 tick ）的后面添加一个 Set RTPC 节点。

+ 目标：我们要获取 Ak Event Notify所在的 Ak Component。
+ RTPC：*Pawn\_Speed*
+ Value：Get Speed（获取速度）（基于速率）

![](http://info.audiokinetic.com/hubfs/Picture20.png)

创建一个由 RTPC 控制的 Switch

+ 创建一个名为 *FS\_Type* 的 Switch Group，将 *Walk* 和 *Run* 作为其中的 Switch。
+ 创建一个名为 *Pawn\_Speed* 的 RTPC。

&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;Min（最小值）= 0（pawn静止）

&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;Max（最大值）= 300（最大速度）

&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;Default（默认值）= 0

![](http://info.audiokinetic.com/hubfs/Picture21.png)

* 在这个例子中，我知道 0 ≤ Walk Speed（行走速度）< 250 ≤ Run Speed（奔跑速度）≤ 300（可以将 Wwise 连接到 Unreal，并实时分析发送到 pawn 的 RTPC 来跟踪此速度值）。
* 勾选 Use Game Parameter 并创建一个如图所示的“阶梯”，来将 *Pawn\_Speed* RTPC 关联到 *FS\_Type* Switch。

![](http://info.audiokinetic.com/hubfs/Picture22.png)

*我们的 Switch 现在就会由游戏发送的 RTPC 值来控制了：*

* 0 < Pawn_Speed ≤ 250 时会切换到 Walk
* 250 < Pawn_Speed ≤ 300 时切换到 Run

+ 将所有的 Run Assets（奔跑素材）音频文件导入到工程层级中，并基于 *FS\_Type* 创建一个新的 Switch Container，它将包括基于  Material 的两个 Switch Container。您会得到与下图相似的结果（[可以简单地使用 Bradley Meyer 的方法来迅速完成](https://blog.audiokinetic.com/using-templates-in-wwise/)）。

![](http://info.audiokinetic.com/hubfs/Picture23.png)

+ 生成 SoundBank，启动游戏并好好享受吧！

# Unity 3D

设置 Tag（标签）

+ 点击您地图里的任意 actor 并进入 Inspector（检视）窗口。
* 点击 Tag 下拉菜单并选择 **Add Tag（添加标签）…**

![](http://info.audiokinetic.com/hubfs/Picture24.png)

+ 在这个窗口中添加 *Wood*，*Concrete*，*Grass* 和 *Tiles* 值。

![](http://info.audiokinetic.com/hubfs/Picture25.png)

Ground（地面）设置

+ 对于玩家行走时可以踩到的每个地面/对象，将 Inspector 窗口里正确的 Tag 指派到 Collider（碰撞器）。
* 有时 Collider 并不是与骨骼绑定的，所以要确认是在 Collider上设置了 Tag, 而不仅是在骨骼上。

![](http://info.audiokinetic.com/hubfs/Picture26.png)

C Sharp 代码

+ 为您项目的 Character Controller（角色控制器）添加这段 C# 代码。

+ 变量声明：

![](http://info.audiokinetic.com/hubfs/Picture27.png)

+ 主脚本：

![](http://info.audiokinetic.com/hubfs/Picture28.png)

希望您喜欢这个教程，我也期待不久的将来能在 Audiokinetic 博客上和你们分享更多游戏音频知识！

SÉBASTIEN GAILLARD    
音频总监    
DONTNOD娱乐    
Sébastien Gaillard 是一位在游戏声音设计、脚本编写和集成方面有 15 年以上经验的音频总监。在过去 3 年中，他负责管理 DONTNOD 的音频部门，尤其是获奖游戏 “Life is Strange（奇异人生）”的音频研发。他也是 ISART Digital 学院的 Technical Sound Deisgn 课程讲师。

