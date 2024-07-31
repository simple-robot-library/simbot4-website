# 持续会话 Continuous Session

<warning>

施工中...

</warning>


## 概述

在进行业务编写时，时常会遇到需要继续连续对话的场景，例如：

```
用户: 绑定账户
BOT: 好的，请输入您的账户
用户: 123456789
BOT: 绑定成功，您的账号为: 123456789
```

如上案例，用户首先发出第一个指令 `绑定账户`，使bot进入等待接受“账户”的状态，
随即接受后续绑定后完成状态。

想要实现这个功能的方式很多，例如最基础的可以通过状态机来完成。
但是当这个流程开始逐步变得复杂，在大量的状态面前手动实现一个状态机就会显得十分繁琐。

这时候，协程就会体现出它的作用。

而**持续会话模块 (`simbot-extension-continuous-session`)**便基于异步与协程对**持续会话**的封装与实现。

<warning title="实验性">

**持续会话**是一个独立的扩展模块，且处于**实验阶段**，
可能会在未来发生任何不兼容变更或被移除。

</warning>


## 安装

**持续会话**是一个独立的扩展模块，你需要额外添加它的依赖并配置它。

<procedure>
<step><control>添加依赖</control>

<tabs id="build" group="build">
<tab title="Gradle(Kotlin DSL)" group-key="kts">

```Kotlin
implementation("love.forte.simbot.extension:simbot-extension-continuous-session:%version%")
```

如果使用 Java 而不配合使用 Gradle 的 `kotlin` 插件, 那么你需要指定依赖的后缀为 `-jvm`。

```Kotlin
implementation("love.forte.simbot.extension:simbot-extension-continuous-session-jvm:%version%")
```

</tab>
<tab title="Gradle(Groovy)" group-key="groovy">

```Groovy
implementation 'love.forte.simbot.extension:simbot-extension-continuous-session:%version%'
```

如果使用 Java 而不配合使用 Gradle 的 `kotlin` 插件, 那么你需要指定依赖的后缀为 `-jvm`。

```Groovy
implementation 'love.forte.simbot.extension:simbot-extension-continuous-session-jvm:%version%'
```

</tab>
<tab title="Maven" group-key="maven">

```xml
<dependency>
    <groupId>love.forte.simbot.extension</groupId>
    <artifactId>simbot-extension-continuous-session-jvm</artifactId>
    <version>%version%</version>
</dependency>
```

</tab>
</tabs>
</step>
<step><control>配置</control>

TODO

<tabs>
<tab title="核心库">

</tab>
<tab title="SpringBoot">

</tab>
</tabs>

</step>
</procedure>



