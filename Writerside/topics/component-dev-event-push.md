# 实现事件推送

当你有了事件，那么就将其发射。

<note>
<p>
事件处理器<b>建议</b>通过Kotlin使用，不过我们也提供了一定程度的Java兼容。
</p>
<p>
如果你打算使用Java调用事件处理器的API，请尽可能选择<b>异步</b>或<b>响应式</b>的API。
</p>
</note>

## 事件处理器

在simbot标准API中定义了一个用于处理事件的事件处理器 `EventProcessor`，
它的作用是由 `Application` 提供给外部（例如组件），使其拥有推送事件的能力。

<note>
你可以在 
<a href="basic-event-listener.md" />
中了解到作为普通开发者如何使用事件处理器(也就是文中的 <code>EventDispatcher</code>)。
</note>

我们首先来简单看一下 `EventProcessor` 的简化版定义：

```Kotlin
public interface EventProcessor {
    /**
     * 处理事件，得到结果流
     */
    public fun push(event: Event): Flow<EventResult>
}
```

<tip>
其实也没简化多少，就简化了一下注释内容。如果你想要了解文档注释的完整信息，可以去参考下API文档或者源码的注释。
</tip>

可以看到，`EventProcessor` 的主要作用就是：通过 `push` 推送一个事件 `Event`，
然后得到所有事件监听函数(`EventListener`)的处理**结果流** `Flow<EventResult>`。

## 获取并保存

在介绍用法之前，先讲一下如何获取 `EventProcessor`。

<tip>

`EventProcessor` 通常以 `EventDispatcher` 的形式出现，后者实现了前者。

</tip>

### Plugin / BotManager 内获取

`EventProcessor` 在构建阶段中，从开始构建 `Plugin` 这个阶段开始可以被获取到。
其中，`BotManager` 的构建就属于这个阶段。

当你的 `Plugin` 或 `BotManager` 实现需要使用到事件推送的能力时，
你可以在注册它们的时候从 `PluginConfigureContext` 获取到 `EventDispatcher`。

以一个 `BotManager` 为例：

```Kotlin
class FooBotManager(
    val eventProcessor: EventProcessor,
    val config: FooBotManagerConfiguration
) : BotManager {
    // ... 其他实现细节省略 ... 

    /** 伴生对象实现工厂 */
    companion object Factory : BotManagerFactory<FooBotManager, FooBotManagerConfiguration> {
        override val key: BotManagerFactory.Key = object : BotManagerFactory.Key {}
        override fun create(context: PluginConfigureContext, configurer: ConfigurerFunction<FooBotManagerConfiguration>): FooBotManager {
            val config = FooBotManagerConfiguration().invokeBy(configurer)

            // 得到 EventDispatcher, 接下来就可以保留它，然后到需要的时候使用它了。
            val eventDispatcher = context.eventDispatcher

            // ...
            return FooBotManager(eventDispatcher, config)
        }
    }
}
```

### 外部获取

当 `Application` 被构建完成后，你可以在直接在 `Application` 中获取到 `EventDispatcher`。

```Kotlin
// 配置、构建、启动 Application
val app = launchSimpleApplication { ... }

// 得到 EventDispatcher
val eventDispatcher = app.eventDispatcher

// 使用它。
eventDispatcher.push(...)
```

## 使用

### 事件推送

一个最简单的使用例子：

<tabs group="Code">
<tab title="Kotlin" group-key="Kotlin">

```Kotlin
// 推送事件，并收集结果。
eventProcessor.push(event).collect()
```

<tip>

事件推送的结果是一个**流**，并且在默认情况下它是一个**冷流**。
你必须要收集它，它才会真正地将事件推入处理流程中。

如果：

```Kotlin
eventProcessor.push(event)
```

而不执行 `collect` (或其他衍生的收集API)，则很可能无法产生任何效果。

</tip>

</tab>
<tab title="Java" group-key="Java">

在 Java 中，我们为 `EventProcessor` 提供了一些异步有好的兼容API。

首先，`push` 函数本身并非挂起函数，因此你可以直接调用它并得到 `Flow` 的结果。

但是Java中想要直接处理 `Flow` 就有些困难了。
举个例子，你可以借助 `kotlinx-coroutines-reactor` 将 `Flow` 转为 `Flux`。

```Java
var flow = eventProcessor.push(event);
var flux = FluxKt.flux(
    EmptyCoroutineContext.INSTANCE,
    flow
);

// 异步地收集结果并忽略结果
flux.subscribe();
```

或者直接丢给一个作用域 `CoroutineScope` 在异步中执行：

```Java
eventProcessor.pushAndLaunch(
    GlobalScope.INSTANCE,
    event
);
```

