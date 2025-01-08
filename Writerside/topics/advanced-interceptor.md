# 拦截器 Interceptor

在构建 `Application` 的过程中，可以通过 `eventDispatcher` 作用域来配置事件调度器，
而在其中可以为整个调度器增加拦截器。

拦截器分为两种，一个是 **全局拦截器** (`EventDispatchInterceptor`)，一个是 **局部拦截器** (`EventInterceptor`)，
它们的区别在于它们生效的范围。

| 种类    | 范围                                             |
|-------|------------------------------------------------|
| 全局拦截器 | 整个事件调度的过程，其中可能包含多个事件处理器(以及它们可能包含的多个 _局部拦截器_ )。 |
| 局部拦截器 | 每一个事件处理器的处理流程。                                 |


## 全局拦截器

放行:

```Kotlin
val app = launchSimpleApplication {
    eventDispatcher {
        // 添加一个全局拦截器
        addDispatchInterceptor {
            // 放行、或进入下一个拦截
            invoke()
        }
    }
}
```

拦截并返回自定义结果:

```Kotlin
val app = launchSimpleApplication {
    eventDispatcher {
        addDispatchInterceptor {
            // 这里直接返回了自定义的 `Flow<EventResult` 而没有执行 `invoke`, 
            // 因此实际上没有任何事件处理器被执行, 
            // 也就是被"拦截"了。
            flowOf(EventResult.empty(), EventResult.empty())
        }
    }
}
```

## 局部拦截器

放行:

```Kotlin
val app: Application = ...

app.listeners {
    process<Event>(propertiesConsumer = {
        // 通过 propertiesConsumer 配置事件处理器的属性，
        // 这其中可以通过 addInterceptor 添加针对当前事件处理器的局部拦截器。
        addInterceptor {
            // 放行、或进入下一个拦截
            invoke()
        }
    }) { event ->
        println("Event: $event")
    }
}
```

拦截并返回自定义结果:

```Kotlin
val app: Application = ...

app.listeners {
    process<Event>(propertiesConsumer = {
        // 通过 propertiesConsumer 配置事件处理器的属性，
        // 这其中可以通过 addInterceptor 添加针对当前事件处理器的局部拦截器。
        addInterceptor {
            // 这里直接返回了自定义的 `EventResult` 而没有执行 `invoke`, 
            // 因此实际上事件处理器没有被执行, 
            // 也就是被"拦截"了。
            EventResult.empty()
        }
    }) { event ->
        println("Event: $event")
    }
}
```

## Quantcat (注解API) 支持

在 [Quantcat API](advanced-quantcat.md) 中定义了与拦截器相关的注解：`@Interceptor`，
因此在支持 Quantcat API 的地方 (比如 Spring Boot 中) 也支持使用注解来注册拦截器。

<note>

在 Quantcat API 中内置了一个很常用的注解 `@ContentTrim`, 它便是基于 `@Interceptor` 的一个衍生注解，
可以用于对标记的监听函数 (标记了 `@Listener` 的函数) 添加一个**局部拦截器**。

</note>

以 `@ContentTrim` 为例，我们介绍一下如何实现一个自定义的拦截器，
使其功能与 `@ContentTrim` 一样，并通过 `@Interceptor` 使用在某个监听函数上。

首先，我们先来看看 `@Interceptor` 的定义：

```kotlin
public annotation class Interceptor(
    /**
     * 提供一个 [AnnotationEventInterceptorFactory] 的 **实现类型**。
     * 如果此类型是 `object`，则直接使用，否则会构建一个实例。
     * 此实例 **可能会被共享**，因此请考虑处理并发。
     *
     * 如果希望添加多个拦截器，请使用多个 [Interceptor] 注解。
     */
    val value: KClass<out AnnotationEventInterceptorFactory>,

    /**
     * 提供给 [AnnotationEventInterceptorFactory] 的预期注册优先级。
     */
    val priority: Int = PriorityConstant.DEFAULT
)
```

可以看到，它有两个参数：一个用来构建拦截器的工厂类型 `value`, 和一个优先级参数 `priority`。

优先级我想它的含义不需要赘述，因此我们着重来讲一下 `value`。它需要一个 **可以被实例化** 的 `AnnotationEventInterceptorFactory` 类型的 `KClass` (Java中即为 `Class`)，
这个类型就需要我们来自定义了。

接下来，实现一个 `MyContentTrimAnnotationEventInterceptorFactory` 。

<tabs group="code">
<tab title="Kotlin" group-key="Kotlin">

```kotlin
public data object ContentTrimEventInterceptorFactory : AnnotationEventInterceptorFactory() {
    override fun create(context: AnnotationEventInterceptorFactory.Context): AnnotationEventInterceptorFactory.Result {
        TODO("待实现...")
    }
}
```

<note>

在 Kotlin 中，可以直接使用 `object` 。

</note>

</tab>
<tab title="Java" group-key="Java">

```java
public class MyContentTrimAnnotationEventInterceptorFactory implements AnnotationEventInterceptorFactory {
    @Override
    public @Nullable Result create(@NotNull AnnotationEventInterceptorFactory.Context context) {
        // TODO 待实现...
        return null;
    }
}
```

</tab>
</tabs>

工厂准备好了，接下来我们需要实现一个 **拦截器(`EventInterceptor`)** ，并在其中实现我们的功能。

> 拦截器的实现类型不需要被外界感知，此处为了方便，直接作为上述工厂类型的内部类了。自己实现的时候根据实际情况自行选择实现方式即可。

<tabs group="code">
<tab title="Kotlin" group-key="Kotlin">

