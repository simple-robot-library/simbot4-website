# 组件开发

本章节下内容会为你介绍如何实现一套自定义的
<tooltip term="组件">组件</tooltip>
, 包括
<tooltip term="组件标识">组件标识</tooltip>、
<tooltip term="插件">插件</tooltip>、
Bot、Bot管理器、事件等。

## 推荐组合

我们强烈推荐您使用 `Kotlin` + `Gradle` 的组合开发组件，
并在有条件的情况下支持 KMP 多平台。

尤其是当你使用 Kotlin 语言进行开发时, 只有 `Gradle` 支持多平台和子章节中会介绍到的 [编译器插件](component-dev-compiler-plugin.md)。

## 注意事项

### Java 版本

simbot4 的 JVM 目标最低为 **Java11**。不论是使用 Kotlin 还是 Java 开发, 
都应当满足此要求。

### Java 模块化 {id="JPMS"}

既然最低 Java 目标的要求是 **Java11**, 我们也强烈建议您在 JVM 平台上支持 Java 的模块化信息([JPMS](https://www.oracle.com/cn/corporate/features/understanding-java-9-modules.html))。

> 说的简单点儿, 就是提供一个合适的 `module-info.java`。

### 开发语言?

simbot4 是基于 Kotlin 多平台实现、提供 Java 友好 API 的事件调度框架。
对于普通开发者来讲, 我们提供了阻塞、异步等多种风格的 Java API 供开发者选择。

但是对于 **组件开发**, 我们强烈建议使用 [Kotlin](https://kotlinlang.org/) 语言。
能使用 Java 开发的组件部分比较有限, 尤其是涉及到 `Bot` 等需要网络请求等内容的实现。

如果你了解 Java 等语言, 那么 Kotlin 的上手也会很快。
如果不介意的话, 我建议尽可能地选择使用 Kotlin 进行 **组件开发**。

当然, 如果你的实现比较简单, 比如仅仅是注册一个事件监听器等, 那么 Java 也可以应付, 
但是不论如何其开发体验都远不及使用 Kotlin。

> 此章节下的内容如果遇到了无法使用 Java 进行实现或实现起来十分困难的地方会进行标注说明。