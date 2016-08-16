# Wwise 基础知识 (9) 理解 SoundBank

目录

- 理解 SoundBank- File Packager（文件打包器）

## 理解 SoundBank为了高效地管理游戏的音频和振动（motion）组件，Wwise 将游戏的所有音频和振动数据置于bank（库）中。bank 大体上是指包含游戏音频和振动数据、媒体或两者兼有的文件。这些bank在游戏的特定时间点加载到游戏平台内存中。通过仅加载必要的资源，可以针对每个平台优化音频和振动的内存占用。bank 是您所有工作成果的集合，其中包含构成游戏所需的最终音频和/或振动内容。Wwise 中有两类 bank：
- **初始化 bank**——包含工程所有一般信息的特殊 bank，包括有关总线层级结构的信息和有关state、switch、RTPC 和环境效果的信息。Wwise 在生成 SoundBank 时将自动创建初始化 bank。初始化 bank 通常在游戏开始时立即加载，因此在游戏期间可以轻松访问所有一般工程信息。在默认情况下，初始化 bank 被命名为“Init.bnk”。- **SoundBank**——包含事件数据、声音和振动结构数据和/或媒体的文件。SoundBank不同于初始化 bank，一般在游戏的不同时间点加载和卸载，这样可以提高平台内存利用率。事件与工程结构元数据也可以和媒体放到不同的 SoundBank 中，这样您可以只在需要时才加载媒体文件。由于所有平台各不相同，因此Wwise 允许您轻松地为每款平台量身定制 SoundBank，并同时生成所有平台的 SoundBank。Wwise 还为您提供排查 SoundBank 相关故障的工具，确保您符合不同平台的要求。
为帮助您更加高效地工作，Wwise 中提供了 SoundBank 界面布局。此布局包括为您的项目创建、管理和生成 SoundBank 所需要的所有视图，包括 SoundBank Manager（声音库管理器）、SoundBank Editor（声音库编辑器）、Project Explorer（工程浏览
器）和 Event Viewer（事件查看器）。
## File Packager（文件打包器）
为 Wwise 工程生成的 SoundBank 和任何媒体流文件可使用 File Packager 这个独立的实用工具打包。文件包（file package）将文件系统抽象出来，封装成为独立单元，这意味着您可以避免平台具体文件系统的某些限制，包括文件名长度和实际文件数量。文件包还可以帮助您更好地管理游戏的多语言版本以及游戏发布后提供的可下载内容。