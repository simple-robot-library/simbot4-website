---
switcher-label: Java API
---

# 实现组件标识

<tooltip term="组件标识">组件标识</tooltip>
是实现一个组件的基础。

<procedure title="实现组件标识的基本步骤">
   <step>实现接口 <code>Component</code>。</step>
   <step>如果需要的话，实现一个配置类。</step>
   <step>实现 <code>ComponentFactory</code> 来提供工厂。</step>
   <step>如果需要的话，实现 SPI 的供应者 <code>ComponentFactoryProvider</code>。</step>
</procedure>

## 实现 Component 接口

**假设** 你想要实现一个叫做 `FooComponent` 的组件标识，且它的 id 为 `com.example.foo`。

接下来，我们先来实现它，定义此类型并实现 `Component` 接口：

<tabs group="code">
<tab title="Kotlin" group-key="Kotlin">

```Kotlin
class FooComponent : Component {
    override val id: String = "com.example.foo"
    override val serializersModule: SerializersModule = EmptySerializersModule()
}
```

</tab>
<tab title="Java" group-key="Java">

```Java
public class FooComponent implements Component {
    @NotNull
    @Override
    public String getId() {
        return "com.example.foo";
    }

    @NotNull
    @Override
    public SerializersModule getSerializersModule() {
        return SerializersModuleBuildersKt.EmptySerializersModule();
    }
}
```

</tab>
</tabs>

<deflist>
<def title="id">
这个组件的唯一标识。建议使用一个不会被重复的命名，例如类似于 Java 包的命名方式，
<path>域名倒置 + <code>.</code> + 简短的组件名称</path> 等。

<tip>不建议使用 <code>simbot</code> 开头，因为这通常由simbot官方实现的组件使用。</tip>
</def>
<def title="serializersModule">
以组件为单位向 <code>Application</code> 
提供一切可能需要用到的序列化信息，
例如 <code>SerializableBotConfiguration</code> 
或 <code>Message.Element</code> 
实现。
<warning>
如果仅使用 Java 实现，你可能无法提供
<control>有价值的</control>
<code>SerializersModule</code> 结果。
因为 <code>kotlinx-serialization</code> 
有编译器行为，不支持直接使用 Java。 

</warning>
</def>
</deflist>

## 实现配置类

由于我们没有什么需要配置的东西，因此仅提供一个空的配置类即可。

<tabs group="code">
<tab title="Kotlin" group-key="Kotlin">

```Kotlin
class FooComponentConfiguration
```

</tab>
<tab title="Java" group-key="Java">

```Java
public class FooComponentConfiguration {
}
```

</tab>
</tabs>

## 实现 ComponentFactory

接下来，实现 `ComponentFactory`，使其用于构建我们的 `FooComponent`。

> 需要注意的是，`ComponentFactory` 的实现建议是 **单例的**。
> 在 Kotlin 中，它应该是 `object` 类型，且建议直接由伴生对象实现；
> 在 Java 中，建议使用静态字段公开一个不变的唯一实例。
> {style="note"}

> `ComponentFactory` 的两个泛型类型分别代表目标 `Component` 的类型和其配置类的类型。
> 此处分别为 `FooComponent` 和 `FooComponentConfiguration`。


<tabs group="code">
<tab title="Kotlin" group-key="Kotlin">

```Kotlin
class FooComponent : Component {
    override val id: String = "com.example.foo"
    override val serializersModule: SerializersModule = EmptySerializersModule()

    /** 伴生对象实现工厂 */
    companion object Factory : ComponentFactory<FooComponent, FooComponentConfiguration> {
        override val key: ComponentFactory.Key = object : ComponentFactory.Key {}
        override fun create(context: ComponentConfigureContext, configurer: ConfigurerFunction<FooComponentConfiguration>): FooComponent {
            FooComponentConfiguration().invokeBy(configurer)
            return FooComponent()
        }
    }
}
```

> 在 Kotlin 中，我们建议直接使用 **伴生对象** 作为工厂的实现类。
> {style="note"}

</tab>
<tab title="Java" group-key="Java">

```Java
public class FooComponent implements Component {
    /** 静态的唯一工厂实例。 */
    public static final FooComponentFactory FACTORY = new FooComponentFactory();

    @NotNull
    @Override
    public String getId() {
        return "com.example.foo";
    }

    @NotNull
    @Override
    public SerializersModule getSerializersModule() {
        return SerializersModuleBuildersKt.EmptySerializersModule();
    }

    /** FooComponent 的工厂实现 */
    public static class FooComponentFactory implements ComponentFactory<FooComponent, FooComponentConfiguration> {
        private FooComponentFactory(){
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
        public FooComponent create(@NotNull ComponentConfigureContext context, @NotNull ConfigurerFunction<? super FooComponentConfiguration> configurer) {
            configurer.invoke(new FooComponentConfiguration());
            return new FooComponent();
        }
    }
}
```

