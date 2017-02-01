# 来自 Wwise 研发团队的秘笈！

![](http://info.audiokinetic.com/hubfs/tips%20from%20wwise%20dev%20team.jpg)

本文通过一系列针对资深用户的小贴士，介绍了 Wwise 中一些鲜为人知的功能。其中多数小节都独立成章，可以根据自己的掌握程度来跳过部分章节。

## 自定义键盘快捷键

Wwise 中的诸多功能都允许通过键盘快捷键来访问（按下 Ctrl+Shift+K 可以查看快捷键列表）。很多功能都有默认快捷键，你可能会发现它们大多使用了辅助按键（Ctrl、Shift 和 Alt）。这就意味着当你需要其他功能时，可以抛开这些辅助按键，而仅使用单键来访问所需功能。

比如可以将快捷键设置如下：

|**指令**|**快捷键**|
|---|:---|
|Workgroup > Diff (Work unit)|D|
|Edit in External Editor 1 (Default)|E|
|Show Source Editor|S|
|View > Project Explorer > Create Random Container|R|

为你最常用的功能设置单键来进行极速操作吧，快捷键就是生产力。

另外请注意，所有的快捷键设置都会保存到这个文件：  
*%appdata%\Audiokinetic\Wwise\KeyboardShortcuts.xml*。

可以将这个文件随意复制转存，方便与团队共享，也方便自己在多个电脑上共用。

## 打印自己的 Wwise 快捷键列表

既然自定义快捷键已经就位，下面我们回到键盘快捷键窗口（Ctrl+Shift+K）。

+ 点击指令列表（面板左侧）。
+ 按 Ctrl+A 将列表全选。
+ 按 Ctrl+C 复制。

打开 Excel 或者其他表格软件并粘贴数据。记住，Wwise 会把列表内容自动转换为制表符分割格式，可以随时粘贴到表格中。

为复制好的表格美化一下格式，把它打印出来贴在工作台上吧。

## 精通 List View

List View 是管理工程数据的首选利器。 它采用通用表格形式，允许你根据喜好来自定义各列中的内容。Wwise 中的各种对象属性数以百计，全都可以显示在 List 视图中，还允许编辑。

按下 Ctrl+Shift+F 即可显示 List View，你会发现：

+ 如果当前处于输入焦点的视图中已经选中了某些对象（例如在 Project Explorer 中），按下 Ctrl+Shift+F 后打开的 List View 中将自动显示所选对象；并且
+ 按下 Ctrl+Shift+F 后，为了便于在 Wwise 工程中搜索对象，List View 将自动聚焦于其中的搜索框。

更改 List 视图中各列内容的方法如下：

+ 右键点击 List View 中的表头，选择 **Configure Columns**；或
+ 点击视图标题栏上的 **View Settings** 按键。

此外，List View 还有一些你可能不知道的妙用，在多选的情况下尤其便利。

当选择了多个对象时：

+ **设置属性值：**当修改其中一个对象的属性时（滑动条、下拉菜单和复选框），所选全部对象都将受到影响，即其他对象属性值也会作相同修改。在拖动滑动条时按住 Shift 键可以进行微调。
+ **设置属性偏置：**在拖动滑动条时按住 Alt 键，所选对象的属性值将基于各自的当前值来进行相对偏置，而不是设置其绝对属性值。
+ **手动键入属性值偏置：**手动键入属性值后附加“+”，可以将其作为正偏置量，附加“-”则作为负偏置量。例如：键入“6-”即可将全部所选对象的音量降低 6dB。

## 改装 MIDI Keymap Editor

MIDI Keymap Editor 用于编辑 Wwise 对象的 MIDI 相关属性，它基于标准 List View，但有两个特别之处：

+ 总是显示所选对象的子对象。
+ 默认显示 MIDI 属性。

List 视图的好处就是可以自由定制其中显示的属性，MIDI Keymap Editor 也不例外。你可以将这些 MIDI 属性替换成自定义属性，从而让 MIDI Keymap Editor 成为一个通用的属性监控器，而且像 Contents Editor 一样总是显示所有子对象属性。

## 认识自定义属性

自定义属性允许你在 Wwise 对象中储存额外信息，可以用于工程管理，或者存储游戏所用的元数据。要在工程中设置自定义属性，可以在工程文件夹（即与工程同名的文件目录）下面创建空文件，并命名为 PROJECTNAME.wcustomproperties。

下面的例子中，我们要加上一个自定义属性，来存储链接到 bug 数据库的 URL。 假设我们使用 JIRA，即可在 wcustomproperties 文件中添加下列内容：

    <?xml version="1.0" encoding="UTF-8"?>
    <!-- Copyright (C) Audiokinetic Inc. -->
    <PluginModule>
      <WwiseObject Name="Sound" CompanyID="1" PluginID="1">
        <Properties>
          <Property Name="Custom:JIRA" Type="string" DisplayName="JIRA URL">
             <DefaultValue></DefaultValue>
          </Property>
        </Properties>
      </WwiseObject>
    </PluginModule>

重新启动 Wwise 并选择 Sound SFX 对象，注意 JIRA URL 属性将会显示在下面的位置：

![](http://info.audiokinetic.com/hubfs/tips-from-wwise-dev-team-2.png)

+ Multi Editor
+ List View （需自定义列内容）
+ Referemce View（需自定义列内容）
+ Query View（需自定义列内容）
+ Property Editor 的 All Properties 选项卡

关于自定义属性的更多信息，请参考完整文档：  
[https://www.audiokinetic.com/library/edge/?source=SDK&id=defining__custom__properties.html](https://www.audiokinetic.com/library/edge/?source=SDK&id=defining__custom__properties.html) 

## 属性和注释中的 URL 链接

如果在自定义字符串属性或者注释中插入了 URL，Wwise 会将其显示为允许点击的链接：

![](http://info.audiokinetic.com/hubfs/tips-from-wwise-dev-team-3.png)

进行整合时，可以使用自定义的操作系统协议处理器，使其他应用程序在点击 URL 时有所反应。比如，可以点击 URL 来打开游戏引擎中的相应元素，也可以让链接指向内网中的文档、bug 数据库，甚至是电脑上的网络服务器。  

## 按住 Shift 同时点击右键  

Wwise 的各种视图均采用标准上下文菜单。右键点击 Wwise 对象（比如 Sound SFX 对象）时，就会有针对 Wwise 对象的快捷菜单弹出，你可能已经非常熟悉这个菜单了。

现在，我们按住 Shift 同时点击右键，会出现两个额外的选项：

+ Copy path(s) to clipboard（将路径复制到剪贴板）
+ Copy GUID(s) to clipboard（将 GUID 复制到剪贴板）

撰写文档和进行整合时，这两个选项可能会很有用。

## 成为 Music Segment Editor 专家

你可能还不知道，Music Segment Editor 里有一些可以提升效率的功能。这里列出一部分，次序不分先后：

+ 双击空白处，可以新建轨道。
+ 按下 Shift 并点击标尺，可以新建 Custom Cue。
+ 按下 Insert 键，可以在播放光标位置新建 Custom Cue。
+ 按住 Ctrl 并拖拽 Entry Cue，其位置改变的同时将不会移动音频内容。
+ 选中片段后，按下 Ctrl 和箭头键，可以移动所选片段。
+ 选中片段后，按下 Shift、Ctrl 和箭头键，可以微移所选片段。
+ 按住 Ctrl 并拖拽片段，可以将其复制。
+ 选中多个片段后，可以同时调整它们的边缘位置。
+ 按下 P 键，可以让 Entry Cue 和 Exit Cue 与所选片段首尾对齐。
+ 按下 S 键，可以在播放光标处切分所选片段。
+ 按下数字键盘的 . 键，可以让播放光标跳转至时间线起点。
+ 按下数字键盘的 0 键，可以让播放光标跳转至 Entry Cue。
+ 按下数字键盘的 1 键，可以让播放光标跳转至 Exit Cue。
+ 按下数字键盘的 2-9 键，可以让播放光标跳转至相应的 Custom Cue。
+ 按下 Q、W 或 E 键，可以打开或者关闭 Bar/Beats、Cues 或 Clips/Loops 对齐选项。
+ 可以从系统文件夹中将 WAV 或 MIDI 文件直接拖拽至 Segment Editor 中。

下列操作适用于 Music Segment Editor、Source Editor 以及各曲线编辑器：

+ 按下 X 键并拖拽，可以平移视图。
+ 按下 Z 键并在视图中拖拽方框，可以将所选部分放大。
+ 按下 Z 键并点击视图，可以显示整个工作区域。
+ 按下数字键盘的 + 键，可以放大视图。
+ 按下数字键盘的 - 键，可以缩小视图。

## 成就解锁

恭喜一路读到了这里，你太棒了！希望今天能学有所获。请不要放弃梦想，更不要让技术细节放慢你的脚步。

**BERNARD RODRIGUE**    
开发总监    
Audio Kinetic

Bernard Rodrigue 是 Audiokinetic 的开发总监。他自 2005 年加入 Audiokinetic 后，一直积极参与 Wwise 的基础研发。现在，Bernard 仍在带领团队从事 Wwise 的提升和扩展研发，比如 Interactive Music 等等。
