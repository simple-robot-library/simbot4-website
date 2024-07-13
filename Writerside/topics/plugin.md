---
switcher-label: JavaAPI风格
---

# 插件 Plugin

<tooltip term="插件"><control>插件</control></tooltip>
是 `simbot-api` 对外公开的接口类型 `Plugin`, 它的定义如下：

```Kotlin
public interface Plugin
```

是的, `Plugin` 接口的定义**没有任何约束**, 因此 `Plugin` 可以由实现者自由定义。

## 使用插件

插件用于构建 `Application` 的过程中, 注册后根据 `Application` 对其工厂所提供的各种信息来实现其功能。

首先**假设**我们有如下这样的一个 `Plugin` 实现:

```Kotlin
class FooPlugin : Plugin {
    companion object Factory : PluginFactory<FooPlugin, Unit> {
        override val key: PluginFactory.Key = object : PluginFactory.Key {}

        override fun create(context: PluginConfigureContext, configurer: ConfigurerFunction<Unit>): FooPlugin {
            return FooPlugin()
        }
    }
}
```

### 安装

接下来我们要在 `Application` 中 **安装(`install`)** 这个假设出来的 `FooPlugin`: 

<tabs group="code">
<tab title="Kotlin" group-key="Kotlin">

```Kotlin
launchSimpleApplication {
    install(FooPlugin)
}
```

</tab>
<tab title="Java" group-key="Java">

```Java
Applications.launchApplicationAsync(Simple.INSTANCE, configurer -> {
    configurer.install(FooPlugin.Factory);
});
```
{switcher-key="%ja%"}

```Java
Applications.launchApplicationBlocking(Simple.INSTANCE, configurer -> {
    configurer.install(FooPlugin.Factory);
});
```
{switcher-key="%jb%"}

</tab>
</tabs>

### 配置

假如 `FooPlugin` 有可配置的信息的话, 也可以使用 DSL/Lambda 对其进行配置:

<tabs group="code">
<tab title="Kotlin" group-key="Kotlin">

```Kotlin
launchSimpleApplication {
    install(FooPlugin) {
        // 配置...
    }
}
```

</tab>
<tab title="Java" group-key="Java">

```Java
Applications.launchApplicationAsync(Simple.INSTANCE, configurer -> {
    configurer.install(FooPlugin.Factory, conf -> {
        // 配置...
    });
});
```
{switcher-key="%ja%"}

```Java
Applications.launchApplicationBlocking(Simple.INSTANCE, configurer -> {
    configurer.install(FooPlugin.Factory, conf -> {
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
    install(FooPlugin)
}

app.plugins.forEach { plugin ->
    // 所有注册了的插件
}
```

</tab>
<tab title="Java" group-key="Java">

```Java
Applications.launchApplicationAsync(Simple.INSTANCE, configurer -> {
    configurer.install(FooPlugin.Factory);
}).asFuture().thenAccept(app -> {
    for (var component : app.getPlugins()) {
        // 所有注册了的插件...
    }
});
```
{switcher-key="%ja%"}

```Java
var app = Applications.launchApplicationBlocking(Simple.INSTANCE, configurer -> {
    configurer.install(FooPlugin.Factory);
});

for (var plugin : app.getPlugins()) {
    // 所有注册了的插件...
}
```
{switcher-key="%jb%"}

</tab>
</tabs>