> 在 Java 中，我们选择使用内部类，并且隐藏构造、通过静态字段公开一个唯一单例以供使用。
> `key` 的单例实现方式也是类似的，只不过它是匿名实现类的单例。
> {style="note"}

</tab>
</tabs>

<deflist>
<def title="key">

此工厂用于安装注册时的唯一标记，应当是一个**唯一单例**。
当 `Application` 对同一个类型的组件进行多次安装时，
就是使用这个 `key` 作为唯一标识来合并多次调用的。

</def>
<def title="create(...)">

提供一个组件配置上下文，以及一个针对配置类的配置函数，构建一个 `FooComponent` 实例。

值得注意的是，在示例代码中，尽管配置类的实现是空的，但是依旧构建了配置类并调用了配置函数。

作为组件的工厂实现，应当尽可能确保配置函数**至少被执行一次**，尽管配置类的实现可能是空的。


</def>
</deflist>

### ComponentConfigureContext

刚刚提到，`create` 的参数中有一个 `ComponentConfigureContext` 类型的“组件配置上下文”参数，它是由 `Application` 提供的，
里面包含了一些当前阶段可以提供的内容。

<deflist>
<def title="applicationConfiguration">

当前 `Appliation` 的配置信息。例如 `CoroutineContext` 信息。

</def>
<def title="applicationEventRegistrar">

一个 `Application` 的 **“启动阶段事件”** 注册器。此注册器中可以注册一些针对 `Application` 不同阶段的事件监听器。

在 `Application` 的启动时，当到达了对应的阶段便会触发相应的事件。算是用于对 `Application` 自身的状态、流程的一种监听，
也可以用来在某个特定的阶段进行某些特定的处理。

> 阶段事件的“阶段”定义可以在 `ApplicationLaunchStage` 中找到，例如 `ApplicationLaunchStage.Launch`: 启动阶段。

</def>
</deflist>

## 整理代码

其实至此，一个最简单的组件标识和它的工厂实现已经结束了。但是对于一个组件标识的实现，
我们有一些非硬性的建议：

<deflist>
<def title="id 常量化">

对于 `Component.id`，我们建议直接作为常量且对外公开，
以应对一些不构建 `Component` 便可获取 `id` 值的场景。

</def>
<def title="serializersModule 静态化">

对于 `Component.serializersModule`，我们建议对其的获取静态化，
以应对一些不构建 `Component` 便可获取 `serializersModule` 信息的场景。

</def>
<def title="实现 Provider 来支持 SPI 自动加载">

建议再实现一个 `ComponentFactoryProvider` 来支持部分场景下（例如 Spring Boot 环境）
通过 SPI 自动加载你的组件。**下文会详细介绍。**

</def>
</deflist>

根据上述的建议（除了 SPI 实现），我们整理一下现有的代码，让它们符合这些建议。

<tabs group="code">
<tab title="Kotlin" group-key="Kotlin">

```Kotlin
class FooComponent : Component {
    override val id: String get() = ID_VALUE
    override val serializersModule: SerializersModule get() = Factory.SerializersModule

    /** 伴生对象实现的工厂实现 */
    companion object Factory : ComponentFactory<FooComponent, FooComponentConfiguration> {
        /** id 常量化 */
        const val ID_VALUE: String = "com.example.foo"
        /** serializersModule "静态化" */
        @JvmField
        val SerializersModule: SerializersModule = EmptySerializersModule()
        
        override val key: ComponentFactory.Key = object : ComponentFactory.Key {}
        override fun create(context: ComponentConfigureContext, configurer: ConfigurerFunction<FooComponentConfiguration>): FooComponent {
            FooComponentConfiguration().invokeBy(configurer)
            return FooComponent()
        }
    }
}
```

</tab>
<tab title="Java" group-key="Java">

