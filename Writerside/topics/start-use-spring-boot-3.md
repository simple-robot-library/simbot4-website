# 使用 Spring Boot

<tldr>

在 JVM 平台下使用 Spring Boot 配合 simbot4 进行
<control>快速开发</control> 。

</tldr>

<note title="更多信息">
有关集成Spring Boot的详细内容可前往
<b><a href="Spring-Boot.md"></a></b> 
进行参考。
</note>

## 准备

### Java 17

simbot4 的 Spring Boot starter 基于 Spring Boot 3，因此 Java 的版本至少为 **Java17** 。

### 创建 Spring Boot 3 项目

首先你得有个 Spring Boot 3 项目, 你可以前往 [Spring Initializr][spring.start]
或者借助 IDE (比如 IDEA) 的相关功能创建一个 Spring Boot 3 的项目。
你可以自由选择需要添加的任何其他 Spring Boot 组件，比如 `spring-boot-starter-web` 之类的。

<note title="相关参考">

- [Spring Initializr][spring.start]
- [Spring Quickstart Guide](https://spring.io/quickstart/)

</note>

## 安装
### 添加 simbot Spring Boot starter

<include from="refers.md" element-id="simbot-spring-3-build"></include>

### 添加组件及其要求的其他内容

simbot的组件是功能的主要提供者，是**重要**的核心概念之一。
你可以前往
<a href="official-components.md"></a>
选择你所需要的组件并添加它们，而它们的手册通常都会有各自相应的**快速开始**或**安装**章节。

<note>
大部分官方组件都会需要你添加 Ktor 引擎，注意观察阅读、不要漏掉喔~
</note>

你也可以使用任何可用的第三方组件，并参照它们各自的文档说明进行安装。

## 使用
### 启用 simbot

前往
<a href="Spring-Boot.md#启用-simbot"></a>
了解更多。

### 编写事件处理器

前往 
<a href="Spring-Boot.md#编写事件处理器"></a>
了解更多。

## 安装组件以及组件配置

前往
<a href="Spring-Boot.md#安装组件以及组件配置"></a>
了解更多。

## 注册Bot

前往
<a href="Spring-Boot.md#注册bot"></a>
了解更多。

## 运行或打包

前往
<a href="Spring-Boot.md#运行或打包"></a>
了解更多。

[spring.start]: https://start.spring.io
