---
switcher-label: Java API
---

# Application

`Application` 是对一组注册的组件和插件们的统一运行环境。
组件和插件们总要依附在一个 `Application` 上。

`simbot-core` 模块提供了一个简单的普通实现：`SimpleApplication`, 
下文将会以这个简单的普通实现为例进行说明。

> Spring Boot starter 模块 `simbot-core-spring-boot-starter` 有另外一个类型的实现, 
> 不过它大部分内容也是基于 `SimpleApplication`, 且不需要也不应手动构造, 所以不做介绍。

## 创建与启动

<tabs group="Code">
<tab title="Kotlin" group-key="Kotlin">

在 Kotlin 中, 使用 `launchApplication(Factory) { config... }` 配置并启动一个 `Application`。

其中, `Factory` 是 `Application` 的工厂实例, 在 `simbot-core` 中它是 `Simple`。

```Kotlin
val app = launchApplication(Simple) {
    config {
        // 一些 Application 的配置, 例如协程上下文
        coroutineContext = Dispatchers.Default
    }

    // 一些事件调度器的配置, 例如协程上下文、拦截器等
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

    // 安装组件或插件, 以及额外的配置
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

在 Java 中, 使用 `Applications.launchApplicationXxx(Factory) { config... }` 
配置并启动一个 `Application`。

其中, `Factory` 是 `Application` 的工厂实例, 在 `simbot-core` 中它是 `Simple.INSTANCE`。

```Java
var applicationAsync = Applications.launchApplicationAsync(Simple.INSTANCE, appConfigurer -> {
    // Application 的配置信息
    appConfigurer.config(appConfig -> {
        // 比如配置协程上下文
        // 此处仅做示例：配置一个自定义的线程池到上下文中。
        // 并不建议非常随意的使用 Executors 中的内容。再强调一遍, 此处仅作为示例。
        appConfig.setCoroutineContext(ExecutorsKt.from(Executors.newCachedThreadPool()));
    });


    // 事件调度器的配置信息
    appConfigurer.eventDispatcher(dispatcherConfig -> {
        // 例如可以配置协程上下文、添加拦截器之类的。
        // 配置协程上下文上面有示例的, 此处就不再重复了, 是类似的效果。

        // 添加一个事件处理拦截器
        dispatcherConfig.addInterceptor(EventInterceptors.async(context -> {
            // 拦截...
            // 假设：重新设置事件处理过程中的 `plainText`, 去除其前后空白字符
            var listenerContext = context.getSource().getEventListenerContext();
            var plainText = listenerContext.getPlainText();
            listenerContext.setPlainText(plainText == null ? null : plainText.trim());

            return context.invoke();
        }));

        // 安装组件、插件, 以及可选的进行配置
        appConfigurer.install(...);
        appConfigurer.install(..., config -> {
            // 配置...
        });
    });
});

// 异步结果可转化为 CompletableFuture
var future = applicationAsync.asFuture();
// ...

```
{switcher-key="%ja%"}

```Java
var application = Applications.launchApplicationBlocking(Simple.INSTANCE, appConfigurer -> {
    // Application 的配置信息
    appConfigurer.config(appConfig -> {
        // 比如配置协程上下文
        // 此处仅做示例：配置一个自定义的线程池到上下文中。
        // 并不建议非常随意的使用 Executors 中的内容。再强调一遍, 此处仅作为示例。
        appConfig.setCoroutineContext(ExecutorsKt.from(Executors.newCachedThreadPool()));
    });


    // 事件调度器的配置信息
    appConfigurer.eventDispatcher(dispatcherConfig -> {
        // 例如可以配置协程上下文、添加拦截器之类的。
        // 配置协程上下文上面有示例的, 此处就不再重复了, 是类似的效果。

        // 添加一个事件处理拦截器
        dispatcherConfig.addInterceptor(EventInterceptors.block(context -> {
            // 拦截...
            // 假设：重新设置事件处理过程中的 `plainText`, 去除其前后空白字符
            var listenerContext = context.getSource().getEventListenerContext();
            var plainText = listenerContext.getPlainText();
            listenerContext.setPlainText(plainText == null ? null : plainText.trim());

            return context.invoke();
        }));

        // 安装组件、插件, 以及可选的进行配置
        appConfigurer.install(...);
        appConfigurer.install(...,config -> {
            // 配置...
        });
    });
});

// ...
```
{switcher-key="%jb%"}

</tab>
</tabs>

构建完 `Application` 便可以开始注册事件处理器和 `Bot` 了。

## 事件处理器的注册

有关事件处理器的相关描述可前往 [事件监听与处理](basic-event-listener.md) 章节阅读。

## Bot的注册

有关Bot注册的相关描述可前往 [Bot管理器](BotManager.md) 章节阅读。

## Spring中获取Application

如果你在使用 Spring Boot starter，那么你可以直接通过bean注入的方式得到 `Application` 实例。

<tabs group="Code">
<tab title="Kotlin" group-key="Kotlin">

```Kotlin
@Component
class MyComponent 
@Autowired // 其实注解可以省略，这里只是为了让'注入'行为比较显眼儿 
constructor(
    private val application: Application // 构造注入 Application
    
) {
    // ...
}
```

</tab>
<tab title="Java" group-key="Java">

```Java
@Component
public class MyComponent {
    private final Application application;
    
    @Autowired // 其实注解可以省略，这里只是为了让'注入'行为比较显眼儿 
    public MyComponent(Application application) { // 构造注入 Application
        this.application = application;
    }
    
    // ...
}
```

</tab>
</tabs>

你也可以基于此间接地获取 `BotManager`、`Bot` 等其他信息。

<tabs group="Code">
<tab title="Kotlin" group-key="Kotlin">

```Kotlin
@Component
class MyComponent(
    private val application: Application
) {
    // 假设这里是个定时任务
    // 或者其他什么跟事件没关系的地方
    fun runTask() {
        // 得到所有的BotManager并遍历
        for (botManager in application.botManagers) {
            // ...
            // 得到每一个BotManager中的所有已注册且未被关闭的Bot并遍历
            for (bot in botManager.all()) {
                // ...
            }
        }
    }
}
```

</tab>
<tab title="Java" group-key="Java">

```Java
@Component
public class MyComponent {
    private final Application application;

    public MyComponent(Application application) {
        this.application = application;
    }

    public void runTask() {
        // 得到所有的BotManager并遍历
        for (var botManager : application.getBotManagers()) {
            // ...
            // 得到每一个BotManager中的所有已注册且未被关闭的Bot并遍历
            for (var it = botManager.all().iterator(); it.hasNext(); ) {
                var bot = it.next();
                // ...
            }
        }
    }
}
```

</tab>
</tabs>