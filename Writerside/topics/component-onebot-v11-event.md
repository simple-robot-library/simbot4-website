# 事件

## 原始事件

我们根据 OneBot11 协议中定义的所有事件结构，
在模块 `simbot-component-onebot-v11-event` 中提供了所有的实现类型。
它们是最基础的可序列化数据类，不含有任何功能。

<deflist>
<def id="RawMetaEvent" title="元事件 RawMetaEvent">

[元事件](https://github.com/botuniverse/onebot-11/blob/master/event/meta.md)

<deflist>
<def id="RawHeartbeatEvent" title="RawHeartbeatEvent">

[心跳](https://github.com/botuniverse/onebot-11/blob/master/event/meta.md#心跳)

</def>
<def id="RawLifecycleEvent" title="RawLifecycleEvent">

[生命周期](https://github.com/botuniverse/onebot-11/blob/master/event/meta.md#生命周期)

</def>
</deflist>
</def>
<def id="RawMessageEvent" title="消息事件 RawMessageEvent">

[消息事件](https://github.com/botuniverse/onebot-11/blob/master/event/message.md#消息事件)

<deflist>
<def id="RawGroupMessageEvent" title="RawGroupMessageEvent">

[群消息事件](https://github.com/botuniverse/onebot-11/blob/master/event/message.md#群消息)

</def>
<def id="RawPrivateMessageEvent" title="RawPrivateMessageEvent">

[私聊消息事件](https://github.com/botuniverse/onebot-11/blob/master/event/message.md#私聊消息)

</def>
</deflist>
</def>
<def id="RawRequestEvent" title="请求事件 RawRequestEvent">

[请求事件](https://github.com/botuniverse/onebot-11/blob/master/event/request.md)

<deflist>
<def id="RawFriendRequestEvent" title="RawFriendRequestEvent">

[加好友请求](https://github.com/botuniverse/onebot-11/blob/master/event/request.md#加好友请求)

</def>
<def id="RawGroupRequestEvent" title="RawGroupRequestEvent">

[加群请求／邀请](https://github.com/botuniverse/onebot-11/blob/master/event/request.md#加群请求邀请)

</def>
</deflist>
</def>
<def id="RawNoticeEvent" title="通知事件 RawNoticeEvent">

[通知事件](https://github.com/botuniverse/onebot-11/blob/master/event/notice.md)

<deflist>
<def id="RawFriendAddEvent" title="RawFriendAddEvent">

[好友添加](https://github.com/botuniverse/onebot-11/blob/master/event/notice.md#好友添加)

</def>
<def id="RawFriendRecallEvent" title="RawFriendRecallEvent">

[好友消息撤回](https://github.com/botuniverse/onebot-11/blob/master/event/notice.md#好友消息撤回)

</def>
<def id="RawGroupAdminEvent" title="RawGroupAdminEvent">

[群管理员变动](https://github.com/botuniverse/onebot-11/blob/master/event/notice.md#群管理员变动)

</def>
<def id="RawGroupBanEvent" title="RawGroupBanEvent">

[群禁言](https://github.com/botuniverse/onebot-11/blob/master/event/notice.md#群禁言)

</def>
<def id="RawGroupDecreaseEvent" title="RawGroupDecreaseEvent">

[群成员减少](https://github.com/botuniverse/onebot-11/blob/master/event/notice.md#群成员减少)

</def>
<def id="RawGroupIncreaseEvent" title="RawGroupIncreaseEvent">

[群成员增加](https://github.com/botuniverse/onebot-11/blob/master/event/notice.md#群成员增加)

</def>
<def id="RawGroupRecallEvent" title="RawGroupRecallEvent">

[群消息撤回](https://github.com/botuniverse/onebot-11/blob/master/event/notice.md#群消息撤回)

</def>
<def id="RawGroupUploadEvent" title="RawGroupUploadEvent">

[群文件上传](https://github.com/botuniverse/onebot-11/blob/master/event/notice.md#群文件上传)

</def>
<def id="RawNotifyEvent" title="RawNotifyEvent">

[群成员荣誉变更](https://github.com/botuniverse/onebot-11/blob/master/event/notice.md#群成员荣誉变更)、
[群红包运气王](https://github.com/botuniverse/onebot-11/blob/master/event/notice.md#群红包运气王)、
[群内戳一戳](https://github.com/botuniverse/onebot-11/blob/master/event/notice.md#群内戳一戳)
等通知相关的事件。

</def>
</deflist>
</def>
<def id="UnknownEvent" title="未知事件 UnknownEvent">

一个特殊的事件类型。
它是当遇到无法被上述这些类型所解析时使用的兜底类型，比如遇到未知的
`type`
或由于各种原因导致反序列化失败。

它不可序列化，除 `time`、`selfId`、`postType`
之外直接提供 `raw` 属性(也就是原始的JSON字符串)。

</def>
</deflist>

## 组件事件

组件实现是基于simbot标准API中定义的事件类型、对上述**原始事件**的包装，
并提供相对应的功能性API实现。

<deflist>
<def id="OneBotMessageEvent" title="OneBotMessageEvent">
与消息相关的事件。

<deflist>
<def id="OneBotGroupMessageEvent" title="OneBotGroupMessageEvent">
与群消息相关的事件。

<deflist>
<def id="OneBotNormalGroupMessageEvent" title="OneBotNormalGroupMessageEvent">
与普通群消息相关的事件。
</def>
<def id="OneBotAnonymousGroupMessageEvent" title="OneBotAnonymousGroupMessageEvent">
与匿名群消息相关的事件。
</def>
<def id="OneBotNoticeGroupMessageEvent" title="OneBotNoticeGroupMessageEvent">
与群系统消息通知相关的事件。
</def>
</deflist>

</def>
<def id="OneBotPrivateMessageEvent" title="OneBotPrivateMessageEvent">
与私聊消息相关的事件。

<deflist>
<def id="OneBotFriendMessageEvent" title="OneBotFriendMessageEvent">
好友私信消息事件。
</def>
<def id="OneBotGroupPrivateMessageEvent" title="OneBotGroupPrivateMessageEvent">
群成员临时会话消息事件。
</def>
</deflist>

</def>
</deflist>

</def>
<def id="OneBotMetaEvent" title="OneBotMetaEvent">
元数据相关的事件
<deflist>
<def id="OneBotLifecycleEvent" title="OneBotLifecycleEvent">
生命周期事件
</def>
<def id="OneBotHeartbeatEvent" title="OneBotHeartbeatEvent">
心跳事件
</def>
</deflist>
</def>
<def id="OneBotRequestEvent" title="OneBotRequestEvent">
请求相关的事件
<deflist>
<def id="OneBotFriendRequestEvent" title="OneBotFriendRequestEvent">
好友添加申请
</def>
<def id="OneBotGroupRequestEvent" title="OneBotGroupRequestEvent">
群添加申请
</def>
</deflist>
</def>
<def id="OneBotNoticeEvent" title="OneBotNoticeEvent">
通知相关的事件。
<deflist>
<def id="OneBotFriendAddEvent" title="OneBotFriendAddEvent">好友已添加事件</def>
<def id="OneBotFriendRecallEvent" title="OneBotFriendRecallEvent">好友消息撤回事件</def>
<def id="OneBotGroupAdminEvent" title="OneBotGroupAdminEvent">管理员变动事件</def>
<def id="OneBotGroupBanEvent" title="OneBotGroupBanEvent">群禁言事件</def>
<def id="OneBotGroupChangeEvent" title="OneBotGroupChangeEvent">群成员变动事件</def>
<def id="OneBotGroupMemberIncreaseEvent" title="OneBotGroupMemberIncreaseEvent">群成员添加事件</def>
<def id="OneBotGroupMemberDecreaseEvent" title="OneBotGroupMemberDecreaseEvent">群成员离开事件</def>
<def id="OneBotGroupRecallEvent" title="OneBotGroupRecallEvent">群消息撤回事件</def>
<def id="OneBotGroupUploadEvent" title="OneBotGroupUploadEvent">群文件上传事件</def>
<def id="OneBotNotifyEvent" title="OneBotNotifyEvent">
群荣耀事件、红包人气王事件或戳一戳事件。

<warning title="实现接口的变更">

v1.4.0 之前，`OneBotNotifyEvent` 实现 `MemberEvent` ，
从 `v1.4.0` 开始则不再实现 `MemberEvent`，并独立出一个下述的 `OneBotGroupNotifyEvent` 类型，
改为由此类型实现 `MemberEvent`。

</warning>

<deflist>
<def title="OneBotGroupNotifyEvent" id="OneBotGroupNotifyEvent" collapsible="true">

`v1.4.0` 后添加，是 `OneBotNotifyEvent` 的子类型，用来描述那些发生在群里的通知事件。

<deflist>
<def id="OneBotHonorEvent" title="OneBotHonorEvent">群荣耀事件</def>
<def id="OneBotLuckyKingEvent" title="OneBotLuckyKingEvent">红包人气王事件</def>
<def id="OneBotPokeEvent" title="OneBotPokeEvent">戳一戳事件</def>
<def id="OneBotMemberPokeEvent" title="OneBotMemberPokeEvent">戳一戳(普通群成员被戳)事件</def>
<def id="OneBotBotSelfPokeEvent" title="OneBotBotSelfPokeEvent">戳一戳(Bot被戳)事件</def>
</deflist>

</def>
<def title="OneBotPrivatePokeEvent" id="OneBotPrivatePokeEvent">

`v1.4.0` 后添加，用来描述私聊里的戳一戳事件，实现 `OneBotPokeEvent`。
此事件中的 `groupId` 始终为 `null`。

</def>
</deflist>

</def>
</deflist>
</def>

<def id="OneBotInternalEvent" title="OneBotInternalEvent">
与OneBot协议本身无关的用于内部流转或拦截的事件。
<deflist>
<def id="OneBotBotStageEvent" title="OneBotBotStageEvent">
与OneBot协议本身无关的Bot的阶段事件。
<deflist>
<def id="OneBotBotRegisteredEvent" title="OneBotBotRegisteredEvent">
一个 `OneBotBot` 被注册了的事件
</def>
<def id="OneBotBotStartedEvent" title="OneBotBotStartedEvent">
一个 `OneBotBot` 被(首次)启动了的事件
</def>
</deflist>
</def>

<def id="OneBotInternalMessageInteractionEvent" title="OneBotInternalMessageInteractionEvent">

OneBot 组件中与 Message 交互有关的事件。

> 自 `v1.6.0` 起添加。可参考 [OneBot组件#177](https://github.com/simple-robot/simbot-component-onebot/pull/177)。

**基础类型**

<deflist>
<def id="OneBotInternalMessagePreSendEvent" title="OneBotInternalMessagePreSendEvent">
OneBot 组件针对消息发送前的拦截事件。
继承 <code>InternalMessagePreSendEvent</code>
</def>
<def id="OneBotInternalMessagePostSendEvent" title="OneBotInternalMessagePostSendEvent">
OneBot 组件针对消息发送后的通知事件。
继承 <code>InternalMessagePostSendEvent</code>
</def>
</deflist>

**`SendSupport` 相关类型**

<deflist>
<def id="OneBotSendSupportInteractionEvent" title="OneBotSendSupportInteractionEvent">
<code>SendSupport.send</code> 的行为事件
</def>
<def id="OneBotSendSupportPreSendEvent" title="OneBotSendSupportPreSendEvent">
<code>SendSupport.send</code> 的拦截事件
</def>
<def id="OneBotSendSupportPostSendEvent" title="OneBotSendSupportPostSendEvent">
<code>SendSupport.send</code> 的通知事件
</def>
</deflist>

**`SendSupport` 相关类型细分子类型**

<deflist>
<def id="OneBotGroupInteractionEvent" title="OneBotGroupInteractionEvent">
<code>OneBotGroup.send</code> 的行为事件
<deflist>
<def id="OneBotGroupPreSendEvent" title="OneBotGroupPreSendEvent">
<code>OneBotGroup.send</code> 的拦截事件
</def>
<def id="OneBotGroupPostSendEvent" title="OneBotGroupPostSendEvent">
<code>OneBotGroup.send</code> 的通知事件
</def>
</deflist>
</def>
<def id="OneBotFriendInteractionEvent" title="OneBotFriendInteractionEvent">
<code>OneBotFriend.send</code> 的行为事件
<deflist>
<def id="OneBotFriendPreSendEvent" title="OneBotFriendPreSendEvent">
<code>OneBotFriend.send</code> 的拦截事件
</def>
<def id="OneBotFriendPostSendEvent" title="OneBotFriendPostSendEvent">
<code>OneBotFriend.send</code> 的通知事件
</def>
</deflist>
</def>
<def id="OneBotMemberInteractionEvent" title="OneBotMemberInteractionEvent">
<code>OneBotMember.send</code> 的行为事件
<deflist>
<def id="OneBotMemberPreSendEvent" title="OneBotMemberPreSendEvent">
<code>OneBotMember.send</code> 的拦截事件
</def>
<def id="OneBotMemberPostSendEvent" title="OneBotMemberPostSendEvent">
<code>OneBotMember.send</code> 的通知事件
</def>
</deflist>
</def>
</deflist>

**`ReplySupport` (`MessageEvent`) 相关类型**

<deflist>
<def id="OneBotMessageEventInteractionEvent" title="OneBotMessageEventInteractionEvent">
<code>OneBotMessageEvent.reply</code> 的行为事件
</def>
<def id="OneBotMessageEventPreReplyEvent" title="OneBotMessageEventPreReplyEvent">
<code>OneBotMessageEvent.reply</code> 的拦截事件
</def>
<def id="OneBotMessageEventPostReplyEvent" title="OneBotMessageEventPostReplyEvent">
<code>OneBotMessageEvent.reply</code> 的通知事件
</def>
</deflist>

**`ReplySupport` (`MessageEvent`) 相关类型细分子类型**

**群聊相关**

<deflist>
<def id="OneBotGroupMessageEventInteractionEvent" title="OneBotGroupMessageEventInteractionEvent">
<code>OneBotGroupMessageEvent.reply</code> 的行为事件
</def>
<def id="OneBotGroupMessageEventPreReplyEvent" title="OneBotGroupMessageEventPreReplyEvent">
<code>OneBotGroupMessageEvent.reply</code> 的拦截事件
</def>
<def id="OneBotGroupMessageEventPostReplyEvent" title="OneBotGroupMessageEventPostReplyEvent">
<code>OneBotGroupMessageEvent.reply</code> 的通知事件
</def>
<def id="OneBotNormalGroupMessageEventInteractionEvent" title="OneBotNormalGroupMessageEventInteractionEvent">
<code>OneBotNormalGroupMessageEvent.reply</code> 的行为事件
</def>
<def id="OneBotNormalGroupMessageEventPreReplyEvent" title="OneBotNormalGroupMessageEventPreReplyEvent">
<code>OneBotNormalGroupMessageEvent.reply</code> 的拦截事件
</def>
<def id="OneBotNormalGroupMessageEventPostReplyEvent" title="OneBotNormalGroupMessageEventPostReplyEvent">
<code>OneBotNormalGroupMessageEvent.reply</code> 的通知事件
</def>
<def id="OneBotAnonymousGroupMessageEventInteractionEvent" title="OneBotAnonymousGroupMessageEventInteractionEvent">
<code>OneBotAnonymousGroupMessageEvent.reply</code> 的行为事件
</def>
<def id="OneBotAnonymousGroupMessageEventPreReplyEvent" title="OneBotAnonymousGroupMessageEventPreReplyEvent">
<code>OneBotAnonymousGroupMessageEvent.reply</code> 的拦截事件
</def>
<def id="OneBotAnonymousGroupMessageEventPostReplyEvent" title="OneBotAnonymousGroupMessageEventPostReplyEvent">
<code>OneBotAnonymousGroupMessageEvent.reply</code> 的通知事件
</def>
<def id="OneBotNoticeGroupMessageEventInteractionEvent" title="OneBotNoticeGroupMessageEventInteractionEvent">
<code>OneBotNoticeGroupMessageEvent.reply</code> 的行为事件
</def>
<def id="OneBotNoticeGroupMessageEventPreReplyEvent" title="OneBotNoticeGroupMessageEventPreReplyEvent">
<code>OneBotNoticeGroupMessageEvent.reply</code> 的拦截事件
</def>
<def id="OneBotNoticeGroupMessageEventPostReplyEvent" title="OneBotNoticeGroupMessageEventPostReplyEvent">
<code>OneBotNoticeGroupMessageEvent.reply</code> 的通知事件
</def>
</deflist>

**私聊相关**

<deflist>
<def id="OneBotPrivateMessageEventInteractionEvent" title="OneBotPrivateMessageEventInteractionEvent"> 
<code>OneBotPrivateMessageEvent.reply</code> 的行为类型
</def>
<def id="OneBotPrivateMessageEventPreReplyEvent" title="OneBotPrivateMessageEventPreReplyEvent"> 
<code>OneBotPrivateMessageEvent.reply</code> 的拦截类型
</def>
<def id="OneBotPrivateMessageEventPostReplyEvent" title="OneBotPrivateMessageEventPostReplyEvent"> 
<code>OneBotPrivateMessageEvent.reply</code> 的通知类型
</def>
<def id="OneBotGroupPrivateMessageEventInteractionEvent" title="OneBotGroupPrivateMessageEventInteractionEvent"> 
<code>OneBotGroupPrivateMessageEvent.reply</code> 的行为类型
</def>
<def id="OneBotGroupPrivateMessageEventPreReplyEvent" title="OneBotGroupPrivateMessageEventPreReplyEvent"> 
<code>OneBotGroupPrivateMessageEvent.reply</code> 的拦截类型
</def>
<def id="OneBotGroupPrivateMessageEventPostReplyEvent" title="OneBotGroupPrivateMessageEventPostReplyEvent"> 
<code>OneBotGroupPrivateMessageEvent.reply</code> 的通知类型
</def>
<def id="OneBotFriendMessageEventInteractionEvent" title="OneBotFriendMessageEventInteractionEvent"> 
<code>OneBotFriendMessageEvent.reply</code> 的行为类型
</def>
<def id="OneBotFriendMessageEventPreReplyEvent" title="OneBotFriendMessageEventPreReplyEvent"> 
<code>OneBotFriendMessageEvent.reply</code> 的拦截类型
</def>
<def id="OneBotFriendMessageEventPostReplyEvent" title="OneBotFriendMessageEventPostReplyEvent"> 
<code>OneBotFriendMessageEvent.reply</code> 的通知类型
</def>
</deflist>

</def>
</deflist>

</def>

</deflist>

### 未知事件

`OneBotUnknownEvent`

是对上述原始事件中的 `UnknownEvent` 类型的包装。

### 未支持事件

`OneBotUnsupportedEvent`

是当出现了原始事件中有(除了 `UnknownEvent`)、
但是尚未提供对应的**组件事件**实现的事件类型时用来兜底的类型。

这些原始事件类型会被统一装入此事件中。

## 事件关系

简单列举一下原始事件与可能对应的组件事件之间的关系。

| 原始事件类型                                              | 组件事件                                                       |
|-----------------------------------------------------|------------------------------------------------------------|
| `RawMetaEvent`                                      | `OneBotMetaEvent`                                          |
| > `RawLifecycleEvent`                               | > `OneBotLifecycleEvent`                                   |
| > `RawHeartbeatEvent`                               | > `OneBotHeartbeatEvent`                                   |
| `RawMessageEvent`                                   | `OneBotMessageEvent`                                       |
| > `RawGroupMessageEvent`                            | > `OneBotGroupMessageEvent`                                |
| > `RawGroupMessageEvent`                            | > > `OneBotNormalGroupMessageEvent`                        |
| > `RawGroupMessageEvent`                            | > > `OneBotAnonymousGroupMessageEvent`                     |
| > `RawGroupMessageEvent`                            | > > `OneBotNoticeGroupMessageEvent`                        |
| > `RawPrivateMessageEvent`                          | > `OneBotPrivateMessageEvent`                              |
| > `RawPrivateMessageEvent`                          | > > `OneBotFriendMessageEvent`                             |
| > `RawPrivateMessageEvent`                          | > > `OneBotGroupPrivateMessageEvent`                       |
| `RawRequestEvent`                                   | `OneBotRequestEvent`                                       |
| > `RawFriendRequestEvent`                           | > `OneBotFriendRequestEvent`                               |
| > `RawGroupRequestEvent`                            | > `OneBotGroupRequestEvent`                                |
| `RawNoticeEvent`                                    | `OneBotNoticeEvent`                                        |
| > `RawFriendAddEvent`                               | > `OneBotFriendAddEvent`                                   |
| > `RawFriendRecallEvent`                            | > `OneBotFriendRecallEvent`                                |
| > `RawGroupAdminEvent`                              | > `OneBotGroupAdminEvent`                                  |
| > `RawGroupBanEvent`                                | > `OneBotGroupBanEvent`                                    |
| > `RawGroupIncreaseEvent` 或 `RawGroupDecreaseEvent` | > `OneBotGroupChangeEvent`                                 |
| > `RawGroupIncreaseEvent`                           | > > `OneBotGroupMemberIncreaseEvent`                       |
| > `RawGroupDecreaseEvent`                           | > > `OneBotGroupMemberDecreaseEvent`                       |
| > `RawGroupRecallEvent`                             | > `OneBotGroupRecallEvent`                                 |
| > `RawGroupUploadEvent`                             | > `OneBotGroupUploadEvent`                                 |
| > `RawNotifyEvent`                                  | > `OneBotNotifyEvent`                                      |
| > `RawNotifyEvent`                                  | <p> > > `OneBotGroupNotifyEvent`</p> (since `v1.4.0`)      |
| > `RawNotifyEvent`                                  | > > > `OneBotHonorEvent`                                   |
| > `RawNotifyEvent`                                  | > > > `OneBotLuckyKingEvent`                               |
| > `RawNotifyEvent`                                  | > > > `OneBotPokeEvent`                                    |
| > `RawNotifyEvent`                                  | > > > > `OneBotMemberPokeEvent`                            |
| > `RawNotifyEvent`                                  | > > > > `OneBotBotSelfPokeEvent`                           |
| > `RawNotifyEvent`                                  | <p> > > > > `OneBotPrivatePokeEvent` </p> (since `v1.4.0`) |
| `UnknownEvent`                                      | > `UnknownEvent`                                           |
| 无                                                   | `OneBotBotStageEvent`                                      |
| 无                                                   | > `OneBotBotRegisteredEvent`                               |
| 无                                                   | > `OneBotBotStartedEvent`                                  |
| 任意未支持事件                                             | `OneBotUnsupportedEvent`                                   |
