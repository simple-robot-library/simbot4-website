# 开始使用

<tldr>
本章节介绍开始使用 simbot4 的基本步骤。
</tldr>

<procedure>
<step>安装核心库或Spring starter, 或者其他任意的 Application 实现。</step>
<step>选择并安装你所需的组件库, 配置它们</step>
<step>启动你的程序, 或者打包、部署。</step>
</procedure>

## 安装 Application 实现

首先, 添加某个基于 `simbot-api` 实现了 `Application` 完整功能的库, 然后根据它们的要求配置或使用。

### 官方实现

simbot4 提供了两个这样的库：`simbot-core` (也就是**“核心库”**), 以及 Spring Boot 的 starter 实现。

- 前往 [**使用核心库**](start-use-core.md) 来了解如何添加并使用 `simbot-core` 以及它提供的 `Application` 实现: `Simple`。
- 前往 [**使用 Spring Boot**](start-use-spring-boot-3.md) 了解如何添加并使用 `simbot-core-spring-boot-starter` 以及它提供的 `Application` 实现。

<note>

一般来讲, 尽管会需要添加组件库, 但是核心库（或其他 Application 实现库）的依赖依然是**必须的**。
因为组件库对核心库（或者标准库）的依赖通常**仅在编译期**。

</note>

## 安装组件、插件

`Application` 提供了 `simbot-api`（也就是**“标准库”**）所定义的诸多功能, 比如组件、插件的安装、事件的调度等等, 
但是它没有 **具体的平台实现** , 比如QQ频道、KOOK、大别野等 **特定平台** 的事件、消息等。

这些**特定平台**的实现, 便是一个个不同的**组件或插件**, 它们一般都是在各自独立的仓库中维护、发版, 
依赖于标准库, 但各自独立。
下文会将它们称为 **“组件库”** 。

以我们 [**官方维护的组件库**](official-components.md) 为例, 它们都会有各自的应用手册, 
手册中也会各自提供类似于“快速开始”的章节, 会提供如何进行配置, 如何搭建一个项目。

你接下来所要做的, 便是选择你希望使用的组件库(们), 将它们所需的依赖也添加下来, 然后进行配置就好了。
甚至, 如果你只需要使用一个单独的组件库, 也可以只参阅对应组件库的“快速开始”, 
里面会同时(简单地)介绍如何安装核心库和组件库实现。

举一个简单的添加依赖的**伪例子**:

<tabs group="build">
<tab title="Gradle (Kotlin DSL)" group-key="kts">

```Kotlin
// 使用核心库
implementation("love.forte.simbot:simbot-core:%version%")
// 添加组件库, 假设有两个叫做 foo1、bar2 的组件实现
implementation("com.example.component:foo1-core:1.14")
implementation("io.github.Jojo.cp:bar2-impl:5.14")
```

</tab>
<tab title="Gradle (Groovy)" group-key="groovy">

```groovy
// 使用核心库
implementation 'love.forte.simbot:simbot-core:%version%'
// 添加组件库, 假设有两个叫做 foo1、bar2 的组件实现
implementation 'com.example.component:foo1-core:1.14'
implementation 'io.github.Jojo.cp:bar2-impl:5.14'
```

</tab>
<tab title="Maven" group-key="maven">

```xml
<!-- 使用核心库 -->
<dependency>
    <group>love.forte.simbot</group>
    <artifactId>simbot-core</artifactId>
    <version>%version%</version>
</dependency>
<!-- 添加组件库, 假设有两个叫做 foo1、bar2 的组件实现 -->
<dependency>
    <group>com.example.component</group>
    <artifactId>foo1-core</artifactId>
    <version>1.14</version>
</dependency>
<dependency>
    <group>io.github.Jojo.cp</group>
    <artifactId>bar2-impl</artifactId>
    <version>5.14</version>
</dependency>
```

</tab>
</tabs>

### 官方组件

- 前往 [**官方维护的组件库**](official-components.md) 寻找你所需的组件, 并根据引导或手册添加依赖并使用。

### 社区组件

- 前往 [**社区组件**](community-components.md) 寻找你所需的组件, 或自行开发组件、插件, 然后使用。

### 组件开发

- 前往 [**组件开发**](component-dev.md) 了解开发一个组件库的方式与步骤。