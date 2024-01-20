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

假设你的启动类的全限定名称为 `example.demo.MyApplication`, 它是这个样子的：

<tabs group="Code">
<tab title="Kotlin">

```Kotlin
@SpringBootApplication
class MyApplication

fun main(args: Array<String>) {
    runApplicarion<MyApplication>(*args)
}
```

</tab>
<tab title="Java">

```Java
@SpringBootApplication
public class MyApplication {
    public static void main(String[] args) {
        SpringApplication.run(MyApplication.class, args);
    }
}
```

</tab>
</tabs>

此时，添加一个注解 `@EnableSimbot` 来启用 simbot 的 Spring Boot starter。

<tabs group="Code">
<tab title="Kotlin">

```Kotlin
@EnableSimbot // 启用 simbot
@SpringBootApplication
class MyApplication

fun main(args: Array<String>) {
    runApplicarion<MyApplication>(*args)
}
```

</tab>
<tab title="Java">

```Java
@EnableSimbot // 启用 simbot
@SpringBootApplication
public class MyApplication {
    public static void main(String[] args) {
        SpringApplication.run(MyApplication.class, args);
    }
}
```

</tab>
</tabs>

<warning>

在 Spring Boot 中，你应当确保你的启动类有**一层以上**的包结构。

</warning>

## 编写事件处理器

在 Spring Boot **可扫描到** 的范围内，创建任意类型，并将其标记为 Spring 的 component：

<tabs group="Code">
<tab title="Kotlin">

```Kotlin
@Component // 交给 Spring 管理
class MyEventHandlers {
    
    @Listener // 标记事件处理函数
    suspend fun handle(event: Event) {
        // 处理事件...
    }
    
    // 更多...
}
```

</tab>
<tab title="Java">

```Java
@Component // 交给 Spring 管理
public class MyEventHandlers {
    
    @Listener // 标记事件处理函数
    public void handle(Event event) {
        // 处理事件...
    }
    
    // 更多...
}
```

</tab>
</tabs>

## 可配置项

starter 提供了一些可用的配置项。

> 对应的配置类型是 `love.forte.simbot.spring.common.application.SpringApplicationConfigurationProperties`。
> 
> 配置属性的键均以 `simbot` 开头。
> 
> 下面的实例中的配置项的值均为默认值。也就是它们都是可以省略的。

<tabs group="Spring Config">
<tab title="properties" group-key="properties">

```
# simbot.application: 与 Application 相关的配置
simbot.application.application-launch-mode=NONE

# simbot.bots: 与 bot 配置相关的属性。
simbot.bots.auto-registration-failure-policy=ERROR
simbot.bots.auto-registration-mismatch-configurable-bot-manager-policy=ERROR_LOG
simbot.bots.auto-registration-resource-load-failure-policy=ERROR
simbot.bots.auto-start-bots=true
simbot.bots.auto-start-mode=ASYNC
simbot.bots.configuration-json-resources=classpath:simbot-bots/*.bot.json

# simbot.components: 组件相关的配置信息。
simbot.components.TODO

# simbot.plugins: 插件相关的配置信息。
simbot.plugins.TODO

```


</tab>
<tab title="yaml" group-key="yaml">

```yaml
simbot:
  # simbot.application: 与 Application 相关的配置
  application:
    application-launch-mode: none
  # simbot.bots: 与 bot 配置相关的属性。
  bots:
    auto-registration-failure-policy: ERROR
    auto-registration-mismatch-configurable-bot-manager-policy: ERROR_LOG
    auto-registration-resource-load-failure-policy: ERROR
    auto-start-bots: true
    auto-start-mode: ASYNC
    configuration-json-resources:
      - 'classpath:simbot-bots/*.bot.json'
  # simbot.components: 组件相关的配置信息。
  components:
    TODO

  # simbot.plugins: 插件相关的配置信息。
  plugins:
    TODO

```

</tab>
</tabs>

[//]: # (https://docs.simbot.forte.love/snapshots/main-v4/simbot-cores/simbot-core-spring-boot-starter-common/love.forte.simbot.spring.common.application/-spring-application-configuration-properties/-bot-properties/index.html)

## 安装组件以及组件配置


## 运行

TODO