除了上面直接借助 `EventProcessor` 自身的API以外，
我们还使用 `EventProcessors` 提供若干静态API来辅助使用。

```Java
// 推送事件并转化为 Flux
// 需要确保项目环境中存在 [[[kotlinx-coroutines-reactor|https://github.com/Kotlin/kotlinx.coroutines/tree/master/reactive]]] 的相关依赖
EventProcessors.pushAndAsFlux(
    eventProcessor, 
    event
).subscribe();

// 推送事件并将结果异步地收集为 List
var resultList = EventProcessors.pushAndCollectToListAsync(
    eventProcessor,
    event,
    GlobalScope.INSTANCE
);
```

<tip>
上文使用 <code>GlobalScope</code> 的地方仅做示例，实际使用以具体情况为准。
</tip>

</tab>
</tabs>

### 调度器

当执行 `EventProcessor.push` 时，其内部执行事件处理逻辑时的调度器是在构建 `Application` 的时候被决定的

```Kotlin
val app = launchApplication(...) {
        // 配置 eventDispatcher
        eventDispatcher {
            coroutineContext = Dispatchers.IO
        }
        
        // 其他配置...
    }
```

因此，通过 `push` 得到的结果无法影响上游的调度器。例如，以上述代码为基准，再追加以下代码：

```Kotlin
app.eventDispatcher
        // 推送事件
        .push(event)
        // 指定一个调度器
        .flowOn(MyCustomDispatcher)
        // 收集结果
        .collect()
```

那么，用于处理事件的监听函数会在 `Dispatcher.IO` 中被异步调度，
而在最后的 `collect` 时会被 `MyCustomDispatcher` 调度。

### 冷流与同步

由上可知，虽然默认的事件调度器的实现是**冷流**，即你不去收集则不会真正地执行事件处理，
但是因为可以配置调度器，因此它并非是一条一条地同步处理的。

考虑如下这组代码：

<compare style="top-bottom" first-title="默认" second-title="IO调度器">
<code-block lang="kotlin">
val app = launchApplication(...) {
        eventDispatcher {
            // 默认配置，即无自定义调度器
        }
    }
app.eventDispatcher
    .push(event)
    .take(3)
    .collect()
</code-block>
<code-block lang="kotlin">
val app = launchApplication(...) {
        eventDispatcher {
            coroutineContext = Dispatchers.IO
        }
    }
app.eventDispatcher
    .push(event)
    .take(3)
    .collect()
</code-block>
</compare>

假设其中一共注册了5个事件的监听函数，每个函数都会输出一段控制台文本，那么二者的输出则有可能是：

<compare style="top-bottom" first-title="默认" second-title="IO调度器">
<code-block lang="kotlin">
第1个处理器：处理事件
第2个处理器：处理事件
第3个处理器：处理事件
</code-block>
<code-block lang="kotlin">
第1个处理器：处理事件
第2个处理器：处理事件
第3个处理器：处理事件
第4个处理器：处理事件
第5个处理器：处理事件
</code-block>
</compare>

因此，虽然 `push` 后使用了 `take(3)` 限制了条数，
但是它无法保证能够控制真实的事件处理次数。

<tip>

响应式API的保留节目。
你可以前往 [官方文档](https://kotlinlang.org/docs/flow.html) 了解有关 `Flow` 的更多信息。

</tip>

### 结果筛选

作为一个插件，通常情况下我们需要对返回的 `EventResult` 结果流做一些处理。
一个最常见的场景：我们要将返回了**错误的结果**以日志的形式输出。

`EventResult` 包括了所有执行了的事件监听函数的执行结果，包括错误的结果。
在标准API中，我们约定如果类型为 `StandardEventResult.Error` 则将其视为一个执行失败的结果
（内部的异常处理也会将出现的错误包装为此），因此你可以在出现此类型时做一些处理：

```Kotlin
eventProcessor
    .push(event)
    // 如果出现异常结果，处理它
    .onEach { result ->
        if (result is StandardEventResult.Error) {
            // 比如：输出日志
            logger.error(
                "Result is error: {}",
                result.content.message,
                result.content, // Throwable
            )
        }
    }
```

我们提供了一些简化操作的API。例如，你可以使用 `onEachError { ... }` 来改写上面的代码：

```Kotlin
eventProcessor
    .push(event)
    // 如果出现异常结果，处理它
    .onEachError { result ->
        // 比如：输出日志
        logger.error(
            "Result is error: {}",
            result.content.message,
            result.content, // Throwable
        )
    }
```

<note>

也许在普通开发者的场景下，异常可以被省略，但是如果作为一个插件的实现，
那么我们**强烈建议**进行较为细致的结果处理，比如至少将异常信息输出至错误日志中。

</note>
