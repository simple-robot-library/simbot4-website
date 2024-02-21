---
switcher-label: Java API
---

# 实现插件

本章节介绍如何实现一个实现一个自定义的
<tooltip term="插件">插件</tooltip>。

此处介绍的是一种简单的插件, 我们假设它的功能是安装后, 向事件调度器中注册一个事件处理器, 
此处理器用来输出所有事件的信息日志的简单插件。

> 我们假设这个插件属于在 [实现组件标识](component-dev-impl-component.md)
> 章节中的 `FooComponent`。

<procedure title="实现插件的基本步骤">
<step>实现接口 <code>Plugin</code>。</step>
<step>如果需要的话, 实现一个配置类。</step>
<step>实现 <code>PluginFactory</code> 来提供工厂。</step>
<step>可选地支持 SPI</step>
</procedure>

## 实现 Plugin 接口

实现一个插件的步骤其实与实现组件标识的步骤大差不差, 首先我们来使用类型 `FooPlugin` 实现接口 `Plugin`。

<tabs group="code">
<tab title="Kotlin" group-key="Kotlin">

```Kotlin
class FooPlugin : Plugin
```

</tab>
<tab title="Java" group-key="Java">

```Java
public class FooPlugin implements Plugin {
}
```

</tab>
</tabs>

喔, 看上去比实现一个 `Component` 还要简单呢！因为 `Plugin` 没有任何约束, 所以只需要实现它, 就可以了。

## 实现配置类

接下来, 我们提供一个对这个插件的配置类。因为事实上是没什么好配置的, 所以只需要提供一个空的配置类即可。

<tabs group="code">
<tab title="Kotlin" group-key="Kotlin">

```Kotlin
class FooPluginConfiguration
```

</tab>
<tab title="Java" group-key="Java">

```Java
public class FooPluginConfiguration {
}
```

</tab>
</tabs>

## 实现 PluginFactory

接下来, 为我们的插件类型实现它的工厂 `PluginFactory`。

> 需要注意的是, 跟 `ComponentFactory` 十分类似, 
> `PluginFactory` 的实现也建议是 **单例的**。
> 在 Kotlin 中, 它应该是 `object` 类型, 且建议直接由伴生对象实现；
> 在 Java 中, 建议使用静态字段公开一个不变的唯一实例。
> {style="note"}

> `PluginFactory` 的两个泛型类型分别代表目标 `Plugin` 的类型和其配置类的类型。
> 此处分别为 `FooPlugin` 和 `FooPluginConfiguration`。

<tabs group="code">
<tab title="Kotlin" group-key="Kotlin">

```Kotlin
class FooPlugin : Plugin {

    /** 伴生对象实现工厂 */
    companion object Factory : PluginFactory<FooPlugin, FooPluginConfiguration> {
        override val key: PluginFactory.Key = object : PluginFactory.Key {}

        override fun create(context: PluginConfigureContext, configurer: ConfigurerFunction<FooPluginConfiguration>): FooPlugin {
            configurer.invokeWith(FooPluginConfiguration())

            // 此处预留空间, 实现功能

            return FooPlugin()
        }
    }
}
```

> 在 Kotlin 中, 我们建议直接使用 **伴生对象** 作为工厂的实现类。
> {style="note"}

</tab>
<tab title="Java" group-key="Java">

```Java
public class FooPlugin implements Plugin {
    /** 静态的唯一工厂实例。 */
    public static final FooPluginFactory FACTORY = new FooPluginFactory();

       /** FooPlugin 的工厂实现 */
        public static class FooPluginFactory implements PluginFactory<FooPlugin, FooPluginConfiguration> {
        private FooPluginFactory() {
        }

        /** Key 的单例唯一实现 */
        private static final Key KEY_INSTANCE = new Key() {
        };

        @NotNull
        @Override
        public Key getKey() {
            return KEY_INSTANCE;
        }

        @NotNull
        @Override
        public FooPlugin create(@NotNull PluginConfigureContext context, @NotNull ConfigurerFunction<? super FooPluginConfiguration> configurer) {
            configurer.invoke(new FooPluginConfiguration());

            // 此处预留空间, 实现功能

            return new FooPlugin();
        }
    }
}
```

> 在 Java 中, 我们选择使用内部类, 并且隐藏构造、通过静态字段公开一个唯一单例以供使用。
> `key` 的单例实现方式也是类似的, 只不过它是匿名实现类的单例。
> {style="note"}

</tab>
</tabs>

<deflist>
<def title="key">

此工厂用于安装注册时的唯一标记, 应当是一个**唯一单例**。
当 `Application` 对同一个类型的插件进行多次安装时, 
就是使用这个 `key` 作为唯一标识来合并多次调用的。

