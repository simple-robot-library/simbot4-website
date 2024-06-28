# Spring Boot

<warning>

建设中。
现在有关 Spring Boot 的内容可先前往 
<a href="start-use-spring-boot-3.md"></a>
查看。
</warning>

<note>

有关 Spring Boot 的更多信息，前往 [spring.io](https://spring.io) 了解更多。

</note>

## 什么是 Spring Boot starter?

参考 [官方文档](https://docs.spring.io/spring-boot/reference/using/build-systems.html#using.build-systems.starters)
的说明：

> Starters are a set of convenient dependency descriptors that you can include in your application. 

启动器是一组方便的依赖描述符，你可以将它包含在你的应用程序中。

而 simbot Spring Boot starter 便是这样一组描述符：允许你将 simbot 的核心功能更快捷、简单地集成进你的 Spring 应用程序中。
除了核心功能的实现，simbot starter 还实现了 
<a href="advanced-quantcat.md"></a> ，
你可以通过各类注解API更快速高效的编写你的事件处理逻辑 —— 与 Spring 应用一起。

## 安装

<procedure>
<step><control>准备 Spring Boot 项目</control>

前往 [start.spring.io](https://start.spring.io) 选择、创建一个 Spring Boot 项目下载。
当然，你也可以选择其他任意可行方式来创建项目，例如借助IDE。

更多信息可参考 [如何构建你的第一个SpringBoot应用](https://docs.spring.io/spring-boot/tutorial/first-application/index.html)。

</step>
<step><control>添加 simbot starter 依赖</control>

<tabs>
<tab title="Spring Boot 3">

使用 Spring Boot 3，确保项目的JDK版本大于等于 JDK17。

<tabs group="build">
<tab title="Gradle(Kotlin DSL)" group-key="kts">

```Kotlin
implementation("love.forte.simbot:simbot-core-spring-boot-starter:%version%")
```

</tab>
<tab title="Gradle(Groovy)" group-key="groovy">

```Groovy
implementation 'love.forte.simbot:simbot-core-spring-boot-starter:%version%'
```

</tab>
<tab title="Maven" group-key="maven">

```xml
<dependency>
    <groupId>love.forte.simbot</groupId>
    <artifactId>simbot-core-spring-boot-starter</artifactId>
    <version>%version%</version>
</dependency>
```

</tab>
</tabs>

</tab>
<tab title="Spring Boot 2">
<warning title="再考虑一下">

我们建议你优先考虑使用 Spring Boot 3。

<list>
<li>

首先，Spring Boot 2.x 目前最后的一个版本 v2.7.x 已经在 2023-11-24 结束了
_开放源码软件支持(OSS support)_ ，并会在 2026 年之前彻底结束支持。

(参考[Spring官方文档](https://spring.io/projects/spring-boot#support))

</li>
<li>

如果阻碍你使用 Spring Boot 3 的仅仅是Java的版本，
你仍有其他办法通过较低版本的Java使用 Spring Boot 3，
这其中的详细内容可参考 Jetbrains 的这篇
[技术博客](https://blog.jetbrains.com/zh-hans/idea/2024/06/java-runtimes-insights-from-the-spring-boot-point-of-view/)。

引用其中的相关说明：

> 此外，Spring Boot 3.x 要求使用 Java 17 作为基线，
> 因此如果您想利用您最喜欢的框架的新主要版本的强大功能，升级 Java 版本至关重要。
>
> 不过，如果现在不能升级，一些供应商（如 Oracle 和 BellSoft）
> 也提供了将 JVM 17 与 JDK 8 结合的解决方案（对于 JDK 11，BellSoft 也有类似的解决方案）。
> 这样会使您的应用程序认为它仍然运行在旧的 Java 版本上，但实际上它拥有一个较新的引擎。
> 对您来说，这意味着什么？ 无需升级 Java 或框架，也几乎无需进行代码更改，即可立即缩短延迟并提高吞吐量！

</li>
</list>

`simbot-spring-boot-starter-v2`
是用来兼容 Spring Boot v2 的，Java版本最低仅需Java11。
但是此模块**不保证**未来会持续维护、更新，也**不保证**其稳定性。

</warning>

<tabs group="build">
<tab title="Gradle(Kotlin DSL)" group-key="kts">

```Kotlin
implementation("love.forte.simbot:simbot-core-spring-boot-starter-v2:%version%")
```

</tab>
<tab title="Gradle(Groovy)" group-key="groovy">

```Groovy
implementation 'love.forte.simbot:simbot-core-spring-boot-starter-v2:%version%'
```

</tab>
<tab title="Maven" group-key="maven">

```xml
<dependency>
    <groupId>love.forte.simbot</groupId>
    <artifactId>simbot-core-spring-boot-starter-v2</artifactId>
    <version>%version%</version>
</dependency>
```

</tab>
</tabs>

</tab>
</tabs>

</step>
<step><control>添加组件及其要求的其他内容</control>

simbot的组件是功能的主要提供者，是**重要**的核心概念之一。
你可以前往 
<a href="official-components.md"></a>
选择你所需要的组件并添加它们，而它们的手册通常都会有各自相应的**快速开始**或**安装**章节。

<note>
大部分官方组件都会需要你添加 Ktor 引擎，注意观察阅读、不要漏掉喔~
</note>

你也可以使用任何可用的第三方组件，并参照它们各自的文档说明进行安装。

</step>
</procedure>