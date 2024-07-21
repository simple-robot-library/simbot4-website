# Telegram 🚧

<a href="https://github.com/simple-robot/simbot-component-telegram/releases/latest">
<img alt="release" src="https://img.shields.io/github/v/release/simple-robot/simbot-component-telegram" />
</a>

<seealso>
<category ref="links">
<a href="https://github.com/simple-robot/simbot-component-telegram">Telegram组件仓库</a>
<a href="https://ktor.io/">Ktor首页</a>
<a href="https://core.telegram.org/bots/api">Telegram Bot API</a>
</category>
</seealso>

<tip>

已经发布了早期可用的尝鲜版本，欢迎体验与**反馈**~

</tip>

<warning title="协助希望🙏">

Telegram组件仍处于**早期开发阶段**，并且积极地期待着**贡献者**的参与与协助！

如果你对参与此组件的开发感兴趣，欢迎前往 
<a href="communities.md" /> 加入社群与我们联系，
或通过 [issues](https://github.com/simple-robot/simbot-component-telegram/issues)
和 [pull request](https://github.com/simple-robot/simbot-component-telegram/pulls)
协助我们！

</warning>

<warning>

文档内容尚在施工中。。。

</warning>

## 概述

**Telegram组件**
是一个 [Kotlin 多平台](https://kotlinlang.org/docs/multiplatform.html) 的 [**Telegram Bot API**][telegram bot doc] SDK实现库，
也是 Simple Robot 标准API下实现的组件库，异步高效、Java友好！

> 序列化和网络请求相关分别基于 [Kotlin serialization](https://github.com/Kotlin/kotlinx.serialization)
> 和 [Ktor](https://ktor.io/).

- 前往**Telegram组件**的 [ GitHub 仓库](https://github.com/simple-robot/simbot-component-telegram)

## 安装
### 安装组件库

<procedure title="安装依赖" id="安装依赖">
<step>
<control>安装simbot核心库实现</control>

<include from="refers.md" element-id="pre-component-install" />

</step>
<step>
<control>安装组件库</control>

`simbot-component-telegram-core`
即为Telegram组件的核心库，
也就是作为simbot组件所使用的
<tooltip term="组件">组件库</tooltip> 。

<tabs id="tg-build" group="build">
<tab title="Gradle(Kotlin DSL)" group-key="kts">

```Kotlin
implementation("love.forte.simbot.component:simbot-component-telegram-core:%tg-version%")
```

如果使用 Java 而不配合使用 Gradle 的 `kotlin` 插件, 那么你需要指定依赖的后缀为 `-jvm`。

```Kotlin
implementation("love.forte.simbot.component:simbot-component-telegram-core-jvm:%tg-version%")
```

</tab>
<tab title="Gradle(Groovy)" group-key="groovy">

```Groovy
implementation 'love.forte.simbot.component:simbot-component-telegram-core:%tg-version%'
```

如果使用 Java 而不配合使用 Gradle 的 `kotlin` 插件, 那么你需要指定依赖的后缀为 `-jvm`。

```Groovy
implementation 'love.forte.simbot.component:simbot-component-telegram-core-jvm:%tg-version%'
```

</tab>
<tab title="Maven" group-key="maven">

```xml
<dependency>
    <groupId>love.forte.simbot.component</groupId>
    <artifactId>simbot-component-telegram-core-jvm</artifactId>
    <version>%tg-version%</version>
</dependency>
```

</tab>
</tabs>

</step>
<step>
<control>安装Ktor客户端引擎</control>

Telegram组件使用 [Ktor](https://ktor.io) 作为 HTTP 客户端实现，
但是默认不会依赖任何具体的**引擎**。

因此，你需要选择并使用一个 Ktor Client 引擎实现。

<include from="refers.md" element-id="engine-choose"/>
</step>

</procedure>


[telegram bot doc]:https://core.telegram.org/bots/api
