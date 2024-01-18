---
switcher-label: Java API
---

# Application

`Application` 是对一组注册的组件和插件们的统一运行环境。
组件和插件们总要依附在一个 `Application` 上。

`simbot-core` 模块提供了一个简单的普通实现：`SimpleApplication`，
下文将会以这个简单的普通实现为例进行说明。

> Spring Boot starter 模块 `simbot-core-spring-boot-starter` 有另外一个类型的实现，
> 不过它大部分内容也是基于 `SimpleApplication`，且不需要也不应手动构造，所以不做介绍。

## 创建与启动

<tabs group="Code">
<tab title="Kotlin" group-key="Kotlin">

在 Kotlin 中，使用 `launchApplication(Factory) { config... }` 配置并启动一个 `Application`。

其中，`Factory` 是 `Application` 的工厂实例，在 `simbot-core` 中它是 `Simple`。

```Kotlin
val app = launchApplication(Simple) {
    config {
        // 一些 Application 的配置，例如协程上下文
        coroutineContext = Dispatchers.Default
    }

    // 一些事件调度器的配置，例如协程上下文、拦截器等
    eventDispatcher {
        coroutineContext = Dispatchers.Default
        addInterceptor { `this` ->
            // intercept..
            `this`.invoke() // invoke
        }
        addDispatchInterceptor { `this` ->
            // intercept..
            `this`.invoke() // invoke
        }
    }

    // 安装组件或插件
    install(...)

    // 安装组件或插件，以及额外的配置
    install(...) {
        // ...
    }
}
```

`Simple` 实现提供一个简写扩展函数：`launchSimpleApplication`：

```Kotlin
val app = launchSimpleApplication {
    // ...
}
```

</tab>
<tab title="Java" group-key="Java">
</tab>
</tabs>

构建完 `Application` 便可以开始注册事件处理器和 `Bot` 了。

## 事件处理器的注册

有关事件处理器的相关描述可前往 [事件监听与处理](basic-event-listener.md) 章节阅读。

## Bot的注册

有关Bot注册的相关描述可前往 [Bot管理器](BotManager.md) 章节阅读。
