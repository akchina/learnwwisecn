# Wwise 基础知识 (11) Listener

目录

- Listener- 多个 Listener- Listener——角色和职责

## ListenerListener（听者）在游戏中代表话筒。Listener 在游戏 3D 空间中拥有位置和朝向。在游戏期间，Listener 的坐标与 Game Object 的位置进行比较，以便将与 Game Object 相关联的 3D 声音指定给相应的扬声器，来模模拟实的 3D 环境。程序员必须确保 Listener 的位置信息为最新状态；否则声音将会通过错误的扬声器来呈现。
 ## 多个 Listener
在单人版游戏中，您永远只能有一个视角，因此一个 Listener 就够了。然而，如果多人同时在一个系统上玩游戏，或者同时显示多个视角，则每个视角需要属于其自己的 Listener，这样才能正确地为所有视角渲染音频。Wwise 声音引擎支持多达八个 Listener。在默认情况下，经过声明的每个 Game Object 只能指定给第一个 Listener。然而，程序员可以灵活地将任何 Game Object 动态地指定给任何 Listener，或取消这种指定。由于声源的定位相对于每个玩家的视角并不总是合理，因此为多个 Listener 实现音频会遇到诸多挑战。这主要是因为游戏只使用一组扬声器为多个玩家再现 3D 环境。Wwise 提供各种工具和方法来弥补这一局限，为所有玩家带来尽可能真实的音频体验。有关 Wwise 如何处理多个 Listener 的更多信息，请参阅 SDK 中的“集成 Listener”一节。## Listener——角色和职责下表说明了与 Listener 相关的哪些任务属于声音设计师的职责，哪些属于程序员的职责：
### Table 11.1. Listener——角色和职责
|任务|声音设计师 （Wwise）|程序员（游戏代码/工具）|
|---|:---:|:---:||设置 Listener 的位置信息||#||将 Game Object 指定给 Listener | |#||管理多个Listener||#|
