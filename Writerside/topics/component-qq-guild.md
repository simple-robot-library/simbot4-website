# QQ机器人

<a href="https://github.com/simple-robot/simbot-component-qq-guild/releases/latest">
<img alt="release" src="https://img.shields.io/github/v/release/simple-robot/simbot-component-qq-guild" />
</a>

<seealso>
<category ref="links">
<a href="https://github.com/simple-robot/simbot-component-qq-guild">QQ机器人组件仓库</a>
<a href="https://ktor.io/">Ktor首页</a>
<a href="https://bot.q.qq.com/wiki/develop/api/">QQ机器人开放平台</a>
</category>
</seealso>

## 概述

**QQ机器人组件**
是一个 [Kotlin 多平台](https://kotlinlang.org/docs/multiplatform.html) 的 [**QQ机器人官方API**][qg bot doc] SDK实现库，
也是 Simple Robot 标准API下实现的组件库，异步高效、Java友好！

现在支持的事件范围有：

- 频道所有**公域**事件
- 论坛相关的私域事件
- QQ群聊相关  (从4.0.0-beta6开始)
- C2C单聊相关 (从4.0.0-beta6开始)

私域相关的事件由于时间有限、且这方面的反馈与需求几乎没有，
因此优先级不高。
如果您有需要或希望协助，
可前往 [反馈](https://github.com/simple-robot/simbot-component-qq-guild/issues) 或 [协助](https://github.com/simple-robot/simbot-component-qq-guild/pulls) 了解更多~

> 序列化和网络请求相关分别基于 [Kotlin serialization](https://github.com/Kotlin/kotlinx.serialization)
> 和 [Ktor](https://ktor.io/).

- 前往**QQ机器人组件**的 [ GitHub 仓库](https://github.com/simple-robot/simbot-component-qq-guild)

## 命名说明

QQ机器人组件命名为 `simbot-component-qq-guild`，
因为最早开始QQ并未开放普通个人开发者使用QQ群聊、QQ单聊的功能，
因此此组件当时仅支持QQ频道。在开放后，其两端可以合并在一起使用，因此QQ群相关的能力才被支持。

> QQ群、QQ单聊功能自 `4.0.0-beta6` 版本起开始支持。

## 前提准备

你需要参考 [官方QQ机器人文档](https://bot.q.qq.com/wiki) ，并注册一个 
**机器人账号** 。审核通过，
便可登录 [QQ开放平台](https://q.qq.com/#/) 查看你的机器人账号信息了。

## 安装

### 安装组件库

<procedure id="install-core" title="安装依赖">
<step>
<control>安装simbot核心库实现</control>

<include from="refers.md" element-id="pre-component-install" />
</step>
<step>
<control>安装组件库</control>

`simbot-component-qq-guild-core` 
即为QQ机器人组件的核心库，
也就是作为simbot组件所使用的
<tooltip term="组件">组件库</tooltip>
。

<tabs id="qg-build" group="build">
<tab title="Gradle(Kotlin DSL)" group-key="kts">

```Kotlin
implementation("love.forte.simbot.component:simbot-component-qq-guild-core:%qg-version%")
```

如果使用 Java 而不配合使用 Gradle 的 `kotlin` 插件, 那么你需要指定依赖的后缀为 `-jvm`。

```Kotlin
implementation("love.forte.simbot.component:simbot-component-qq-guild-core-jvm:%qg-version%")
```

</tab>
<tab title="Gradle(Groovy)" group-key="groovy">

```Groovy
implementation 'love.forte.simbot.component:simbot-component-qq-guild-core:%qg-version%'
```

如果使用 Java 而不配合使用 Gradle 的 `kotlin` 插件, 那么你需要指定依赖的后缀为 `-jvm`。

```Groovy
implementation 'love.forte.simbot.component:simbot-component-qq-guild-core-jvm:%qg-version%'
```

</tab>
<tab title="Maven" group-key="maven">

```xml
<dependency>
    <groupId>love.forte.simbot.component</groupId>
    <artifactId>simbot-component-qq-guild-core-jvm</artifactId>
    <version>%qg-version%</version>
</dependency>
```

</tab>
</tabs>
</step>
<step>
<control>安装Ktor客户端引擎</control>

QQ机器人组件使用 [Ktor](https://ktor.io) 作为 HTTP 客户端实现，
但是默认不会依赖任何具体的**引擎**。

因此，你需要选择并使用一个 Ktor Client 引擎实现。

<warning>

注意，你需要选择一个支持 **WebSocket** 的引擎。

</warning>

<include from="refers.md" element-id="engine-choose"/>
</step>
</procedure>


[qg bot doc]: https://bot.q.qq.com/wiki/develop/api/
