# Wwise 快速上手指南: 程序员篇
基于 Wwise v2016.1.0.5775

## 目录

* [如何阅读本文档](#sec-howtoread)
* [Wwise 给程序员提供的便利](#sec-benefits)
* [安装和设置](#sec-install)
* [Wwise 工作流程](#sec-workflow)
* [声音引擎初始化和维护](#sec-init)
* [加载声音数据包](#sec-load)
* [播放](#sec-playback)
* [空间定位](#sec-pos)
* [互动控制](#sec-interact)
* [异步处理](#sec-async)
* [调试](#sec-debug)
* [进阶学习](#sec-gofurther)
* [技术支持沟通](#sec-support)


<h3 id='sec-howtoread'></h3>
## 如何阅读本文档

亲爱的程序员，欢迎使用互动音频中间件 Wwise ！本文档可以帮助你快速熟悉 Wwise 知识体系中只与程序员相关的信息，从而能与音频设计团队配合开发互动音频内容。

本文档按正常学习顺序组织章节，力求点到即止，配合[官方 SDK 集成演示](#sec-demo)学习效果最佳。对新用户，建议顺序阅读本文档，打下基础，之后即可在实践中根据[进阶学习](#sec-gofurther)一节，利用 Wwise 安装包内的学习资源继续学习。

本文档是按照副标题注明的 Wwise 版本情况所写，虽然 Wwise 的基础流程比较稳定，本文档版本与较早版本相比还是可能会存在更新的内容。为了叙述的简洁，本文档不会特意标注这些区别，请自行比对。

<h3 id='sec-benefits'></h3>
## Wwise 给程序员提供的便利

Wwise 的功能和流程可以为程序员带来如下便利：

-   **简化音频编程**：由于复杂的声音行为和逻辑被封装到了[事件](#sec-event)和[游戏同步器](#sec-gamesync)中，成为了数据包的一部分，以触发事件和设置游戏同步器为主导的 API 调用得以让代码变得简单统一，比如不再需要直接编程控制音量等声音参数，条件逻辑也得到了简化；事件之外的很多动态声音行为比如音乐切换，尽管有对应的 API 调用，然而还可以进一步被设计师封装到事件中，从而再度简化编码工作。

-   **简化声音集成**：通过事件、游戏同步器和组成抽象数据机制，并商定了机制中用到的各种名称 ID 后，程序员的声音集成代码便只涉及引用这些固定的名称 ID，随后代码将很少需要改动；设计师可以将设计和修改持续封装到 SoundBank 数据中，并尽可能复用 SoundBank。于是开发中声音设计的增量更新可以通过自动化管线来刷新数据，很少需要程序员干预。

-   **减少优化负担**：设计师可以在 Wwise 设计工具中直接监控音频性能数据，并直接在设计中做优化调整，结果依然通过更新声音数据来生效，因此大大降低了程序员优化声音性能的负担。

-   **减少低效沟通**：由于上述的流程改进，在进行了[必要的团队沟通](#sec-teamcomm)之后，程序员和设计师便可以分头行动，很少需要做声音设计机制和数值细节层面上的沟通。

大致了解了这些便利之后，下面就来一步步地学习 Wwise 吧！

<h3 id='sec-install'></h3>
## 安装和设置

在开始学习 Wwise 之前，你需要下载安装 Wwise。

### 去哪里下载？
----

[Audiokinetic 官网下载页面](https://www.audiokinetic.com/zh/download/)提供所有 Wwise 相关的软件产品下载。


### 下载安装启动器
----
为了安装 Wwise 及其捆绑资源，你只需要点击上述下载页面上的*Download Wwise*链接下载并安装Wwise Launcher（Wwise 启动器）。随后打开 Wwise 启动器，便可以通过这个统一的控制中心来管理和 Wwise 相关的资源<sup>[1](#fn0)</sup><a name='fn0-bk'></a>，比如：

* 选择软件组件、下载和在线安装；
* 启动已安装好的 Wwise 软件；
* 打开已有的 Wwise 工程；
* 查看在线文档。

在启动器顶部有一组选项卡，和下载安装相关的选项卡有：

* **Wwise**：点击此选项卡后，如果有新版本可供安装，则可以直接选择安装新版本。如果本机上已经安装了 Wwise，则可以直接启动 Wwise 或者查看和修改安装内容。
* **Samples**: 点击此选项卡后，可以选择安装 Wwise 捆绑的教程、示例工程等学习资源，或者打开已经安装好的资源。
 
在使用启动器进行在线下载安装期间，请保证稳定的网络连接。


### 要下载哪个版本？
----

Wwise 发布的完整版本号写作：

```
年.大版本.小版本.构建号
```

例如:

```
2016.1.0.5775
```

小版本为大版本基础上的补丁更新。我们建议用户在项目中尽量使用同一个大版本，并随时免费更新到该大版本下的最新稳定版本（即最高小版本）。你需要知道：

* 目前Audiokinetic每年最多只会发布两个Wwise大版本。

* 同一个大版本下的小版本间数据兼容。例如：2016.1.0和2016.1.1间数据是兼容的。也即，如果声音设计师将手头的 Wwise 设计工具<sup>[2](#fn1)</sup><a name='fn1-bk'></a>2016.1.0版更新到了2016.1.1，那么他生成的声音数据仍将和程序员手头集成了2016.1.1的游戏版本兼容；反之亦然。

* 大版本间的数据不向后兼容，但旧版本数据可以用新版本设计工具打开并自动升级（Migration）。例如：集成了2016.1.0的游戏版本不能直接使用2015.1.6版本生成的声音数据，但可以用2016.1.0的设计工具打开2015.1.6的工程并自动升级。
* 在升级 Wwise 之前，请根据 Wwise Launcher 中的链接阅读 Release Notes（发布说明）。

### Windows 程序员至少要下载安装什么？
----

在Wwise启动器中进入安装准备后，在内容选择页面的组件区（Packages）勾选以下内容：

* Authoring
* SDK
* Source Code

在目标平台区（Deployment Platforms）勾选以下内容：

* Microsoft > Windows

然后开始安装。安装结果默认位于：`C:\Program Files (x86)\Audiokinetic\`

### Mac 程序员至少要下载安装什么？
----

在 Wwise 启动器中进入安装准备后，在内容选择页面的组件区（Packages）勾选以下内容：

* Authoring
* SDK
* Source Code

在目标平台区（Deployment Platforms）勾选以下内容：

* Apple > OS X

然后开始安装。安装结果默认位于：`/Applications/Audiokinetic/`。

### 在电脑上安装了什么？
----

下表为安装好 Wwise 之后在*指定安装目录*下的内容。


**名称** | **子目录** | **用户** | **说明**
:--- | :--- | :--- | :---
Wwise 设计工具 | Windows: `Authoring/`<br>Mac: `Wwise.app` | 设计师, 程序员 | 声音设计工具，也用于调试和性能分析
Wwise SDK | `SDK/` | 程序员 | 声音引擎、插件库和例程

<h3 id='sec-demo'></h3>
### 官方 SDK 集成演示
----

学习本文档时如果对照官方 SDK 集成演示则效果最佳。安装好 Wwise 后，与该演示相关的关键路径有：

-   `Wwise/SDK/<平台名>/<配置>/bin/`：演示可执行程序根目录（也是 SDK 的动态链接库所在目录），可执行程序有三个配置版本，分别对应`Debug`、`Profile`或者`Release`子目录下面（见[什么是 Debug、Profile 和 Release？](#sec-config)）。

-   `Wwise/SDK/samples/IntegrationDemo/`：演示源代码根目录。

-   `Wwise/SDK/samples/SoundEngine/`：演示程序所依赖的例程根目录。此根目录下的`Common`子目录包含跨平台代码，而`<平台名>`子目录（比如`Windows`）包含平台专用代码及集成开发环境（IDE）工程文件。

-   `Wwise/SDK/samples/IntegrationDemo/WwiseProject/`：演示程序的 Wwise 工程，主要由设计师用 Wwise 设计工具管理并生成演示程序使用的声音数据。Windows 版自带声音数据；Mac 版在安装后需要手动生成声音数据。

-   `Wwise/SDK/samples/IntegrationDemo/WwiseProject/GeneratedSoundBanks/<平台名>/`：演示程序需要加载的声音数据所在目录。

确保声音数据目录下有数据，打开对应目标平台的可执行演示程序，你将看到窗口中有一列专题演示的菜单。若要选择各个专题演示，在电脑上用键盘上的方向箭头、Enter 键和 Esc 键控制，在移动设备上用虚拟手柄控制。专题演示会在本文档后面的专题章节中详细介绍，程序员既可以直接体验演示程序，也可以打开 IDE 工程从源代码重新编译并调试演示程序。

<h3 id='sec-config'></h3>
### 什么是 Debug、Profile 和 Release？
----

在`Wwise/SDK/<平台名>/bin/`和`Wwise/SDK/<平台名>/lib/`下面你会看到`Debug`、`Profile`和`Profile`三个子目录，这是 Wwise 为了支持开发流程而准备的。`lib`子目录下面的 SDK 程序库对应三种配置：

-   **Debug**：用来调试的未优化版本，支持与 Wwise 设计工具通过网络连接来进行实时混音、性能监测和[调试](#sec-debug)；

-   **Profile**：主要用作开发期间运行于游戏中的优化版本，支持与 Wwise 设计工具通过网络连接来进行实时混音、性能监测和[调试](#sec-debug)；

-   **Release**：用于游戏最终发布的优化版本，不支持与 Wwise 设计工具连接。

在开发期间，项目应该尽量使用 *Profile* 版本的 Wwise SDK 程序库配置。

如果安装设置已经完成了的话，接下来我们先看看采用 Wwise 来制作集成声音的基本工作流程。

<h2 id='sec-workflow'>Wwise 工作流程</h2>

### 团队分工
----

在 Wwise 的工作流程中，声音设计师（下称*设计师*）与程序员有明确分工。主要目的是给设计师更多控制，为程序员减轻工作量。

简单地说，设计师主要负责创作、模拟、混合、调试和优化声音以及声音行为，程序员主要只负责触发声音及其行为和部分优化声音性能的工作，设计师和程序员通过明确定义的数据协议进行沟通。

### 迭代流程
----

Wwise 提倡在游戏音频开发中采用五步迭代流程：

-   **创作**：设计师在 Wwise 设计工具中设计音效、音乐和对白等。这一步可以在声音引擎甚至游戏引擎还没有到位时就可以开始。

-   **模拟**：设计师在 Wwise 设计工具中模拟游戏中的声音场景。这一步同样可以在声音引擎甚至游戏引擎就位之前进行。

-   **集成**：程序员在项目之初将声音引擎一次性集成到游戏引擎中，之后设计师可以开始将设计结果生成为声音数据包，通知程序员在游戏中加载数据包，并为游戏中的对象和场景触发播放、音乐切换、混音调整等声音行为。

-   **混音**：设计师从 Wwise 设计工具实时连接到游戏中按照审美标准和创意设计调整多个声音间的相对关系，混音期间无需重新构建游戏。

-   **性能分析**：设计师从 Wwise 设计工具实时连接到游戏中监控游戏运行时的声音资源占用，并和程序员合作优化性能。

### 从游戏的角度看 Wwise 声音引擎活动
----

从游戏开始运行到退出，Wwise 声音引擎至少会进行以下常见活动：

-   **初始化**：声音引擎初始化专用内存池和线程，加载并初始化必要的模块，并加载全局数据包。

-   **加载声音数据包**：在播放声音或执行动态声音行为之前，声音引擎要加载这些行为所需的设计参数和声音数据。

-   **注册并跟踪听觉主体和游戏对象**：声音设计的作用域可以为全局和具体游戏中的对象（比如角色），声音引擎因此需要注册追踪游戏对象。如果游戏要根据角色等游戏对象的空间位置调整声音属性比如音量衰减，则声音引擎要登记并更新听觉主体（一般是玩家控制的角色）和游戏对象（一般是玩家以外的发声体）各自的空间位置。位置跟踪行为一般发生在游戏主循环的每帧中。

-   **触发播放和控制行为**：游戏向声音引擎发送对应播放等行为的消息，并将控制声音行为所需要的游戏变量信息输送给声音引擎。

-   **渲染**：声音引擎根据游戏发来的请求通过渲染来发出声音并实现玩家能听到的动态声音行为，比如音量和音调的变化、音乐切换、混音调整等。

-   **挂起和唤醒**：当游戏切入后台或者被其它应用程序打断的时候，需要暂停所有声音活动；当游戏回到前台或者结束中断时又需要继续进行挂起前的声音活动。

-   **注销游戏对象**：当用在某游戏对象上的播放等行为完成后，游戏引擎将不再需要的游戏对象记录注销掉。

-   **卸载声音数据包**：当游戏不再需要某些声音数据后，声音引擎要卸载这些数据以释放内存。

-   **关闭**：当游戏要退出时，声音引擎需要终止一切播放和其它声音行为，关闭初始化了的模块，结束线程并释放内存池。

### 程序员的职责
----

前面列出的声音引擎活动都有 Wwise SDK 中的对应的 API（应用程序接口）调用，程序员在游戏代码中添加这些调用后声音引擎就能完成这些活动。

<h3 id='sec-teamcomm'></h3>
### 必要的团队沟通
----

程序员需要和设计师做的沟通包括：

-   项目在目标平台上需要的运算资源：内存、声音数据包体积、流的数量等。

-   如何根据游戏场景触发事件。

-   要用哪些游戏中的变量来实时影响声音。

理解了这些工作流程和之后，下面就来看看程序员如何在游戏代码中集成 Wwise 的声音引擎和基于 Wwise 的声音设计内容。

<h3 id='sec-init'></h3>
## 声音引擎初始化和维护

这一步对于同一个游戏引擎一般只需要编一次程。

### 初始化
----

首先，打开[官方 SDK 集成演示](#sec-demo)中当前开发平台的 IDE 工程，在进行初始化之前，需要针对目标平台定义内存钩子（Memory Hooks），这些钩子可用来实现和监控内存操作。内存钩子的例程参见以下源代码文件中的对应代码段：

* Windows 平台：`Wwise/SDK/samples/IntegrationDemo/Windows/Main.cpp`<sup>[2](#fn3)</sup><a name='fn3-bk'></a>；

* Mac 平台：`Wwise/SDK/samples/IntegrationDemo/Mac/Platform.cpp`。

接下来，找到`IntegrationDemo.cpp`文件中的`IntegrationDemo::InitWwise()`方法<sup>[3](#fn4)</sup><a name='fn4-bk'></a>，这个方法给出了声音引擎初始化的例子。初始化包括以下步骤：

-   **初始化内存管理器**：`AK::MemoryMgr::Init();`这一步将确定全局内存参数，比如 Wwise 专用的几个内存池大小。

-   **创建流管理器**：`AK::StreamMgr::Create();`这一步创建流管理器，它负责和声音数据包相关的 I/O 行为比如流播放。

-   **初始化 I/O 设备**：`m_pLowLevelIO->Init();`这个设备对象负责平台相关的底层 I/O 行为，演示程序中对应各个平台都有不同的类实现。在中我们会稍作讨论。

-   **初始化声音引擎**：`AK::SoundEngine::Init();`这一步初始化声音引擎本身。

-   **（可选）初始化音乐引擎**：`AK::MusicEngine::Init();`如果你的项目中要用到 Wwise 的互动音乐功能，则需要这一步，否则不用。

-   **初始化性能分析通信模块**：在项目的开发版本中为了让 Wwise 设计工具能与游戏连接并使用 Wwise Profiler（性能分析器）来监视和调试游戏中的声音，要先初始化通信模块。

```cpp
#if !defined AK_OPTIMIZED
          
// ......

AK::Comm::Init();

// ......

#endif // AK_OPTIMIZED
```
项目的 Release 版本中需要定义`AK_OPTIMIZED`这个条件编译宏，以确保最终发布版本不包含性能分析的支持。

至此声音引擎就初始化完毕了。

<h3 id='sec-render'></h3>
### 渲染 
----

在声音引擎初始化之后，为了让设计师设计的播放和其他声音行为最终能让玩家听到，还需要在游戏主循环中加入渲染声音的 API 调用：`AK::SoundEngine::RenderAudio();`。一般要在游戏主循环的每帧中进行这些渲染。

参考`IntegrationDemo.cpp`中的`IntegrationDemo::Render()`方法。

### 关闭
----

退出游戏的时候需要关闭声音引擎。关闭声音引擎与初始化操作大致“对称”，调用各个模块的对应 API 即可，参见`IntegrationDemo.cpp`中的`IntegrationDemo::TermWwise()`方法：

-   关闭性能分析通信模块：

```cpp
#if !defined AK_OPTIMIZED
             
// ......

AK::Comm::Term();

// ......

#endif // AK_OPTIMIZED  
```      

-   **（可选）关闭音乐引擎**：`AK::MusicEngine::Term();`

-   **关闭声音引擎**：`AK::SoundEngine::Term();`

-   **关闭 I/O 设备**：`m_pLowLevelIO->Term();`

-   **销毁流管理器**：`AK::IAkStreamMgr::Get()->Destroy();`

-   **关闭内存管理器**：`AK::MemoryMgr::Term();`

### 挂起和唤醒
----

当游戏切入后台或者被其它应用程序打断的时候，通常需要在不关闭整个声音引擎的条件下暂停所有的声音播放和行为；当游戏回到前台或者回复中断前的状态时又需要唤醒声音引擎来继续挂起前的播放和行为。

调用`AK::SoundEngine::Suspend()`来挂起，调用`AK::SoundEngine::WakeupFromSuspend()`来唤醒。调用这些 API 的时机要看各个平台上对于前后台切换和中断处理的具体要求。参考[官方 SDK 集成演示](#sec-demo)中的相关 API 调用<sup>[4](#fn7)</sup><a name='fn7-bk'></a>。

<h3 id='sec-dbginit'></h3>
### 调试初始化 
----

因为在通信模块初始化之后，才能使用 Wwise Profiler 来监控声音引擎的行为，所以初始化过程中还不能利用 Profiler 来调试。这一步的调试主要依赖对相关 API 的返回值做出检测。任何一步失败后要终止后续步骤。

注意检查控制台的错误输出中以`AK`起头的信息，`AK`代表 Audiokinetic。

<h3 id='sec-load'></h3>
## 加载声音数据包

在播放声音或执行动态声音行为之前，声音引擎要加载这些行为所需的声音数据和控制参数。

<h3 id='sec-bank'></h3>
### SoundBank
----

Wwise 里的声音数据用特殊的文件格式打包，这种数据文件被称为 *SoundBank*，文件扩展名为`.bnk`。SoundBank 由 Wwise 设计工具生成，实际中可以由设计师设计其内容，并通过 Wwise 自带的设计工具命令行程序`WwiseCLI.exe`集成到项目自动化管线中去。

### 准备底层I/O
----

在中我们提到过初始化底层 I/O 设备（Low-level I/O），这个模块负责读取 SoundBank 所需要的数据 I/O 工作，主要包括设置和添加 SoundBank 目录，根据这些目录和用户输入的 SoundBank 文件名来构建 SoundBank 文件完整路径，并利用平台系统的 I/O API 打开文件和读取数据。

参考[官方 SDK 集成演示](#sec-demo)源代码中以下类及其基类的实现：`CAkFilePackageLowLevelIOBlocking`。

### 设置目录
----

SoundBank 文件一般存放在目标平台存储设备的一个或多个目录下。声音引擎初始化完毕后，为了能够加载SoundBanks，要调用几个 API 来指定 SoundBank 所在目录的路径。

参考`IntegrationDemo.cpp`中的`IntegrationDemo::Init()`方法：

-   **设置声音数据包基本目录**：`m_pLowLevelIO->SetBasePath();`，加载声音数据包时，声音引擎将首先到这个目录下寻找声音数据包。在 Android 平台上，这个基本目录必须位于应用的APK安装包体内。

-   **（可选）添加其它声音数据包目录**：`m_pLowLevelIO->AddBasePath();`，移动平台上有时需要使用多个目录来储存声音数据，比如游戏发售后的增量更新会希望存在另外的目录下，这些新目录需要加入搜索路径。在 Android 平台上，如果要将 SoundBank 存放在应用的 APK 包之外的位置，需要执行这一步。

-   **设定本地化语言**：`AK::StreamMgr::SetCurrentLanguage();`，这个语言名字代表上述目录下的子目录名，声音引擎找到上述*基本路径（Base Path）*后，会根据当前本地化语言设定寻找语言相关的声音数据（比如对白）的存储位置。

设置好这些目录相关的路径之后，在调用加载 SoundBank 的 API 时，只需输入 SoundBank 文件名作为参数就可以了。

### 加载全局数据
----

设置好SoundBank路径之后，首先要加载*初始化 SoundBank*，它的文件名是固定的：`Init.bnk`。这个文件包含游戏期间所有声音和声音行为需要的共享数据。

使用`AK::SoundEngine::LoadBank()`这个 API 来加载 SoundBank，可以通过参数选择阻塞或者异步方式。

参考[官方 SDK 集成演示](#sec-demo)源码`BaseMenuPage.cpp`中方法`BaseMenuPage::Init()`的实现。

### 加载播放需要的数据
----

在播放声音和控制其它声音行为之前，要加载这些声音和行为数据所在的 SoundBanks，依然采用`AK::SoundEngine::LoadBank()`API。

参考[官方 SDK 集成演示](#sec-demo)中的相关 API 调用。

### 卸载数据
----

确认不再需要 SoundBank 之后可以将其卸载以释放内存。

调用`AK::SoundEngine::UnloadBank()` API 来卸载 SoundBank。

<h3 id='sec-dbgbank'></h3>
### 调试声音数据加载
----

SoundBank 相关的调试可以利用 Wwise Profiler <sup>[5](#fn8)</sup><a name='fn8-bk'></a>，比如：

-   在 Capture Log 窗口中关注带有`Bank`关键字的错误信息。

-   在 Advanced Profiler（高级性能分析器）窗口的 SoundBanks 选项卡中观察已经加载的 SoundBanks。

注意检查控制台的错误输出中以`AK`起头的信息，`AK`代表 Audiokinetic。

<h3 id='sec-playback'></h3>
## 播放

### 注册游戏对象
----

在播放声音之前要注册播放涉及的游戏对象。多数声音都有对应的游戏对象比如角色、武器等，声音引擎需要注册这些对象，随后的播放和其它声音行为，将根据注册后的对象 ID 确定。当不再需要这些游戏对象的时候，比如角色退场后，声音引擎需要注销这些对象。

调用`AK::SoundEngine::RegisterGameObj()`和`AK::SoundEngine::UnregisterGameObj()` API 来注册和注销游戏对象。参考[官方 SDK 集成演示](#sec-demo)中的相关 API 调用。Wwise 用`AK_INVALID_GAME_OBJECT`来表示无效对象或者所有对象（全局域）。

<h3 id='sec-event'></h3>
### 事件
----

Wwise 用 Event（事件）来封装播放等声音行为。一个 Event 可以包含设计师定义的多个行为，比如播放、停止播放、改变音量等，程序员只需要和设计师约定 Event 的名称，继而按照约定的名称字符串以及作用对象的注册 ID 作为输入参数调用`AK::SoundEngine::PostEvent()` API 即可，发送到声音引擎的事件经过操作即可生效，成为玩家可以听到的声音效果。

参考[官方 SDK 集成演示](#sec-demo)中的相关 API 调用。

注意*停止播放*、*暂停播放*等也是事件可以封装的行为，由设计师控制。

<h3 id='sec-dbgevent'></h3>
### 调试播放
----

游戏对象相关的调试可以利用 Wwise Profiler，比如：

-   在 Capture Log 窗口中关注带有`Game Object`等关键字的错误信息。

-   在 Performance Monitor 窗口中的时间轴上通过`Number of Registered Objects`曲线实时观察当前已注册游戏对象，可能需要先从窗口标题栏上的 View Settings 中启用该参数。

-   在 Game Object Profiler 界面布局（Layout 菜单下选择）下实时观察游戏中注册过的游戏对象和听者的空间位置。

事件相关的调试可以利用 Wwise Profiler，比如：

-   在 Capture Log 窗口中关注带有`Event`和`Stop`等关键字的错误信息。

-   在 Performance Monitor 窗口中的时间轴上通过`Number of Active Events`曲线实时观察当前活跃事件，可能需要先从窗口标题栏上的 View Settings 中启用该参数。

注意检查控制台的错误输出中以`AK`起头的信息，`AK`代表 Audiokinetic。

<h3 id='sec-pos'></h3>
## 空间定位

### 听者和游戏对象
----

声音引擎要通过更新听者和游戏中发声对象的空间信息来实现声音的空间定位。听者（Listener）一般代表玩家控制的主角，游戏对象（Game Object）代表游戏中的发声体（Emitter），两者在空间中的相对关系决定声音的方向和距离衰减所造成的音量等声音属性变化。

在游戏主循环中，调用`AK::SoundEngine::SetListenerPosition()`来更新听者的空间位置，调用`AK::SoundEngine::SetPosition()`来更新游戏对象的空间位置。

参考[官方 SDK 集成演示](#sec-demo)中的相关 API 调用。

<h3 id='sec-dbgpos'></h3>
### 调试空间定位
----

空间定位相关的调试可以利用 Wwise Profiler，比如：

-   在 Game Object Profiler 界面布局（Layout 菜单下选择）下实时观察游戏中注册过的游戏对象和听者的空间位置。

注意检查控制台的错误输出中以`AK`起头的信息，`AK`代表 Audiokinetic。

<h3 id='sec-interact'></h3>
## 互动控制

<h3 id='sec-gamesync'></h3>
### 游戏同步器
----

Wwise通过*Switch（切换开关）*，*State（状态）*，*RTPC（实时参数控制）*等参数来将游戏中的信息挂接到声音属性和行为上实现互动控制，这些参数统称为*Game Syncs（游戏同步器）*。

调用对应这些参数的 API 来改变这些参数的值，如下表所示：

**游戏同步器** | *调用* API
:--- | :---
Switch | `AK::SoundEngine::SetSwitch()`
State | `AK::SoundEngine::SetState()`
RTPC | `AK::SoundEngine::SetRTPCValue()`

Switch 用于游戏对象上的状态切换，比如根据角色踩到的路面类型变化来切换脚步声；State 用于全局状态切换，比如根据游戏场景和进程发展来播放不同的音乐；RTPC 用于连续数值变化，比如利用车辆发动机转速的连续变化来影响发动机声音。

参考[官方 SDK 集成演示](#sec-demo)中的相关 API 调用。

程序员不需要知道这些 API 是如何影响声音的，只需要和设计师约定好状态和切换的名称，准备好游戏世界中的变量，将这些变量的数值信息作为 API 输入参数发送给声音引擎就可以了。

<h3 id='sec-dbggamesync'></h3>
### 调试互动控制
----

互动控制相关的调试可以利用 Wwise Profiler，比如：

-   在 Capture Log 窗口中关注带有`Switch`和`State`关键字的错误信息。

-   可以在 Game Sync Monitor 窗口中沿时间轴观察 RTPC 值的时变情况。

注意检查控制台的错误输出中以`AK`起头的信息，`AK`代表 Audiokinetic。

<h3 id='sec-async'></h3>
## 异步处理

在发送[事件](#sec-event)的时候，可能无法预知一些实时信息和操作的时机，必须等待声音引擎的通知，一个例子是音乐的节拍时间点。

### Callbacks
----

`PostEvent`API 因此提供了专门的参数，用来给程序员指定Callbacks（回调函数）以便进行异步处理。这些 Callbacks 可以完成很多有用的任务，比如：

-   如果注册了`AK_EndOfEvent`类型的 Callback，当事件处理结束时客户程序就可以收到通知，这样可以实现比如衔接多个事件或行为这样的功能。

-   如果注册了`AK_MusicSyncBeat`类型的 Callback，就可以在互动音乐播放时的节拍点上收到通知，实现基于节拍的功能。

参考[官方 SDK 集成演示](#sec-demo)中的相关 API 调用。关于所有支持的Callback类型请参考 [API 文档中的条目：`AkCallbackType`](https://www.audiokinetic.com/library/2016.1.0_5775/?source=SDK&id=_ak_callback_8h_a948c083ff18dc4c8dfe1d32cb0eb6732.html)。

<h3 id='sec-dbgasync'></h3>
### 调试异步处理
----

因为 Callback 函数内容是用户代码，可以包含任何内容，所以调试方法可以参考其它章节的内容。Callback 是否已执行可以用常规方法检测。

注意检查控制台的错误输出中以`AK`起头的信息，`AK`代表 Audiokinetic。


<h3 id='sec-debug'></h3>
## 调试

调试是开发中程序员关注的重要内容，本节汇总前述各章节中的*调试*小节的内容，以供集中查阅。

* [调试初始化](#sec-dbginit)
* [调试声音数据加载](#sec-dbgbank)
* [调试播放](#sec-dbgevent)
* [调试空间定位](#sec-dbgpos)
* [调试互动控制](#sec-dbggamesync)
* [调试异步处理](#sec-dbgasync)

所有涉及 Wwise Profiler 的调试活动，程序员和设计师都可以参与。

<h3 id='sec-gofurther'></h3>
## 进阶学习

读到这里，Wwise 集成需要的程序员基础知识就告一段落了。希望你已经体会到了[Wwise 给程序员提供的便利](#sec-benefits)。接下来：

-   本文档中提到的话题均可在官方 SDK 帮助文档中找到更详尽的内容。
-   Wwise SDK 有丰富的功能值得研究，本文档只提到了一部分。在读完本指南之后，你可以参照[官方 SDK 帮助文档](https://www.audiokinetic.com/library/2016.1.0_5775/?source=SDK&id=index.html)继续学习。
-   Wwise 安装包内自带开源游戏 Cube Demo，可以学习其源代码，并试试动手修改和添加功能。
-   可以免费下载并学习 Wwise 为商用引擎的集成，比如 Unity 和 Unreal 集成。

下面简单介绍一部分进阶内容，仅就前文提到的方向稍加延伸提示。其它进阶内容比如动态对话和动态序列、编写 Wwise 插件等，有兴趣的读者可以阅读官方 SDK 帮助文档中的对应章节。

### SoundBank 加载策略
----

基于 SoundBank 的内存和 I/O 管理是开发中必须考虑的话题。本文档介绍了`AK::SoundEngine::LoadBank()`API，这种 SoundBank 加载方式简单易用，是最基本和最常用的方式，但 Wwise SDK 还提供了其它 API，用来实现更灵活的基于 SoundBank 内容的加载管理策略。

详见[官方 SDK 帮助文档章节](https://www.audiokinetic.com/library/2016.1.0_5775/?source=SDK&id=sdk__bank__training.html)。

### 流播放
----

按照本文档中的方式集成了 [SoundBank](#sec-bank) 之后，要注意
`AK::SoundEngine::LoadBank()`API 将所有数据一次性载入内存；如果需要采用流播放以节省内存，则程序员一般无须干预，由设计师在设计工具中指定。采用流播放后 Wwise 会在 SoundBank 的存放位置生成扩展名为`.wem`<sup>[6](#fn9)</sup><a name='fn9-bk'></a>的文件，这些文件包含播放用的确切音频数据，需要和 SoundBank 文件一同部署到目标平台，但程序只需加载`.bnk`文件，不需要管理`.wem`文件。

### 远程流媒体
----

如果 Wwise 自带的底层 I/O 实现不能满足项目的具体需求，则项目可以自行实现自己的底层 I/O 。比如，要以流媒体形式读取远程 SoundBank，那么程序员可以实现为在线流播放的底层 I/O。[官方 SDK 集成演示](#sec-demo)中也包括阻塞（类名关键字*Blocking*）和异步（类名关键字*Deferred*）I/O 的例程。

关于底层 I/O，详见[官方 SDK 帮助文档章节](https://www.audiokinetic.com/library/2016.1.0_5775/?source=SDK&id=streamingmanager__lowlevel.html)。

### 环境效果
----

集成了[空间定位](#sec-pos)之后，还可以在游戏地图中指定几何区域，当听者或者游戏对象进入特定区域后，可以编程触发设计师准备好的环境效果比如混响，并可以在多个区域效果间做出混合，创造连贯可信的声学环境模拟。详见[官方 SDK 帮助文档章节](https://www.audiokinetic.com/library/2016.1.0_5775/?source=SDK&id=integrating__elements__environments.html)。

### 线、面、体发声体
----

河流、湖泊和大型机械需要比点声源更复杂的声音效果，`AK:SoundEngine::SetMultiplePositions()`这个 API 可以帮助实现这些几何上呈现为线、面、体的发声体。

详见[官方 SDK 帮助文档章节](https://www.audiokinetic.com/library/2016.1.0_5775/?source=SDK&id=soundengine__3dpositions.html#soundengine_3dpositions_multiplepos)。

<h3 id='sec-gameeditor'></h3>
### 游戏引擎编辑器集成
----

游戏开发中常常需要在游戏编辑器中浏览一些声音属性，比如在 3D 游戏中，可能会需要用图形来表现地图中游戏对象声音的传播范围，以方便在不试听的情况下监控调试声音衰减。Wwise 提供了特殊的模块SoundFrame来帮助游戏引擎程序员来实现这些游戏编辑器集成功能，让游戏编辑器可以直接和Wwise 设计工具沟通信息。

参考`Wwise/SDK/samples/SoundFrame/`下的例程。

<h3 id='sec-support'></h3>
## 技术支持沟通

希望通过学习本文档，你已经可以上手使用 Wwise SDK 了，祝你和设计师团队合作顺利！

如果注册用户在学习和查阅文档之后，依然遇到了疑难问题，项目也购买了技术支持的话，则可以通过官网的注册项目页面或者电子邮件向 Audiokinetic 公司发起技术支持请求。如果没有购买技术支持，请前往[官方问答论坛](https://www.audiokinetic.com/qa/)发帖。

注册用户在向技术支持提问的时候应尽可能提供下列信息：

-   使用的 Wwise 版本；
-   使用的游戏引擎（主要指商用引擎）版本；
-   目标平台；
-   问题描述和期待的结果；
-   实际操作步骤和观察结果；
-   Wwise 设计工具中 Profiler 的 Capture Log（捕获日志）；
-   其它能提供诊断信息的资料：程序日志、崩溃堆栈、以及截图等辅助信息。

如果网络连接允许的条件下，那么从项目页面发出技术支持请求是推荐的方式，输入上述信息会较明确方便，也便于双方追踪问题进度；但如果网络连接有障碍，则可以发邮件至：support AT audiokinetic DOT com<sup>[7](#fn10)</sup><a name='fn10-bk'></a>，并手动输入上面列出的相关信息。

技术支持服务的详情请见Audiokinetic官方网站的[技术支持服务](https://www.audiokinetic.com/zh/support/)页面。

**Happy coding with Wwise!**


## 脚注
<a id='fn0'>[1]</a>
Wwise 为 Unity 和 Unreal Engine 提供的官方集成暂时不能通过 Wwise Launcher 安装和管理。[返回](#fn0-bk)

<a id='fn1'>[2]</a>
Wwise Authoring Tool 在本文档中引用为`Wwise 设计工具`或`设计工具`。[返回](#fn1-bk)

<a id='fn3'>[3]</a>
下文引用文件时如无特殊交代，均位于`Wwise/SDK/samples/IntegrationDemo/`或者`Wwise/SDK/samples/SoundEngine/`目录及其子目录下面。[返回](#fn3-bk)

<a id='fn4'>[4]</a>
为了简化，本文档中所有对 API 调用的引用均省略了输入参数和返回值说明。若想了解这些参数，请参考 Wwise SDK 帮助文档，文档可以从 Wwise 安装后的`SDK/Help`目录下和[官方网站](https://www.audiokinetic.com/library/2016.1.0_5775/?source=SDK&id=index.html)上获取。[返回](#fn4-bk)

<a id='fn7'>[5]</a>
如果要寻找 API 调用例子，可以在 IDE 或者文本编辑器中根据 API 名称关键字在文件集合中作全局搜索。[返回](#fn7-bk)

<a id='fn8'>[6]</a>
Wwise Profiler 是 Wwise 设计工具的一部分，详见[Wwise 帮助文档](https://www.audiokinetic.com/library/2016.1.0_5775/?source=Help&id=profiling)。[返回](#fn8-bk)

<a id='fn9'>[7]</a>
*wem* 代表 Wwise Encoded Media。[返回](#fn9-bk)

<a id='fn10'>[8]</a>
为防垃圾邮件，邮件地址中的`@`和`.`字符分别用`AT`和`DOT`替代。[返回](#fn10-bk)
