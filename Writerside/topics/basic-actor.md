# 行为对象 Actor

<tooltip term="行为对象">行为对象</tooltip>
代表一个跟 Bot 或其他行为对象之间具有关联、且存在一些行为交互能力的类型。

换句话说, `Actor` 和 `Bot` 都是一些具有交互功能（例如发送消息、查询/修改信息等）的充血模型对象。

> 下述其他类型均继承 `Actor`, 后续不再赘述。

<deflist>
<def title="id">

`ID` 类型, 获取这个行为对象的 ID。

</def>
</deflist>

## User

一个“用户”, 例如某个频道服务器中的成员。
不一定代表真人, 也可能是对应平台下的其他 bot 或者某种默认的系统用户。

<deflist>
<def title="name">

这个用户的名称。是与组织范围无关的名称。
当然, 除非平台限制你只能获取到组织范围内的名称（例如频道成员）。

</def>
<def title="avatar">

用户的头像链接。获取不到会是 `null`。

</def>
</deflist>

## Contact

<tooltip term="联系人">联系人</tooltip>
是一种可以与 Bot 建立独立会话、进行通讯的行为对象。 
联系人可能代表一个其他用户, 也可能代表一个与某用户关联的“会话”。

比如：QQ频道的一个“私聊会话”, 和QQ中的“好友”, 都可以视为 `Contact`。

继承 `User`。

## Organization

<tooltip term="组织">组织</tooltip>, 
是一个拥有多个 
<tooltip term="组织成员">组织成员</tooltip>
的行为主体。

<deflist>
<def title="name">

此组织的名称。

</def>
<def title="ownerId">

此组织的拥有者的ID。如果获取不到则为 `null`。

</def>
<def title="member(...)">

根据ID寻找或查询指定的成员信息。

</def>
<def title="members">

获取此组织内的所有成员集。也包括 Bot 在其中的表现, 
除非平台特性无法混淆 Bot 与 Member。

</def>
<def title="botAsMember">

Bot 在当前组织内作为成员的表现。

</def>
<def title="roles">

`Collectable<Role>` 类型, 此组织中的所有可用角色集。
有可能得到一个空的集合 —— 这说明当前组织没有角色这一概念。

</def>
</deflist>

## ChatRoom

一个
<tooltip term="聊天室">聊天室</tooltip>
是一个可以向其中发送消息的行为主体。 向聊天室发送的消息可能会被多个 组织成员 收到。

<deflist>
<def title="name">

此聊天室的名称。

</def>
<def title="send(...)">

`ChatRoom` 实现接口 `SendSupport`, 运行通过文本、消息链或事件消息体来发送消息。

> 如果平台下由于某些条件出现一些不支持 `send` 的实现, 它们会抛出 `UnsupportedOperationException`。

</def>
</deflist>

## ChatGroup

一个用于聊天的
<tooltip term="聊天室">聊天群</tooltip>。

群聊本身承载了聚集**组织成员**的职责和作为**聊天室**向其他成员发送消息的职责。

> 实现 `ChatRoom` 和 `Organization`。

## Guild

一个
<tooltip term="频道服务器">频道服务器</tooltip>
是对一组 
<tooltip term="组织成员">组织成员</tooltip>
和一组 
<tooltip term="频道">频道</tooltip> 
的统一。

> 实现 `Organization`。

<deflist>
<def title="channel(...)">

根据ID寻找一个指定的频道。如果找不到则会得到 `null`。

</def>
<def title="channels">

获取此频道服务器内的所有频道集。

</def>
<def title="chatChannel(...)">

根据ID寻找一个指定的
<tooltip term="聊天频道">聊天频道</tooltip>。
如果找不到则会得到 `null`。

</def>
<def title="chatChannels">

获取此频道服务器内的所有聊天频道集。
结果通常是 `channels` 的子集。

</def>
</deflist>

<note>

根据平台的不同特性, `Guild` 的具体实现还有可能提供更多类似的 API, 
例如QQ频道中还有“分类子频道”、“帖子子频道”等。

</note>

## Channel

<tooltip term="频道">频道</tooltip>
是
<tooltip term="频道服务器">频道服务器</tooltip>
中所有频道中的某个频道。
频道的类型可能有很多, 其中就包含了允许发送消息的
<tooltip term="聊天频道">聊天频道</tooltip>。

<deflist>
<def title="name">

频道的名称。

</def>
</deflist>

<deflist>
<def title="category">

`Category?` 类型, 此频道的分类。如果没有分类、或平台中没有分类这个概念, 则可以得到 `null`。

</def>
</deflist>

## ChatChannel

<tooltip term="聊天频道">聊天频道</tooltip>
继承了 `ChatRoom` 的 `Channel`, 代表这个频道可以用来向其他人发送消息。

## Member

一个组织内的
<tooltip term="组织成员">组织成员</tooltip>。

> 继承 `User`。

<deflist>
<def title="name">

此成员的名称。通常是代表它作为一个用户的名称, 而不是在某个组织内的“昵称”。

</def>
<def title="nick">

此成员在组织内的昵称。如果未设置或无法获取则会得到 `null`。

</def>
</deflist>