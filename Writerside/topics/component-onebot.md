# OneBot

<a href="https://github.com/simple-robot/simbot-component-onebot/releases/latest">
<img alt="release" src="https://img.shields.io/github/v/release/simple-robot/simbot-component-onebot" />
</a>


- 前往 [**OneBot组件**GitHub 仓库](https://github.com/simple-robot/simbot-component-onebot)
- 前往 [**OneBot组件**手册](https://simple-robot.github.io/simbot-component-onebot/)

<warning title="正在迁移...">

独立的手册内容正在逐步迁移至当前文档，但尚未迁移完成。

</warning>

## 概述

OneBot 组件是一个
[OneBot11协议](https://github.com/botuniverse/onebot-11)
的客户端SDK，
也是
[Simple Robot](http://github.com/simple-robot/simpler-robot) 
标准API下实现的组件库。

借助simbot核心库提供的能力，它支持众多高级功能和封装，比如组件协同、Spring支持等，
助你快速开发 OneBot 客户端应用！

> 序列化和网络请求相关分别基于 [Kotlin serialization](https://github.com/Kotlin/kotlinx.serialization)
和 [Ktor](https://ktor.io/)。

## 前期准备

### OneBot11协议服务端

OneBot组件是一个OneBot协议的**客户端**实现，
因此在使用之前，你需要安装、下载、启动一个支持OneBot11协议的**服务端**。

<warning>

鉴于OneBot协议的主要应用场景的特殊性，
大多数服务端的实现库都**不建议、不允许**公开讨论、宣传有关它们的信息，
因此此处将 **不做列举**。

你可以选择去GitHub等开源网站搜索各种实现库，
或者可以前往 [社群](https://simbot.forte.love/communities.html)
向其他人寻求帮助。

</warning>

## 安装

<note>
在安装组件库之前，确保你已经安装了可用的核心库实现，比如 simbot-core 或 Spring Boot starter.
</note>

### 安装OneBot11组件

首先，安装 `simbot-component-onebot-v11-core` 依赖。

<tabs id="build">
<tab title="OneBot11">
<tabs group="build">
<tab title="Gradle(Kotlin DSL)" group-key="kts">

```Kotlin
implementation("love.forte.simbot.component:simbot-component-onebot-v11-core:%ob-version%")
```

如果使用 Java 而不配合使用 Gradle 的 `kotlin` 插件, 那么你需要指定依赖的后缀为 `-jvm`。

```Kotlin
implementation("love.forte.simbot.component:simbot-component-onebot-v11-core-jvm:%ob-version%")
```

</tab>
<tab title="Gradle(Groovy)" group-key="groovy">

```Groovy
implementation 'love.forte.simbot.component:simbot-component-onebot-v11-core:%ob-version%'
```

如果使用 Java 而不配合使用 Gradle 的 `kotlin` 插件, 那么你需要指定依赖的后缀为 `-jvm`。

```Groovy
implementation 'love.forte.simbot.component:simbot-component-onebot-v11-core-jvm:%ob-version%'
```

</tab>
<tab title="Maven" group-key="maven">

```xml
<dependency>
    <groupId>love.forte.simbot.component</groupId>
    <artifactId>simbot-component-onebot-v11-core-jvm</artifactId>
    <version>%ob-version%</version>
</dependency>
```

</tab>
</tabs>
</tab>
</tabs>

OneBot11组件使用 [Ktor](https://ktor.io) 作为 HTTP 客户端实现，
但是默认不会依赖任何具体的**引擎**。

因此，你需要选择并使用一个 Ktor Client 引擎实现。

<warning>

注意，你需要选择一个支持 **WebSocket** 的引擎。

</warning>

<include from="refers.md" element-id="engine-choose"/>

