---
switcher-label: JavaAPI风格
---

<show-structure for="chapter,procedure" depth="3"/>

# Java友好 ♥

<tldr>
对 simbot4 中提供的诸项 Java 友好方案的介绍。
</tldr>

## Lambda

simbot4 中相比于 simbot3, 对于部分 lambda 的场景进行了简单的优化,
在 Java 中需要手写返回 `return Unit.INSTANCE;` 的地方大大减少了。

## 阻塞与异步

simbot4
借助编译器插件
[kotlin-suspend-transform-compiler-plugin][kstcp]
为 Java 和其他可能的平台提供阻塞、异步以及 `Reserve` 风格的API（`Reserce` 后续小节会讲）。

[kstcp]: https://github.com/ForteScarlet/kotlin-suspend-transform-compiler-plugin

例如, 一个 Kotlin 中的挂起函数：

```Kotlin
suspend fun run(): Int {
    // ...
}
```

在 Java 中是难以调用的, 因为挂起函数在编译后会经过“编译魔法”而产生一些变化。

通过编译器插件, 它便会为这个 `run()` 方法产生那么几个新的、可供 Java 直接使用的 API：

```Java
/** 阻塞桥接, 等待(阻塞)挂起函数完成后返回结果 */
public int runBlocking() { ... }

/** 异步桥接, 返回 future */
public CompletableFuture<? extends Integer> runAsync() { ... }

/** Reserve 预处理桥接, 返回一个预处理结果 Reserve */
public SuspendReserve<? extends Integer> runReserve() { ... }
```


### 💡文档表现

在文档中，可能会同时提供阻塞、异步、预处理(响应式)三种风格的API。

此时你可以通过文档**右上角**的切换标签
<control>JavaAPI风格</control>
来修改展示的API风格。

<note>

动动手试试吧！现在显示的风格是：
<control switcher-key="%ja%">%ja%</control>
<control switcher-key="%jb%">%jb%</control>
<control switcher-key="%jr%">%jr%</control>

</note>


### ⚠️注意异步中的异常处理

当你在使用**异步**结果时（也包括下文会提到的各响应式类型结果），你需要多一些心眼儿。

以 `CompletableFuture` 为例，如果不做任何操作，
你很有可能会丢失且无法感知到**异常**。

例如：

```Java
var future = xxx.runAsync();
```

此时，如果 `runAsync` 内异步任务出现异常，代码中（或者控制台、日志等）实际上不会出现任何信息。
你需要使用明确的API进行异常处理：

```Java
// 假设返回结果是 Integer
CompletableFuture<? extends Integer> future = xxx.runAsync();

// 可以使用 exceptionally 在出现异常时计算一个新的结果
future.exceptionally(exception -> {
    logger.error("异常了！", exception);
    // 更建议返回一个真实的结果，但是确保下游不会出错的情况下，使用 `null` 也无可厚非。
    return null;
});

// 可以使用 handle 同时处理正常结果或异常结果
future.handle((value, exception) -> {
    if (exception != null) {
        // 如果出现了异常
        logger.error("异常了！", exception);
        return null;
    }
    
    // 正常结果
    return value;
});
        
// 可以使用 whenComplete 来注册回调，来感知到异常，而不影响下游的直接使用。
future.whenComplete((value, exception) -> {
    if (exception != null) {
        // 如果出现了异常
        logger.error("异常了！", exception);
        
        return;
    }

    // 使用 value 正常结果
    
});
```

下文所述的各种响应式结果通常也可能会有类似的问题，需要多加注意喔。


## SuspendReserve 预执行桥接器

前文提到了编译器插件会生成返回值为 `SuspendReserve` 预处理结果桥接函数,
借助它便可以将挂起函数转化为更多可以支持的类型, 尤其是 Java 中的各种**响应式**结果。

```Java
var reserve = xxx.runReserve();

// 将挂起函数的结果作为 [[[Reactor|https://projectreactor.io/]]] 的 `reactor.core.publisher.Mono` 返回
// 使用此转化器需要确保运行时环境中存在 [[[kotlinx-coroutines-reactor|https://github.com/Kotlin/kotlinx.coroutines/tree/master/reactive]]] 的相关依赖
var mono = reserve.transform(SuspendReserves.mono());

// 将挂起函数的结果作为 [[[RxJava 2.x|https://github.com/ReactiveX/RxJava]]] 的 `io.reactivex.Maybe` 返回
// 使用此转化器需要确保运行时环境中存在 [[[kotlinx-coroutines-rx2|https://github.com/Kotlin/kotlinx.coroutines/tree/master/reactive]]] 的相关依赖
var rx2Maybe = reserve.transform(SuspendReserves.rx2Maybe());

// 将挂起函数的结果作为 [[[RxJava 3.x|https://github.com/ReactiveX/RxJava]]] 的 `io.reactivex.rxjava3.core.Maybe` 返回
// 使用此转化器需要确保运行时环境中存在 [[[kotlinx-coroutines-rx3|https://github.com/Kotlin/kotlinx.coroutines/tree/master/reactive]]] 的相关依赖
var rx3Maybe = reserve.transform(SuspendReserves.rx3Maybe());
```

当然, 通过 `SuspendReserve` 你也可以做到普通的阻塞与异步效果。

```Java
var reserve = xxx.runReserve();

// 阻塞并等待返回
var block = reserve.transform(SuspendReserves.block());

// 将挂起函数的结果作为 `CompletableFuture` 返回
var future = reserve.transform(SuspendReserves.async());
```

## Spring Boot 支持

simbot4 提供了基于 **Spring Boot 3** 的快速应用实现模块：`simbot-core-spring-boot-starter`。

starter 模块也实现了 [量子猫](advanced-quantcat.md) 模块, 提供基于**注解**的高效开发方案。
