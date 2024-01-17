# 事件 Event

<tldr>
基础的事件类型定义与介绍。
</tldr>

<note>

文档介绍的内容是经过**简化**、且非 **“实时”** 的。
如果你希望阅读更详细、更贴合版本真实情况的描述，
请前往参阅 [API文档引导站](https://docs.simbot.forte.love)
中相关内容的 `KDoc`。

</note>

## 基本事件类型

事件类型中，比较基础、接近根部的类型。

<deflist>
<def title="Event" id="d-event">

事件。是所有事件类型的老父亲。

> 所有其他事件都直接或间接继承 `Event`，
> 在后续的类型定义中将不再赘述。

属性：

<deflist type="medium">
<def title="id">

`ID` 类型，事件的ID。

根据不同的场景，它可能是真实的，也可能是随机的。

</def>
<def title="time">

`Timestamp` 类型，事件发生的时间或此事件被接收到的时间。

</def>
</deflist>

</def>
<def title="ComponentEvent" id="d-component-event">

代表一个含有
<tooltip term="组件标识"><code>Component</code></tooltip>
信息的事件。

属性：

<deflist type="medium">
<def title="component">

`Component` 类型，事件所属组件的组件标识。

</def>
</deflist>

</def>
<def title="BotEvent" id="d-bot-event">

继承 [`ComponentEvent`](#d-component-event)，
代表一个含有 `Bot` 的事件。

属性：

<deflist type="medium">
<def title="bot">

`Bot` 类型，事件所属或源自的 `Bot`。

</def>
</deflist>

</def>
<def title="ContentEvent" id="d-content-event">

存在一个 **主要事件中心 (`content`)** 的事件类型。

属性：

<deflist type="medium">
<def title="content">

`Any?` 类型，也就是可能为任何类型，代表这个事件的**主要事件中心**。

> `content` 的具体类型主要由其他具体实现类来覆盖定义。

</def>
</deflist>

</def>
<def title="SourceEvent" id="d-source-event">

存在一个 **源头 (`source`)** 的事件类型。
通常与 [`ContentEvent`](#d-content-event) 配合实现。

属性：

<deflist type="medium">
<def title="source">

`Any?` 类型，也就是可能为任何类型，代表这个事件的 **“源头”** 。

> `source` 的具体类型主要由其他具体实现类来覆盖定义。

</def>
</deflist>

</def>
<def title="ChangeEvent" id="d-change-event">

继承 [`ContentEvent`](#d-content-event)，
发生了某种变化的事件。

属性：

<deflist type="medium">
<def title="content">


`Any?` 类型，也就是可能为任何类型，代表这个事件的 **发生了变化的主体。** 。

> `content` 继承自 [`ContentEvent`](#d-content-event)。

</def>
</deflist>
</def>
</deflist>

## 行为对象 Actor 相关事件

一些与
<tooltip term="行为对象">行为对象</tooltip>
相关、并将其视为**事件中心**的事件类型。

<deflist>
<def title="ActorEvent" id="d-actor-event">

所有**行为对象事件**的统一父类，继承 [BotEvent](#d-bot-event)、[ContentEvent](#d-content-event)。

> **本节内**所有其他事件都继承 `ActorEvent`，在后续的类型定义中将不再赘述。

属性：

<deflist type="medium">
<def title="content">


`Actor` 类型，被作为事件中心的
<tooltip term="行为对象">行为对象</tooltip>。

> `content` 继承自 [`ContentEvent`](#d-content-event)。
>
> 本节内后续出现的此属性将不再赘述此说明。

</def>
</deflist>

</def>
<def title="ContactEvent">

以
<tooltip term="联系人">联系人</tooltip>
为中心的事件。

属性：

<deflist type="medium">
<def title="content">

`Contact` 类型，被作为事件中心的
<tooltip term="联系人">联系人</tooltip>。

</def>
</deflist>

</def>
<def title="OrganizationEvent" id="d-organization-event">

以
<tooltip term="组织">组织</tooltip>
为中心的事件。

属性：

<deflist type="medium">
<def title="content">

`Organization` 类型，被作为事件中心的
<tooltip term="组织">组织</tooltip>。

</def>
</deflist>

</def>
<def title="ChatRoomEvent">

以
<tooltip term="聊天室">聊天室</tooltip>
为中心的事件。

属性：

<deflist type="medium">
<def title="content">

`ChatRoom` 类型，被作为事件中心的
<tooltip term="聊天室">聊天室</tooltip>。

</def>
</deflist>

</def>
<def title="ChatGroupEvent">

以
<tooltip term="聊天群">聊天群</tooltip>
为中心的事件。

属性：

<deflist type="medium">
<def title="content">

`ChatGroup` 类型，被作为事件中心的
<tooltip term="聊天群">聊天群</tooltip>。

</def>
</deflist>

</def>
<def title="GuildEvent">

以
<tooltip term="频道服务器">频道服务器</tooltip>
为中心的事件。

属性：

<deflist type="medium">
<def title="content">

`Guild` 类型，被作为事件中心的
<tooltip term="频道服务器">频道服务器</tooltip>。

</def>
</deflist>

</def>
<def title="ChannelEvent">

以
<tooltip term="频道">频道</tooltip>
为中心的事件。

属性：

<deflist type="medium">
<def title="guild">

`Guild` 类型，此事件中心的频道所属的
<tooltip term="频道服务器">频道服务器</tooltip>。

</def>
<def title="content">

`Channel` 类型，被作为事件中心的
<tooltip term="频道">频道</tooltip>。

</def>
</deflist>

</def>
<def title="ChatChannelEvent">

以
<tooltip term="聊天频道">聊天频道</tooltip>
为中心的事件。

属性：

<deflist type="medium">
<def title="guild">

`Guild` 类型，此事件中心的频道所属的
<tooltip term="频道服务器">频道服务器</tooltip>。

</def>
<def title="content">

`ChatChannel` 类型，被作为事件中心的
<tooltip term="聊天频道">聊天频道</tooltip>。

</def>
</deflist>

</def>
<def title="OrganizationSourceEvent">

以
<tooltip term="组织">组织</tooltip>
作为源头、但并非事件主要主体的事件类型。

> 此类型不实现 [ActorEvent](#d-actor-event)，
> 但会与下述部分 [MemberEvent](#d-member-event) 相关类型的事件相互配合。

属性：

<deflist type="medium">
<def title="source">

`Organization` 类型，事件中心的所属源头
<tooltip term="组织">组织</tooltip>。

</def>
</deflist>

</def>
<def title="MemberEvent" id="d-member-event">

以
<tooltip term="组织成员">组织成员</tooltip>
为中心的事件。

属性：

<deflist type="medium">
<def title="source">

`Organization` 类型，此事件中心的组织成员所属的源头
<tooltip term="组织">组织</tooltip>。

</def>
<def title="content">

`Member` 类型，被作为事件中心的
<tooltip term="组织成员">组织成员</tooltip>。

</def>
</deflist>

</def>
<def title="ChatGroupMemberEvent" id="d-group-member-event">

以一个
<tooltip term="聊天群">聊天群</tooltip>
中的
<tooltip term="组织成员">组织成员</tooltip>
为中心的事件。

属性：

<deflist type="medium">
<def title="source">

`ChatGroup` 类型，此事件中心的组织成员所属的源头
<tooltip term="聊天群">聊天群</tooltip>。

</def>
<def title="content">

`Member` 类型，被作为事件中心的
<tooltip term="组织成员">组织成员</tooltip>。

</def>
</deflist>

</def>
<def title="GuildMemberEvent" id="d-guild-member-event">

以一个
<tooltip term="频道">频道</tooltip>
中的
<tooltip term="组织成员">组织成员</tooltip>
为中心的事件。

属性：

<deflist type="medium">
<def title="source">

`Guild` 类型，此事件中心的组织成员所属的源头
<tooltip term="频道">频道</tooltip>。

</def>
<def title="content">

`Member` 类型，被作为事件中心的
<tooltip term="组织成员">组织成员</tooltip>。

</def>
</deflist>
</def>
</deflist>

## 请求相关事件

一些与**申请、请求**或相似概念相关的事件类型。

<deflist>
<def title="RequestEvent">

`Bot` 收到的某种与请求/申请有关的事件。继承 [BotEvent](#d-bot-event)。

> 本节内所有事件均继承 `RequestEvent`，下文将不再赘述。

<deflist type="medium">
<def title="message">

`String?` 类型，伴随请求的附加消息。可能为 `null`。

</def>
<def title="type">

`RequestEvent.Type` 枚举类型，此申请的主动或被动类型。

元素：

- `PROACTIVE`
- `PASSIVE`

</def>
<def title="reject()">拒绝此请求。</def>
<def title="accept()">接受此请求。</def>
</deflist>

</def>
<def title="OrganizationRequestEvent">

`Bot` 收到的某种与
<tooltip term="组织">组织</tooltip>
相关的请求/申请有关的事件。

继承 [OrganizationEvent](#d-organization-event)。

</def>
<def title="OrganizationJoinRequestEvent">

某个用户想要加入目标
<tooltip term="组织">组织</tooltip>
的请求事件。

<deflist type="medium">
<def title="requesterId">

`ID` 类型，申请人的 ID。

</def>
<def title="requester">

`User?` 类型，尝试获取申请者的一些基础信息。
如果无法获取则可能为 `null`。

</def>
</deflist>

</def>
<def title="ChatGroupJoinRequestEvent">

某用户申请加入
<tooltip term="聊天群">聊天群</tooltip>
的事件。

<deflist type="medium">
<def title="content">

`ChatGroup` 类型，被申请的
<tooltip term="聊天群">聊天群</tooltip>。

</def>
</deflist>

</def>
<def title="GuildJoinRequestEvent">

某用户申请加入
<tooltip term="频道">频道</tooltip>
的事件。

<deflist type="medium">
<def title="content">

`Guild` 类型，被申请的
<tooltip term="频道">频道</tooltip>。

</def>
</deflist>

</def>
</deflist>

## 成员变动事件

一些与
<tooltip term="组织成员">组织成员</tooltip>
发生了**变化、变动**或相似概念相关的事件类型。

<deflist>
<def title="MemberChangeEvent">

当
<tooltip term="组织成员">组织成员</tooltip>
发生了某种变化时的事件。

> 本节内的事件均继承 `MemberChangeEvent`，下文将不再赘述。

<deflist type="medium">
<def title="content">

`Member` 类型，发生了变化的
<tooltip term="组织成员">组织成员</tooltip>。

</def>
</deflist>

</def>
<def title="GuildMemberChangeEvent">

当
<tooltip term="频道">频道</tooltip>
的
<tooltip term="组织成员">组织成员</tooltip>
发生了某种变化时的事件。

继承 [GuildMemberEvent](#d-guild-member-event)。

<deflist type="medium">
<def title="source">

`Guild` 类型，发生变化所在的
<tooltip term="频道">频道</tooltip>。

</def>
<def title="content">

`Member` 类型，发生了变化的
<tooltip term="组织成员">组织成员</tooltip>。

</def>
</deflist>

</def>
<def title="GroupMemberChangeEvent">

当
<tooltip term="聊天群">聊天群</tooltip>
的
<tooltip term="组织成员">组织成员</tooltip>
发生了某种变化时的事件。

继承 [GroupMemberEvent](#d-group-member-event)。

<deflist type="medium">
<def title="source">

`ChatGroup` 类型，发生变化所在的
<tooltip term="聊天群">聊天群</tooltip>。

</def>
<def title="content">

`Member` 类型，发生了变化的
<tooltip term="组织成员">组织成员</tooltip>。

</def>
</deflist>

</def>
</deflist>

## 组织变动事件

一些与
<tooltip term="组织">组织</tooltip>
发生了**变化、变动**或相似概念相关的事件类型。

<deflist>
<def title="OrganizationChangeEvent">

某
<tooltip term="组织">组织</tooltip>
产生了某种变化的事件。

继承 [ChangeEvent](#d-change-event) ，[OrganizationEvent](#d-organization-event)。

> 本节内所有事件均继承 `OrganizationChangeEvent`，下文将不再赘述。

<deflist type="medium">
<def title="content">

`Organization` 类型，发生了变化的
<tooltip term="组织">组织</tooltip>。

</def>
</deflist>

</def>
<def title="MemberIncreaseOrDecreaseEvent">
某组织成员增加或减少的事件。

<deflist type="medium">
<def title="content">

`Organization` 类型，增加或减少成员的
<tooltip term="组织">组织</tooltip>。

返回类型会根据实现类的场景而变化。

</def>
<def title="member">

`Member?` 类型，增加或减少的
<tooltip term="组织成员">组织成员</tooltip>。
如不支持获取，则可能得到 null 。

</def>
</deflist>

</def>
<def title="MemberIncreaseEvent">
某组织成员增加事件。继承 <code>MemberIncreaseOrDecreaseEvent</code>。
</def>
<def title="MemberDecreaseEvent">
某组织成员减少事件。继承 <code>MemberIncreaseOrDecreaseEvent</code>。
</def>
<def title="ChatGroupMemberIncreaseOrDecreaseEvent">
某 
<tooltip term="聊天群">聊天群</tooltip>
成员变动事件。
</def>
<def title="ChatGroupMemberIncreaseEvent">
某 
<tooltip term="聊天群">聊天群</tooltip>
成员增加事件。继承 <code>ChatGroupMemberIncreaseOrDecreaseEvent</code>。
</def>
<def title="ChatGroupMemberDecreaseEvent">
某 
<tooltip term="聊天群">聊天群</tooltip>
成员减少事件。继承 <code>ChatGroupMemberIncreaseOrDecreaseEvent</code>。
</def>
<def title="GuildMemberIncreaseOrDecreaseEvent">
某 
<tooltip term="频道">频道</tooltip>
成员变动事件。
</def>
<def title="GuildMemberIncreaseEvent">
某 
<tooltip term="频道">频道</tooltip>
成员增加事件。继承 <code>GuildMemberIncreaseOrDecreaseEvent</code>。
</def>
<def title="GuildMemberDecreaseEvent">
某 
<tooltip term="频道">频道</tooltip>
成员减少事件。继承 <code>GuildMemberIncreaseOrDecreaseEvent</code>。
</def>
</deflist>

## 消息事件

一些与**Bot收到消息**相关的事件类型。

<deflist>
<def title="MessageEvent">

一个 `Bot` 收到消息的事件。继承 [BotEvent](#d-bot-event)、`ReplySupport`。

> 本节内所有事件均继承 `MessageEvent`，下文将不再赘述。

<deflist type="medium">
<def title="authorId">

`ID` 类型，这个消息的发送人ID。

</def>
<def title="messageContent">

`MessageContent` 类型，事件中收到的消息内容。

</def>
<def title="reply(...)">

基于此事件收到的消息进行回复。
来自 `ReplySupport` 接口。

</def>
</deflist>
</def>
<def title="ChatRoomMessageEvent">
一个 <code>Bot</code> 从 
<tooltip term="聊天室">聊天室</tooltip>
处收到消息的事件。
</def>
<def title="ChatGroupMessageEvent">
一个 <code>Bot</code> 从 
<tooltip term="聊天群">聊天群</tooltip>
处收到消息的事件。
</def>
<def title="ChatChannelMessageEvent">
一个 <code>Bot</code> 从 
<tooltip term="聊天频道">聊天频道</tooltip>
处收到消息的事件。
</def>
<def title="MemberMessageEvent">
一个 <code>Bot</code> 从 
<tooltip term="组织成员">组织成员</tooltip>
处收到消息的事件。
</def>
<def title="ChatGroupMemberMessageEvent">
一个 <code>Bot</code> 从 
<tooltip term="聊天群">聊天群</tooltip>
中的
<tooltip term="组织成员">组织成员</tooltip>
处收到消息的事件。
</def>
<def title="GuildMemberMessageEvent">
一个 <code>Bot</code> 从 
<tooltip term="频道">频道</tooltip>
中的
<tooltip term="组织成员">组织成员</tooltip>
处收到消息的事件。
</def>
<def title="ContactMessageEvent">
一个 <code>Bot</code> 从 
<tooltip term="联系人">联系人</tooltip>
处收到消息的事件。
</def>
</deflist>