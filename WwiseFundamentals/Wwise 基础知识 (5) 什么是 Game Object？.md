# Wwise 基础知识 (5) 什么是 Game Object？

目录

- 什么是 Game Object?- 注册Game Object- Scope——游戏对象范围与全局范围的比较- 使用 Game Object 的优势- Game Object —— 角色和职责

## 什么是 Game Object?Game Object（游戏对象）是 Wwise 中的核心概念，因为声音引擎中被触发的每个 Event（事件）都与一个Game Object 相关联。Game Object 通常指游戏中能够发出声音的特定对象或元素，包括角色、武器、环境对象（比如火把）等。然而在某些情况下，您可以将 Game Object 指定给某个游戏元素的不同部分。例如，您可以将不同的 Game Object 指定给巨人角色的不同部分，以使巨人的脚步声和配音听起来好象来自 3D 声音空间中的不同位置。### Note如果您熟悉 Unreal 虚幻游戏引擎，Wwise 中的 Game Object 就类似于 Unreal 中的 Actors（角色）。Wwise 为每个 Game Object 存储了各种信息，Game Object 将使用这些信息来确定在游戏中如何播放每个声音。以下所有类型的信息均可与Game Object相关联：
- 与Game Object相关联的音频对象的属性偏置值，包括音量和音高。- 3D 位置和朝向。- Game Sync 信息，包括State（状态）、Switch（切换开关）和 RTPC（实时参数控制）。- 环境效果。- 声障（Obstruction）和声笼（Occlusion）。### Note与其他属性不同，衰减应用于音频对象而非游戏对象。这样声音设计师可以更加灵活地单独控制每个音效的衰减。Wwise 中的 3D GameObject（3D 游戏对象）视图可以让声音设计师查看音效所关联的Game Object、Game Object相对于Listener（听者）的位置以及每个音效的衰减半径。## 注册Game Object在使用Game Object之前，程序员需要在游戏代码中注册Game Object。当您不再需要它们时，您应该注销它们，因为在注销与这些值相关联的游戏对象之前，声音引擎将继续存储它们的相关信息（3D 位置、RTPC、切换开关等）。## Scope——游戏对象范围与全局范围的比较通过使用Game Object，Wwise 引入了Scope（作用范围）概念，事件一节中将对Scope 进行简短的讨论。Scope 决定把属性和事件应用在游戏的哪个层次上。现在您可以选择在Game Object层次上或全局范围内应用这些元素。游戏的特定情景和/或正在发生的动作将决定作用范围，以及最终您在 Wwise 中所采取的方法。例如，假设您在创建第一人称射击游戏。游戏主人公必须穿过城市街道才能抢到敌人的旗帜。当该角色在城市中行走时，您将听到他的脚步声。如果您想更改与脚步相关联的属性或音效，您只能在与对应主人公双脚的 Game Object 层次上局部应用这些更改。另一方面，如果游戏角色潜入水中，则需要更改周围环境中继续播放的所有音效，例如爆炸和车辆。在此类情形下，您需要进行全局更改。

## 使用 Game Object 的优势使用 Game Object 可以简化音频管理，因为程序员仅需跟踪 Game Object ，无需跟踪单个音效。创建 Game Object 后，程序员仅需发送事件、设置 Game Sync （包括切换开关、状态和 RTPC）以及游戏环境。在 Wwise 中，由声音设计师确定播放哪个音效和如何播放该音效的具体细节。通过这种方法，您可以在处理与各种游戏实体相关联的诸多音效时节省大量的时间。## Game Object —— 角色和职责下表说明了哪些与 Game Object 相关的任务属于声音设计师的职责，哪些属于程序员的职责：
### Table 5.1. Game Object —— 角色和职责

|任务|声音设计师 （Wwise）| 程序员（游戏代码/工具）|
|---|:---:|:---:||将Game Object关联到游戏中的 3D 对象|#|#||注册/注销 Game Object| |#|
|更新 Game Object 定位信息||#||设定每个音频对象的衰减|#|||定义事件作用范围|#||