```Java
public class FooComponent implements Component {
    /** id 常量化 */
    public static final String ID_VALUE = "com.example.foo";
    /** serializersModule 静态化 */
    public static final SerializersModule SerializersModule = SerializersModuleBuildersKt.EmptySerializersModule();

    /** 静态的唯一工厂实例。 */
    public static final FooComponentFactory FACTORY = new FooComponentFactory();

    @NotNull
    @Override
    public String getId() {
        return ID_VALUE;
    }

    @NotNull
    @Override
    public SerializersModule getSerializersModule() {
        return SerializersModule;
    }

    /** FooComponent 的工厂实现 */
    public static class FooComponentFactory implements ComponentFactory<FooComponent, FooComponentConfiguration> {
        private FooComponentFactory(){
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
        public FooComponent create(@NotNull ComponentConfigureContext context, @NotNull ConfigurerFunction<? super FooComponentConfiguration> configurer) {
            configurer.invoke(new FooComponentConfiguration());
            return new FooComponent();
        }
    }
}
```

</tab>
</tabs>

> `serializersModule` 的“静态化”可以使用属性、也可以使用函数。这里选择使用属性的方式。

## 支持 SPI {id="impl_spi_provide"}

> 是否支持 SPI 取决于你的功能需求，这不是必须的。

### 实现 ComponentFactoryProvider

> `ComponentFactoryProvider` 主要为 JVM 平台服务，不过此接口是多平台实现的。

接下来，为我们的 `FooComponent.Factory` 实现一个可以通过 SPI 加载的供应者。

`ComponentFactory` 的作用是构建 `Component`，而 `ComponentFactoryProvider` 的作用则是加载 `ComponentFactory`。

<tabs group="code">
<tab title="Kotlin" group-key="Kotlin">

```Kotlin
class FooComponentFactoryProvider : ComponentFactoryProvider<FooComponentConfiguration> {
    override fun loadConfigurers(): Sequence<ComponentFactoryConfigurerProvider<FooComponentConfiguration>>? {
        return null
    }

    override fun provide(): ComponentFactory<*, FooComponentConfiguration> {
        return FooComponent.Factory
    }
}
```

</tab>
<tab title="Java" group-key="Java">

```Java
public class FooComponentFactoryProvider implements ComponentFactoryProvider<FooComponentConfiguration> {
    @Nullable
    @Override
    public Sequence<ComponentFactoryConfigurerProvider<FooComponentConfiguration>> loadConfigurers() {
        return null;
    }

    @NotNull
    @Override
    public ComponentFactory<?, FooComponentConfiguration> provide() {
        return ooComponent.FACTORY;
    }
}
```

</tab>
</tabs>

### 添加 services 文件

接下来，在你的项目资源目录的
<path>resources/META-INF/services</path>
中创建一个文件：
`love.forte.simbot.component.ComponentFactoryProvider`，
并在其中填入你的实现类的全限定名称，例如：

```
com.example.foo.FooComponentFactoryProvider
```

### 修改 module-info.java

同样的，如果你提供了模块化信息文件 `module-info.java`，你还需要在其中补充上相关信息：

```Java
import love.forte.simbot.component.ComponentFactoryProvider;

module com.example.foo {
    // requires、exports 等内容省略
    
    provides ComponentFactoryProvider with com.example.foo.FooComponentFactoryProvider;
}
```

## 结束

以上就是包括必要操作、建议行为等内容在内的完整的实现一个
<tooltip term="组件标识">组件标识</tooltip>
的步骤了。

现在你可以将你的组件安装在任意的 `Application` 中，
或者在 Spring Boot 等支持 SPI 的地方自动安装啦~

<tabs group="code">
<tab title="Kotlin" group-key="Kotlin">

```Kotlin
val app = launchSimpleApplication {
    install(FooComponent)
}

// Kotlin 可以直接通过扩展函数根据类型寻找目标
val fooComponent = app.components.find<FooComponent>()
println(fooComponent)
```

</tab>
<tab title="Java" group-key="Java">

```Java
Applications.launchApplicationAsync(Simple.INSTANCE, configurer -> {
    // 注册
    configurer.install(FooComponent.FACTORY);
}).asFuture().thenAccept(app -> {
    // 寻找注册的组件中的 FooComponent
    // 也可以使用类型寻找，此处图省事儿，直接通过id寻找
    var fooComponent = app.getComponents().findById(FooComponent.ID_VALUE);
    System.out.println(fooComponent);
});
```

{switcher-key="%ja%"}

```Java
var app = Applications.launchApplicationBlocking(Simple.INSTANCE, configurer -> {
    configurer.install(FooComponent.FACTORY);
});

// 也可以使用类型寻找，此处图省事儿，直接通过id寻找
var fooComponent = app.getComponents().findById(FooComponent.ID_VALUE);
System.out.println(fooComponent);
```

{switcher-key="%jb%"}


</tab>
</tabs>