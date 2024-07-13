---
switcher-label: JavaAPI风格
---

# Bot

Bot, 一个机器人。simbot 作为一个**Bot风格**的事件调度框架, `Bot` 是一个很常见也很关键的类型。

## 注册/创建

一个 `Bot` 通常由某个 **Bot管理器** 注册创建。
有关于注册 `Bot` 的相关说明你可以前往 [**组件与插件-Bot管理器**](BotManager.md) 章节查看。

## 启动

一个被注册出来的 `Bot` 是**尚未启动**的, 你需要在适当的地方启动它, 它才会开始运作（例如开始接收事件）。

<tabs group="Code">
<tab title="Kotlin" group-key="Kotlin">

```Kotlin
val bot = botManager.register(...)
bot.start() // 启动它
```

</tab>
<tab title="Java" group-key="Java">

```Java
var bot = botManager.register(...);
var future = bot.startAsync(); // 异步中启动它
// ...
```
{switcher-key="%ja%"}

```Java
var bot = botManager.register(...);
bot.startBlocking(); // 启动它
```
{switcher-key="%jb%"}


</tab>
</tabs>

## 基本信息与能力

你可以在 `Bot` 中获取到一些基本的信息, 以及 `Bot` 会提供一些针对其生命周期的基本能力。 

<deflist>
<def title="name">
    
`Bot` 的名称。通常只有满足某些条件后（比如在 `start` 了之后）才能获取。

</def>
<def title="id">

`Bot` 的 ID。但是此ID通常是注册 Bot 时使用的某种 ID（比如 `APPID`, 
而这个 ID 并不一定就是 Bot 作为用户的 ID。

</def>
<def title="component">

`Bot` 所属的组件的
<tooltip term="组件标识">组件标识</tooltip>。

</def>
<def title="isMe(...)">
   
提供一个 `ID`, 用于判断这个 ID 是否代表当前 Bot。

</def>
<def title="isStarted">
   
判断当前 `Bot` 是否已经**启动过**至少一次。

</def>
<def title="isActive">
   
判断当前 `Bot` 是否处于活跃状态。

</def>
<def title="isCompleted">
   
判断当前 `Bot` 是否处于已完成状态。

</def>
<def title="join(...)">
   
挂起 `Bot`, 直到它被关闭。

</def>
<def title="cancel(...)">
   
关闭 `Bot`。

</def>
<def title="guildRelation">
   
`GuildRelation?` 类型, 如果不为 `null` 则说明其支持与
<tooltip term="频道">频道</tooltip>
进行交互、获取信息。

其他详细内容下文会介绍。

</def>
<def title="groupRelation">
   
`GroupRelation?` 类型, 如果不为 `null` 则说明其支持与
<tooltip term="聊天群">聊天群</tooltip>
进行交互、获取信息。

其他详细内容下文会介绍。

</def>
<def title="contactRelation">
   
`ContactRelation?` 类型, 如果不为 `null` 则说明其支持与
<tooltip term="联系人">联系人</tooltip>
进行交互、获取信息。

其他详细内容下文会介绍。

</def>
</deflist>

## 行为对象交互

### GuildRelation

与
<tooltip term="频道">频道</tooltip>
的关系交互类型, 其中包含了一些获取 `Guild` 相关信息的 API。

<deflist>
<def title="guild(...)">

根据 `ID` 寻找指定的 `Guild` 目标。

</def>
<def title="guilds">

获取 Bot 所在的所有 `Guild` 的集。

</def>
<def title="guildCount()">

获取 Bot 所在的所有 `Guild` 的数量。
不支持的情况下可能会返回 `-1` 或直接通过拉取列表计数。

</def>
</deflist>

### GroupRelation

与
<tooltip term="聊天群">聊天群</tooltip>
的关系交互类型, 其中包含了一些获取 `ChatGroup` 相关信息的 API。

<deflist>
<def title="group(...)">

根据 `ID` 寻找指定的 `ChatGroup` 目标。

</def>
<def title="groups">

获取 Bot 所在的所有 `ChatGroup` 的集。

</def>
<def title="groupCount()">

获取 Bot 所在的所有 `ChatGroup` 的数量。
不支持的情况下可能会返回 `-1` 或直接通过拉取列表计数。

</def>
</deflist>

### ContactRelation

与
<tooltip term="联系人">联系人</tooltip>
的关系交互类型, 其中包含了一些获取 `Contact` 相关信息的 API。

<deflist>
<def title="contact(...)">

根据 `ID` 寻找指定的 `Contact` 目标。

</def>
<def title="contacts">

获取 Bot 所在的所有 `Contact` 的集。

</def>
<def title="contactCount()">

获取 Bot 所在的所有 `Contact` 的数量。
不支持的情况下可能会返回 `-1` 或直接通过拉取列表计数。

</def>
</deflist>

### 额外扩展

`Bot` 的具体实现可能会根据平台的实际情况提供更多额外的、专属的API。