</def>
<def title="create(...)">

提供一个插件配置上下文, 以及一个针对配置类的配置函数, 构建一个 `FooPlugin` 实例。

值得注意的是, 在示例代码中, 尽管配置类的实现是空的, 但是依旧构建了配置类并调用了配置函数。

作为插件的工厂实现, 应当尽可能确保配置函数**至少被执行一次**, 尽管配置类的实现可能是空的。


</def>
</deflist>

### PluginConfigureContext

刚刚提到, `create` 的参数中有一个 `PluginConfigureContext` 类型的“插件配置上下文”参数, 它是由 `Application` 提供的, 
里面包含了一些当前阶段可以提供的内容。

<deflist>
<def title="applicationConfiguration">

当前 `Appliation` 的配置信息。例如 `CoroutineContext` 信息。

</def>
<def title="applicationEventRegistrar">

一个 `Application` 的 **“启动阶段事件”** 注册器。此注册器中可以注册一些针对 `Application` 不同阶段的事件监听器。

在 `Application` 的启动时, 当到达了对应的阶段便会触发相应的事件。算是用于对 `Application` 自身的状态、流程的一种监听, 
也可以用来在某个特定的阶段进行某些特定的处理。

> 阶段事件的“阶段”定义可以在 `ApplicationLaunchStage` 中找到, 例如 `ApplicationLaunchStage.Launch`: 启动阶段。

</def>
<def title="components">

目前（上一步）构建得到的所有安装后的 `Component`, 
可以用来进行校验或获取一些配置等。

</def>
<def title="eventDispatcher">

`Application` 提供的**事件调度器**。
此时可以通过它来注册事件处理器, 或者保存下来在后续推送事件。

</def>
</deflist>

## 实现组件校验

我们在一开始提到过, 假设我们的 `FooPlugin` 是**属于** `FooComponent` 的。
那么在构建 `FooPlugin` 的时候就应当对组件进行校验, 只有所需的组件被安装后, 才能继续。

<note>

组件校验这一步骤**不是必须的**。因为以本示例的功能来看, `FooPlugin` 与 `FooComponent`
之间没有必然的联系, 也没有需要使用的配置信息等内容。因此事实上就算没有具体的组件标识 `FooComponent`, 
`FooPlugin` 也可以正常使用。

此处的组件校验为了较为严谨的实现组件与插件之间的关系, 严谨的依赖关系与明确的错误提示可以使得组件与插件的关系更加明晰, 
当然, 也会一定程度降低容错率。

</note>

修改我们的工厂实现, 增加校验逻辑：

<tabs group="code">
<tab title="Kotlin" group-key="Kotlin">

```Kotlin
class FooPlugin : Plugin {
    companion object Factory : PluginFactory<FooPlugin, FooPluginConfiguration> {
        override val key: PluginFactory.Key = object : PluginFactory.Key {}

        override fun create(context: PluginConfigureContext, configurer: ConfigurerFunction<FooPluginConfiguration>): FooPlugin {
            configurer.invokeWith(FooPluginConfiguration())
            
            // 校验组件
            if (context.components.find<FooComponent>() == null) {
                // 如果没有对应ID的组件, 抛出 NoSuchComponentException 异常。
                throw NoSuchComponentException(FooComponent.ID_VALUE)
            }
            
            
            // 此处预留空间, 实现功能

            return FooPlugin()
        }
    }
}
```

</tab>
<tab title="Java" group-key="Java">

```Java
public class FooPlugin implements Plugin {
    public static final FooPluginFactory FACTORY = new FooPluginFactory();

    public static class FooPluginFactory implements PluginFactory<FooPlugin, FooPluginConfiguration> {
        // 其他内容省略..

        @NotNull
        @Override
        public FooPlugin create(@NotNull PluginConfigureContext context, @NotNull ConfigurerFunction<? super FooPluginConfiguration> configurer) {
            configurer.invoke(new FooPluginConfiguration());

            // 校验组件
            if (context.getComponents().findById(FooComponent.ID_VALUE) == null) {
                // 如果没有对应ID的组件, 抛出 NoSuchComponentException 异常。
                throw new NoSuchComponentException(FooComponent.ID_VALUE);
            }

            // 此处预留空间, 实现功能

            return new FooPlugin();
        }
    }
}
```

</tab>
</tabs>

## 实现功能：注册额外的事件处理器

接下来实现我们预期的功能：注册一个事件处理器, 这个处理器会处理所有的事件, 将这个事件输出到日志中。

<tabs group="code">
<tab title="Kotlin" group-key="Kotlin">

