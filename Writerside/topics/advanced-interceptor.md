# 拦截器 Interceptor

<primary-label ref="doc-wip" />

<warning>

文档待施工...

</warning>


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

在 Quantcat API 中内置了一个很常用的注解 `@ContentTrim`, 它便是基于 `@Interceptor` 的一个衍生注解，
可以用于对标记的监听函数 (标记了 `@Listener` 的函数) 添加一个**局部拦截器**。

<tabs group="code">
<tab title="Kotlin" group-key="Kotlin">



</tab>
<tab title="Java" group-key="Java">



</tab>
</tabs>

TODO