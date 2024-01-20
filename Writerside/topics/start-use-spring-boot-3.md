# 使用 Spring Boot 3

<tldr>

在 JVM 平台下使用 Spring Boot 3 配合 simbot4 进行
<control>快速开发</control>。

</tldr>

## 准备

### Java 17

simbot4 的 Spring Boot starter 基于 Spring Boot 3，因此请准备至少 **Java17** 版本的 Java。

### 创建 Spring Boot 3 项目

首先你得有个 Spring Boot 3 项目, 你可以前往 [Spring Initializr](https://start.spring.io)
或者借助 IDE (比如 IDEA) 的相关功能创建一个 Spring Boot 3 的项目。
你可以自由选择需要添加的任何其他 Spring Boot 组件，比如 `spring-boot-starter-web` 之类的。

<note title="相关参考">

- [Spring Initializr](https://start.spring.io)
- [Spring Quickstart Guide](https://spring.io/quickstart/)

</note>

## 安装 starter

在你的 Spring Boot 项目基础上，添加 `simbot-core-spring-boot-starter` 相关依赖。

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
    <group>love.forte.simbot</group>
    <artifactId>simbot-core-spring-boot-starter</artifactId>
    <version>%version%</version>
</dependency>
```

</tab>
</tabs>

## 修改启动类


## 编写事件处理器


## 可配置项


## 安装组件以及组件配置


## 运行

TODO