```Kotlin
class FooPlugin : Plugin {
    companion object Factory : PluginFactory<FooPlugin, FooPluginConfiguration> {
        override val key: PluginFactory.Key = object : PluginFactory.Key {}

        override fun create(context: PluginConfigureContext, configurer: ConfigurerFunction<FooPluginConfiguration>): FooPlugin {
            configurer.invokeWith(FooPluginConfiguration())
            
            // 校验组件 省略...
            
            // 注册事件处理器
            val handle = context.eventDispatcher.register { eventContext ->
                logger.info("Event: {}", eventContext.event)
                
                // 返回一个普通的空结果
                EventResult.empty()
            }

            return FooPlugin()
        }
    }
}
```

</tab>
<tab title="Java" group-key="Java">

```Java
public class FooPlugin implements Plugin {
    public static final FooPluginFactory FACTORY = new FooPluginFactory();

    public static class FooPluginFactory implements PluginFactory<FooPlugin, FooPluginConfiguration> {
        // 其他内容省略..

        @NotNull
        @Override
        public FooPlugin create(@NotNull PluginConfigureContext context, @NotNull ConfigurerFunction<? super FooPluginConfiguration> configurer) {
            configurer.invoke(new FooPluginConfiguration());

            // 校验组件 省略...
            
            // 注册事件处理器
            var handle = context.getEventDispatcher().register(JAsyncEventListener.toListener(eventContext -> {
                LOGGER.info("Event: {}", eventContext.getEvent());

                // 返回一个普通的异步空结果
                return CompletableFuture.completedFuture(EventResult.empty());
            }));

            return new FooPlugin();
        }
    }
}
```

{switcher-key="%ja%"}

```Java
public class FooPlugin implements Plugin {
    public static final FooPluginFactory FACTORY = new FooPluginFactory();

    public static class FooPluginFactory implements PluginFactory<FooPlugin, FooPluginConfiguration> {
        // 其他内容省略..

        @NotNull
        @Override
        public FooPlugin create(@NotNull PluginConfigureContext context, @NotNull ConfigurerFunction<? super FooPluginConfiguration> configurer) {
            configurer.invoke(new FooPluginConfiguration());

            // 校验组件 省略...
            
            // 注册事件处理器
            var handle = context.getEventDispatcher().register(JBlockingEventListener.toListener(eventContext -> {
                LOGGER.info("Event: {}", eventContext.getEvent());
                
                // 返回一个普通的空结果
                return EventResult.empty();
            }));

            return new FooPlugin();
        }
    }
}
```

{switcher-key="%jb%"}

</tab>
</tabs>

## 完善功能：增加 FooPlugin 的信息

你可能发现了, 在上述示例中注册完事件处理器后返回了一个 `handle`, 这个 `handle`
是注册事件处理器后的“回执”类型 `EventListenerRegistrationHandle`, 
它可以在后续希望取消此事件处理器的时候使用 `dispose` 来取消。


<tabs group="code">
<tab title="Kotlin" group-key="Kotlin">

```Kotlin
val handle = dispatcher.register(...)

handle.dispose() // 取消注册
```

</tab>
<tab title="Java" group-key="Java">

```Java
var handle = dispatcher.register(...);

handle.dispose(); // 取消注册
```

</tab>
</tabs>

如果你希望允许用户在后续从 `FooPlugin` 中取消你所注册的功能, 那么我们便可以将这个 `handle` 记录在我们的 `FooPlugin` 中。

<tabs group="code">
<tab title="Kotlin" group-key="Kotlin">

```Kotlin
class FooPlugin(val handle: EventListenerRegistrationHandle) : Plugin {
    companion object Factory : PluginFactory<FooPlugin, FooPluginConfiguration> {
        override val key: PluginFactory.Key = object : PluginFactory.Key {}

        override fun create(context: PluginConfigureContext, configurer: ConfigurerFunction<FooPluginConfiguration>): FooPlugin {
            configurer.invokeWith(FooPluginConfiguration())
            
            // 校验组件, 省略
            
            // 注册事件处理器, 省略
            val handle = context.register(...)

            // 记录到 FooPlugin 中
            return FooPlugin(handle)
        }
    }
}
```

</tab>
<tab title="Java" group-key="Java">

