# 概述

此处会针对“组件、插件、组件库” 等概念进行简单的介绍。

## 组件

一个代码中的 
<tooltip term="组件标识"><control>"组件"</control></tooltip>
，也可称为一个 **"组件标识"**，即代表一个 **接口 `Component`** 的实现类型。

_也可以将其理解为狭义的组件。_

比如：

```Kotlin
interface FooComponent : Component {
    // 省略内部实现...
}

class BarComponentImpl : Component {
    // 省略内部实现...
}
```

代码示例中的 `FooComponent` 和 `BarComponentImpl` 都可被称为 "组件标识" 的实现。

"组件标识"的作用是为一个或多个
<tooltip term="插件"><control>插件</control></tooltip>
提供一个"身份"、向 `Application` 中**提供序列化信息**、以及可以**在多个插件间共享**的一些配置。

我们来看看 `Component` 接口的定义：

```Kotlin
public interface Component {
    /**
     * 一个组件的ID。
     * 组件id建议使用类似于Java包路径的格式，
     * 例如 `org.example.Sample` 并尽量避免重复。
     */
    public val id: String

    /**
     * 组件对外提供的统合所有所需的序列化信息。
     * 通常为 message 类型的序列化或文件配置类的序列化信息。
     */
    public val serializersModule: SerializersModule
}
```

`Component` 中主要定义了一个用作标识的字符串ID `id: String`, 和提供**序列化信息的** `serializersModule: SerializersModule`。
其中，向 `Application` 中提供序列化信息是 `Component` 最基本的主要职责。

组件标识在某些情况下并不是**必要的**, 对于一些简单的插件实现来讲，组件标识的实现是可以省略的。
比如一个插件，即不需要一个可共享的配置实现，也没有需要提供的序列化信息，也没有任何地方需要提供 `Component` 属性，
那么它就可以没有对应的 _组件标识_ 实现。

## 插件

<tooltip term="插件"><control>插件</control></tooltip>
是可以安装在 `Application` 中的**功能单位**。
一个**插件的实现**就是指实现了接口 `Plugin` 的某个类型。

例如：

```Kotlin
interface MyPlugin : Plugin {
    // 省略内部实现...
}

class FooPluginImpl : Plugin {
    // 省略内部实现...
}
```

代码示例中的 `MyPlugin` 和 `FooPluginImpl` 都可被称为 "插件" 的实现。

何为**功能单位**？就是在构建 `Application` 的过程中，可以额外添加/修改一些信息、或执行一些逻辑。

举个例子，你可以在 `Application` 启动过程中注册一个事件处理器，或者保存事件调度器 `EventDispatcher` 并在后续（比如在异步中）推送一些事件。

我们来看看 `Plugin` 接口的定义：

```Kotlin
public interface Plugin
```

非常简单对吧？`Plugin` 接口内没有任何约束，也就是说 `Plugin` 的实现可以非常自由。 

一般来讲，一个插件可能"属于"某个 _组件标识_ ，但是也正如上文所说，_组件标识_ 并不是必要的。
实际上，这种"所属关系"更多的是一种**约定**，而并非**限制**。

这也是为什么 `Plugin` 接口的定义中实际上并没有要求有一个 `Component` 类型的属性。

一般情况下，为了**严谨**，可以在构建 `Plugin` 过程中去校验某个 `Component` 是否存在，
并获取到它、保存下来。但是如果你的 `Plugin` 实现的确就是用不到 `Component`，那么就不需要理会它。

<note>

拓展模块 **持续会话 (`simbot-extension-continuous-session`)** 就是一个没有 `Component` 实现的插件库。

</note>

## 组件库

<tooltip term="组件"><control>组件库</control></tooltip>，
在部分文档内容中也会被称为 **组件** (一般会带个名字，比如"xxx组件")，它是一个比较宏观的说法，
通常指代一整个仓库或一整个依赖的统称。

一个仓库，其中包含了一个或多个组件和插件的实现，就可以将其统称为组件库。
而后为组件库起个名字，便可称之为xxx组件或xxx组件库。

_也可以将其理解为广义的组件。_

因此 _"组件库"_ 是一个抽象的统称，并不是某个具体的代码、某个具体的接口实现。

## Bot管理器

Bot管理器即指接口类型 `BotManager`，它继承了 `Plugin`，是 **插件** 的一个特殊子类型，
用来定义针对 `Bot` 的管理行为。

`BotManager` 中定义了对 `Bot` 的创建、寻找等功能，一般实现 `BotManager` 就会需要一个配套的 `Bot` 实现，
比如 `FooBot` 和 `FooBotManager`。

与单纯的 `Plugin` 不同，`Bot` 是明确要求一个具体的 `Component` 的，
而 `Bot` 又是通过 `BotManager` 创建的，因此 `BotManager` 通常需要一个对应的 `Component` 实现，
并需要在构建的时候从 `Application` 中寻找、校验。

## 事件

"事件"即代表一个实现了接口 `Event` (或其子类型) 的类型。
例如：

```Kotlin
interface MyEvent : Event {
    // ...
}

class FooEvent : BotEvent {
    // ...
}
```

代码示例中的 `MyEvent` 和 `FooEvent` 都是一种事件类型。
其中，simbot标准库的 `simbot-api` 模块 (也就是 `Event` 接口所在的模块) 定义了一些基础的事件类型，
可以借助 IDE 的提示、[API文档](https://docs.simbot.forte.love) 
或前往文档章节
<a href="basic-event.md" />
查阅。