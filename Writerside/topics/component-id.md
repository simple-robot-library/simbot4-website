---
switcher-label: JavaAPI风格
---

# 组件标识 Component

<tooltip term="组件标识">组件标识</tooltip> 
指的是 `simbot-api` 模块中对外提供的接口类型 `Component`。它的定义大致如下：

```Kotlin
public interface Component {
    /**
     * 一个组件的ID。
     * 组件id建议使用类似于Java包路径的格式, 
     * 例如 `org.example.Sample` 并尽量避免重复。
     */
    public val id: String

    /**
     * 组件对外提供的统合所有所需的序列化信息。
     * 通常为 message 类型的序列化或文件配置类的序列化信息。
     */
    public val serializersModule: SerializersModule
    
    // 其他省略...
}
```

<deflist>
<def title="id">

此组件的一个唯一ID标识。

</def>
<def title="serializersModule">

此组件的各种序列化信息, 例如消息元素、可序列化的 `Bot` 配置类等。

</def>
</deflist>

## 使用组件标识

组件标识用于构建 `Application` 的过程中, 先注册组件标识, 用来告知后续步骤将要被注册的
<tooltip term="插件">插件</tooltip>
并为它提供一些可能的配置。

首先我们**假设**有如下这样的一个 `Component` 实现：

```Kotlin
class FooComponent : Component {
    override val id: String = "example.foo"
    override val serializersModule: SerializersModule = EmptySerializersModule()

    companion object Factory : ComponentFactory<FooComponent, Unit> {
        override val key: ComponentFactory.Key = object : ComponentFactory.Key {}
        override fun create(context: ComponentConfigureContext, configurer: ConfigurerFunction<Unit>): FooComponent {
            return FooComponent()
        }
    }
}
```

### 安装

接下来我们要在 `Application` 中 **安装(`install`)** 这个假设出来的 `FooComponent`: 

<tabs group="code">
<tab title="Kotlin" group-key="Kotlin">

```Kotlin
launchSimpleApplication {
    install(FooComponent)
}
```

</tab>
<tab title="Java" group-key="Java">

```Java
Applications.launchApplicationAsync(Simple.INSTANCE, configurer -> {
    configurer.install(FooComponent.Factory);
});
```
{switcher-key="%ja%"}

```Java
Applications.launchApplicationBlocking(Simple.INSTANCE, configurer -> {
    configurer.install(FooComponent.Factory);
});
```
{switcher-key="%jb%"}

</tab>
</tabs>

### 配置

假如 `FooComponent` 有可配置的信息的话, 也可以使用 DSL/Lambda 对其进行配置:

<tabs group="code">
<tab title="Kotlin" group-key="Kotlin">

```Kotlin
launchSimpleApplication {
    install(FooComponent) {
        // 配置...
    }
}
```

</tab>
<tab title="Java" group-key="Java">

```Java
Applications.launchApplicationAsync(Simple.INSTANCE, configurer -> {
    configurer.install(FooComponent.Factory, conf -> {
        // 配置...
    });
});
```
{switcher-key="%ja%"}

```Java
Applications.launchApplicationBlocking(Simple.INSTANCE, configurer -> {
    configurer.install(FooComponent.Factory, conf -> {
        // 配置...
    });
});
```
{switcher-key="%jb%"}

</tab>
</tabs>

### 事后获取

当我们注册完成、并启动了一个 `Application` 之后, 如果想要获取注册过的组件, 则可以在 `Application` 
中找到它们：

<tabs group="code">
<tab title="Kotlin" group-key="Kotlin">

```Kotlin
val app = launchSimpleApplication {
    install(FooComponent)
}

app.components.forEach { component ->
    // 所有注册了的组件
}
```

</tab>
<tab title="Java" group-key="Java">

```Java
Applications.launchApplicationAsync(Simple.INSTANCE, configurer -> {
    configurer.install(FooComponent.Factory);
}).asFuture().thenAccept(app -> {
    for (var component : app.getComponents()) {
        // 所有注册了的组件...
    }
});
```
{switcher-key="%ja%"}

```Java
var app = Applications.launchApplicationBlocking(Simple.INSTANCE, configurer -> {
    configurer.install(FooComponent.Factory);
});

for (var component : app.getComponents()) {
    // 所有注册了的组件...
}
```
{switcher-key="%jb%"}

</tab>
</tabs>