```Java
public class FooPlugin implements Plugin {
    public static final FooPluginFactory FACTORY = new FooPluginFactory();

    /** 记录 handle */
    private final EventListenerRegistrationHandle handle;

    public FooPlugin(EventListenerRegistrationHandle handle) {
        this.handle = handle;
    }

    /** 获取 handle */
    public EventListenerRegistrationHandle getHandle() {
        return handle;
    }

    public static class FooPluginFactory implements PluginFactory<FooPlugin, FooPluginConfiguration> {
        // 其他内容省略..

        @NotNull
        @Override
        public FooPlugin create(@NotNull PluginConfigureContext context, @NotNull ConfigurerFunction<? super FooPluginConfiguration> configurer) {
            configurer.invoke(new FooPluginConfiguration());

            // 校验组件, 省略

            // 注册事件处理器, 省略
            var handle = context.getEventDispatcher().register(...);

            // 记录到 FooPlugin 中
            return new FooPlugin(handle);
        }
    }
}
```

</tab>
</tabs>

如此一来, `FooPlugin` 这个类型本身也具有了一定的功能性。

## 支持 SPI {id="impl_spi_provide"}

> 是否支持 SPI 取决于你的功能需求, 这不是必须的。

### 实现 PluginFactoryProvider {id="impl-plugin-factory-provider"}

> `PluginFactoryProvider` 主要为 JVM 平台服务, 不过此接口是多平台实现的。

接下来, 为我们的 `FooPlugin.Factory` 实现一个可以通过 SPI 加载的供应者。

`PluginFactory` 的作用是构建 `Plugin`, 而 `PluginFactoryProvider` 的作用则是加载 `PluginFactory`。

<tabs group="code">
<tab title="Kotlin" group-key="Kotlin">

```Kotlin
class FooPluginFactoryProvider : PluginFactoryProvider<FooPluginConfiguration> {
    override fun configurersLoader(): Sequence<PluginFactoryConfigurerProvider<FooPluginConfiguration>>? {
        return null
    }

    override fun provide(): PluginFactory<*, FooPluginConfiguration> {
        return FooPlugin.Factory
    }
}
```

</tab>
<tab title="Java" group-key="Java">

```Java
public class FooPluginFactoryProvider implements PluginFactoryProvider<FooPluginConfiguration> {
    @Nullable
    @Override
    public Sequence<PluginFactoryConfigurerProvider<FooPluginConfiguration>> configurersLoader() {
        return null;
    }

    @NotNull
    @Override
    public PluginFactory<?, FooPluginConfiguration> provide() {
        return FooPlugin.FACTORY;
    }
}
```

</tab>
</tabs>

### 添加 services 文件 {id="add-services"}

接下来, 在你的项目资源目录的
<path>resources/META-INF/services</path>
中创建一个文件：
`love.forte.simbot.plugin.PluginFactoryProvider`, 
并在其中填入你的实现类的全限定名称, 例如：

```
com.example.foo.FooPluginFactoryProvider
```

### 修改 module-info.java {id="modify-module-info-java"}

同样的, 如果你提供了模块化信息文件 `module-info.java`, 你还需要在其中补充上相关信息：

```Java
import love.forte.simbot.plugin.PluginFactoryProvider;

module com.example.foo {
    // requires、exports 等内容省略
    
    provides PluginFactoryProvider with com.example.foo.FooPluginFactoryProvider;
}
```

## 结束

以上就是包括必要操作、建议行为等内容在内的完整的实现一个
<tooltip term="插件">插件</tooltip>
的步骤了。

现在你可以将你的插件安装在任意的 `Application` 中, 
或者在 Spring Boot 等支持 SPI 的地方自动安装啦~

<tabs group="code">
<tab title="Kotlin" group-key="Kotlin">

```Kotlin
val app = launchSimpleApplication {
    install(FooPlugin)
}

// Kotlin 可以直接通过扩展函数根据类型寻找目标
val fooPlugin = app.plugins.find<FooPlugin>()
println(fooPlugin)
```

</tab>
<tab title="Java" group-key="Java">

```Java
Applications.launchApplicationAsync(Simple.INSTANCE, configurer -> {
    // 注册
    configurer.install(FooPlugin.FACTORY);
}).asFuture().thenAccept(app -> {
    // 寻找注册的组件中的 FooPlugin
    var fooComponent = app.getPlugins().stream()
            .filter(plugin -> plugin instanceof FooPlugin)
            .findFirst()
            .orElse(null);

    System.out.println(fooComponent);
});
```

{switcher-key="%ja%"}

```Java
final var app = Applications.launchApplicationBlocking(Simple.INSTANCE, configurer -> {
    // 注册
    configurer.install(FooPlugin.FACTORY);
});

// 寻找注册的组件中的 FooPlugin
var fooPlugin = app.getPlugins().stream()
        .filter(plugin -> plugin instanceof FooPlugin)
        .findFirst()
        .orElse(null);

System.out.println(fooPlugin);
```

{switcher-key="%jb%"}

</tab>
</tabs>
