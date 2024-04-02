---
switcher-label: Java API
---

# 使用核心库

<tldr>

使用 `Application` 的基本实现模块 `simbot-core`。

</tldr>

## 准备

准备一个 Kotlin 或 Java 项目。

- 如果是 Kotlin 多平台项目, 可参考 [概述](outline.md#multiplatform-targets) 了解支持的平台。
- 如果是 Java 项目, 请确保 Java 版本是 **Java11** 或以上。

## 安装

<tabs group="build">
<tab title="Gradle(Kotlin DSL)" group-key="kts">

```Kotlin
implementation("love.forte.simbot:simbot-core:%version%")
```

<tip>

如果使用 Java 而不配合使用 Gradle 的 `kotlin` 插件, 那么你需要指定依赖的后缀为 `-jvm`。

```Kotlin
implementation("love.forte.simbot:simbot-core-jvm:%version%")
```

</tip>
</tab>
<tab title="Gradle(Groovy)" group-key="groovy">

```Groovy
implementation 'love.forte.simbot:simbot-core:%version%'
```

<tip>

如果使用 Java 而不配合使用 Gradle 的 `kotlin` 插件, 那么你需要指定依赖的后缀为 `-jvm`。

```Groovy
implementation 'love.forte.simbot:simbot-core-jvm:%version%'
```

</tip>


</tab>
<tab title="Maven" group-key="maven">

```xml

<dependency>
    <groupId>love.forte.simbot</groupId>
    <artifactId>simbot-core-jvm</artifactId>
    <version>%version%</version>
</dependency>
```

</tab>
</tabs>

## 使用

### 创建 Application

<tabs group="Code">

<tab title="Kotlin" group-key="Kotlin">

使用 `Simple` 工厂, 或使用扩展函数 `launchSimpleApplication`。

```Kotlin
val app = launchApplication(Simple) {
    // 配置...
}

// 或

val app = launchSimpleApplication {
    // 配置...
}

app.join() // 挂起直到被关闭
```

</tab>
<tab title="Java" group-key="Java">

```Java
var applicationAsync = Applications.launchApplicationAsync(Simple.INSTANCE, appConfigurer -> {
        // 配置 Application...
       
    });
});

// 异步结果可转化为 CompletableFuture
var future = applicationAsync.asFuture();
// ...

// 阻塞当前线程直到被关闭
future.join();
```

{switcher-key="%ja%"}

```Java
var application = Applications.launchApplicationBlocking(Simple.INSTANCE, appConfigurer -> {
        // 配置 Application...

    });
});

// 注册事件处理器、注册bot等

// 阻塞当前线程直到被关闭
application.joinBlocking();
```

{switcher-key="%jb%"}

</tab>
</tabs>

有关 `Application` 的配置与使用可直接前往参考 [基本内容-Application](basic-application.md) 章节,
它里面用于示例的内容就是 `simbot-core` 的实现 `Simple`。

## 安装组件

前往 [**官方维护的组件库**](official-components.md) 选择你想要使用的组件，或者选择某个/些你想要使用的第三方组件，
根据它们的手册安装、配置到你的 `Application` 中。

## 运行

执行你的程序入口 `main` 方法，体验一下吧~

> 在 native 平台下，你可能需要使用 `runBlocking { ... }` 包裹上述需要挂起的内容
> (因为 native 尚不支持可挂起的 main 入口，参考 [KT-52753](https://youtrack.jetbrains.com/issue/KT-52753/Native-Support-suspending-entrypoints))，
> 此时建议为其分配调度器，比如：`runBlocking(Dispatchers.Default) { ... }`。
