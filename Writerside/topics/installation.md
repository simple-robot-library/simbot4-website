# 安装

simbot的核心功能由实现了 simbot-api 模块的内容提供。

## 使用核心库

核心库是最基础的对 simbot-api 的实现模块。

### 准备 {id='prepare-simbot-core'}

<snippet id="prepare-simbot-core-snippet">

准备一个 Kotlin 或 Java 项目。

- 如果是 Java 或 Kotlin/JVM 项目, 请确保 JDK 版本是 **JDK11** 或以上。
- 如果是 Kotlin 多平台项目, 可参考 [概述](outline.md#multiplatform-targets) 了解支持的平台。

</snippet>

### 安装核心库

<include from="refers.md" element-id="simbot-core-build"></include>

## 使用 Spring Boot

参考
<b><a href="Spring-Boot.md"></a></b>
了解更多。


## 安装组件库

<tooltip term="组件">组件库</tooltip>
是平台功能的主要提供者，是重要的核心概念之一。

前往
<a href="official-components.md"></a>
选择、添加一个你所需的组件库，并可以前往它们各自的手册了解更多信息。