```Kotlin
public data object ContentTrimEventInterceptorFactory : AnnotationEventInterceptorFactory() {
    override fun create(context: AnnotationEventInterceptorFactory.Context): AnnotationEventInterceptorFactory.Result {
        TODO("待实现...")
    }

    private data object InterceptorImpl : EventInterceptor {
        override suspend fun EventInterceptor.Context.intercept(): EventResult {
            with(eventListenerContext) {
                plainText = plainText?.trim()
            }

            return invoke()
        }
    }
}
```

</tab>
<tab title="Java" group-key="Java">

Java 中，由于存在挂起函数，你无法直接实现 `EventInterceptor`。
根据异步和阻塞的不同，你可以选择不同的Java衍生类型实现。

> 通过文档右上角可以切换展示的API风格。

<p switcher-key="%ja%">

使用 `JAsyncEventInterceptor` 可使用异步API实现 `EventInterceptor`。
对于性能来讲，其优于阻塞API。

</p>

<br/>

```Java
public class MyContentTrimAnnotationEventInterceptorFactory implements AnnotationEventInterceptorFactory {
    @Override
    public @Nullable Result create(@NotNull AnnotationEventInterceptorFactory.Context context) {
        // TODO
        return null;
    }

    private static final class InterceptorImpl implements JAsyncEventInterceptor {
        @Override
        public @NotNull CompletableFuture<EventResult> intercept(@NotNull JAsyncEventInterceptor.Context context) throws Exception {
            final var eventListenerContext = context.getSource().getEventListenerContext();
            final var plainText = eventListenerContext.getPlainText();
            if (plainText != null) {
                eventListenerContext.setPlainText(plainText.trim());
            }

            // 放行
            return context.invoke();
        }
    }
}
```
{switcher-key="%ja%"}

<p switcher-key="%jb%">

使用 `JBlockEventInterceptor` 可使用阻塞API实现 `EventInterceptor`。
对于一些复杂的逻辑，它可能会更简单一些。

</p>

```Java
public class MyContentTrimAnnotationEventInterceptorFactory implements AnnotationEventInterceptorFactory {
    @Override
    public @Nullable Result create(@NotNull AnnotationEventInterceptorFactory.Context context) {
        // TODO
        return null;
    }

    private static final class InterceptorImpl implements JBlockEventInterceptor {
        @Override
        public @NotNull EventResult intercept(@NotNull JBlockEventInterceptor.Context context) throws Exception {
            final var eventListenerContext = context.getSource().getEventListenerContext();
            final var plainText = eventListenerContext.getPlainText();
            if (plainText != null) {
                eventListenerContext.setPlainText(plainText.trim());
            }

            // 放行
            return context.invoke();
        }
    }
}
```
{switcher-key="%jb%"}

</tab>
</tabs>

实现完了拦截器，则最后一步便是在工厂中返回你的拦截器实例。

<tabs group="code">
<tab title="Kotlin" group-key="Kotlin">

```Kotlin
public data object ContentTrimEventInterceptorFactory : AnnotationEventInterceptorFactory() {
    override fun create(context: AnnotationEventInterceptorFactory.Context): AnnotationEventInterceptorFactory.Result {
        return AnnotationEventInterceptorFactory.Result.build {
            // 拦截器实例
            interceptor(InterceptorImpl)
            // 针对拦截器的一些额外配置...
            // 比如优先级, 优先级可以使用来自注解的配置
            configuration { priority = context.priority }
        }
    }

    private data object InterceptorImpl : EventInterceptor {
        // 内容省略...
    }
}
```

</tab>
<tab title="Java" group-key="Java">

```Java
public class MyContentTrimAnnotationEventInterceptorFactory implements AnnotationEventInterceptorFactory {
    @Override
    public @Nullable Result create(@NotNull AnnotationEventInterceptorFactory.Context context) {
        return Result.build(config -> {
           // 拦截器实例
           config.interceptor(new InterceptorImpl());
           config.configuration(interceptorProperties -> {
               // 针对拦截器的一些额外配置...
               // 比如优先级, 优先级可以使用来自注解的配置
               interceptorProperties.setPriority(context.getPriority());
           });
        });
    }

    private static final class InterceptorImpl implements ... {
        // 内容省略...
    }
}
```

</tab>
</tabs>

可以看到，我们使用 `AnnotationEventInterceptorFactory.Result.build` 
构建了一个 `AnnotationEventInterceptorFactory.Result` 并返回。

它的内容便是本次构建的拦截器以及其配置。如果此时返回 `null` 则代表不添加拦截器。

实现完了工厂，我们就可以在标记了 `@Listener` 的监听上使用它了。

<tabs group="code">
<tab title="Kotlin" group-key="Kotlin">

```Kotlin
@Listener
@Interceptor(MyContentTrimAnnotationEventInterceptorFactory::class)
suspend fun handle(event: Event) {
    // ...
}
```

</tab>
<tab title="Java" group-key="Java">

```Java
@Listener
@Interceptor(MyContentTrimAnnotationEventInterceptorFactory.class)
public void handle(Event event) {
    // ...
} 
```

</tab>
</tabs>

你也可以像 `@ContentTrim` 一样，使用一个自定义注解来包装它。

<tabs group="code">
<tab title="Kotlin" group-key="Kotlin">

```Kotlin
@Target(AnnotationTarget.FUNCTION)
@Interceptor(MyContentTrimAnnotationEventInterceptorFactory::class)
public annotation class MyContentTrim
```

```Kotlin
@Listener
@MyContentTrim
suspend fun handle(event: Event) {
    // ...
}
```

</tab>
<tab title="Java" group-key="Java">

```Java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
@Interceptor(MyContentTrimAnnotationEventInterceptorFactory.class)
public @interface MyContentTrim {}
```

```Java
@Listener
@MyContentTrim
public void handle(Event event) {
    // ...
} 
```

</tab>
</tabs>