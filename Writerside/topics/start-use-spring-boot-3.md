# 使用 Spring Boot 3

<tldr>

在 JVM 平台下使用 Spring Boot 3 配合 simbot4 进行
<control>快速开发</control>。

</tldr>

## 准备

### Java 17

simbot4 的 Spring Boot starter 基于 Spring Boot 3，因此 Java 的版本至少为 **Java17** 。

### 创建 Spring Boot 3 项目

首先你得有个 Spring Boot 3 项目, 你可以前往 [Spring Initializr][spring.start]
或者借助 IDE (比如 IDEA) 的相关功能创建一个 Spring Boot 3 的项目。
你可以自由选择需要添加的任何其他 Spring Boot 组件，比如 `spring-boot-starter-web` 之类的。

<note title="相关参考">

- [Spring Initializr][spring.start]
- [Spring Quickstart Guide](https://spring.io/quickstart/)

</note>

## 安装 starter

在你的 Spring Boot 项目基础上，添加 `simbot-core-spring-boot-starter` 相关依赖。

<tabs group="build">
<tab title="Gradle(Kotlin DSL)" group-key="kts">

```Kotlin
implementation("love.forte.simbot:simbot-core-spring-boot-starter:%version%")
```

</tab>
<tab title="Gradle(Groovy)" group-key="groovy">

```Groovy
implementation 'love.forte.simbot:simbot-core-spring-boot-starter:%version%'
```

</tab>
<tab title="Maven" group-key="maven">

```xml
<dependency>
    <group>love.forte.simbot</group>
    <artifactId>simbot-core-spring-boot-starter</artifactId>
    <version>%version%</version>
</dependency>
```

</tab>
</tabs>

## 修改启动类

假设你的启动类的全限定名称为 `example.demo.MyApplication`, 它是这个样子的：

<tabs group="Code">
<tab title="Kotlin" group-key="Kotlin">

```Kotlin
@SpringBootApplication
class MyApplication

fun main(args: Array<String>) {
    runApplicarion<MyApplication>(*args)
}
```

</tab>
<tab title="Java" group-key="Java">

```Java
@SpringBootApplication
public class MyApplication {
    public static void main(String[] args) {
        SpringApplication.run(MyApplication.class, args);
    }
}
```

</tab>
</tabs>

此时，添加一个注解 `@EnableSimbot` 来启用 simbot 的 Spring Boot starter。

<tabs group="Code">
<tab title="Kotlin" group-key="Kotlin">

```Kotlin
@EnableSimbot // 启用 simbot
@SpringBootApplication
class MyApplication

fun main(args: Array<String>) {
    runApplicarion<MyApplication>(*args)
}
```

</tab>
<tab title="Java" group-key="Java">

```Java
@EnableSimbot // 启用 simbot
@SpringBootApplication
public class MyApplication {
    public static void main(String[] args) {
        SpringApplication.run(MyApplication.class, args);
    }
}
```

</tab>
</tabs>

<warning>

在 Spring Boot 中，你应当确保你的启动类有**一层以上**的包结构。

</warning>

## 编写事件处理器

在 Spring Boot **可扫描到** 的范围内，创建任意类型，并将其标记为 Spring 的 component：

<tabs group="Code">
<tab title="Kotlin" group-key="Kotlin">

```Kotlin
@Component // 交给 Spring 管理
class MyEventHandlers {
    
    @Listener // 标记事件处理函数
    suspend fun handle(event: Event) {
        // 处理事件...
    }
    
    // 更多...
}
```

</tab>
<tab title="Java" group-key="Java">

```Java
@Component // 交给 Spring 管理
public class MyEventHandlers {
    
    @Listener // 标记事件处理函数
    public void handle(Event event) {
        // 处理事件...
    }
    
    // 更多...
}
```

</tab>
</tabs>

## 可配置项

starter 提供了一些可用的配置项。

> 对应的配置类型是 `love.forte.simbot.spring.common.application.SpringApplicationConfigurationProperties`。
> 
> 配置属性的键均以 `simbot` 开头。

> 下面的实例中的配置项的值均为默认值。也就是它们都是可以省略的。

<tabs group="Spring Config">
<tab title="properties" group-key="properties">

```
# simbot.application: 与 Application 相关的配置
simbot.application.application-launch-mode=NONE

# simbot.bots: 与 bot 配置相关的属性。
simbot.bots.auto-registration-failure-policy=ERROR
simbot.bots.auto-registration-mismatch-configurable-bot-manager-policy=ERROR_LOG
simbot.bots.auto-registration-resource-load-failure-policy=ERROR
simbot.bots.auto-start-bots=true
simbot.bots.auto-start-mode=ASYNC
simbot.bots.configuration-json-resources[0]=classpath:simbot-bots/*.bot.json

# simbot.components: 组件相关的配置信息。
simbot.components.auto-install-providers=true
simbot.components.auto-install-provider-configures=true

# simbot.plugins: 插件相关的配置信息。
simbot.plugins.auto-install-providers=true
simbot.plugins.auto-install-provider-configures=true
```

</tab>
<tab title="yaml" group-key="yaml">

```yaml
simbot:
  # simbot.application: 与 Application 相关的配置
  application:
    application-launch-mode: none
  # simbot.bots: 与 bot 配置相关的属性。
  bots:
    auto-registration-failure-policy: ERROR
    auto-registration-mismatch-configurable-bot-manager-policy: ERROR_LOG
    auto-registration-resource-load-failure-policy: ERROR
    auto-start-bots: true
    auto-start-mode: ASYNC
    configuration-json-resources:
      - 'classpath:simbot-bots/*.bot.json'
  # simbot.components: 组件相关的配置信息。
  components:
    auto-install-providers: true
    auto-install-provider-configures: true

  # simbot.plugins: 插件相关的配置信息。
  plugins:
    auto-install-providers: true
    auto-install-provider-configures: true
```

</tab>
</tabs>

<deflist>
<def title="simbot.bots.auto-registration-failure-policy">

`ApplicationLaunchMode` 枚举, 保持 `Application` 活跃的策略。

默认为 `NONE`。

如果你的 Spring Boot 应用中没有可以保持程序活跃的内容
(最常见的例如 `spring-web`), 那么可以选择使用 `THREAD`。

<deflist type="medium">
<def title="NONE">
没有行为。
</def>
<def title="THREAD">
使用一个独立的非守护线程保持程序活跃。
</def>
</deflist>

</def>
<def title="simbot.bots.auto-registration-mismatch-configurable-bot-manager-policy">

`BotConfigResourceLoadFailurePolicy` 枚举, 当加载用于注册bot的配置文件出现错误时的处理策略。

默认为 `ERROR`。

<deflist type="medium">
<def title="ERROR">当出现无法解析的资源文件时抛出异常来尝试中断整个处理流程。</def>
<def title="ERROR_LOG">当出现无法解析的资源文件时输出 <code>error</code> 级别的日志来尝试中断整个处理流程。</def>
<def title="WARN">当出现无法解析的资源文件时会输出 <code>warn</code> 日志。</def>
<def title="IGNORE">当出现无法解析的资源文件时仅会输出 <code>debug</code> 调试日志。</def>
</deflist>

</def>
<def title="simbot.bots.auto-registration-resource-load-failure-policy">

`MismatchConfigurableBotManagerPolicy` 枚举, 当无法为某个 `SerializableBotConfiguration`
找到任何可供其注册的 `BotManager` 时的处理策略。

默认为 `ERROR`。

<deflist type="medium">
<def title="ERROR">
当无法为配置类找到任何可供注册的 <code>BotManager</code> 时抛出异常来尝试中断整个处理流程。
</def>
<def title="ERROR_LOG">
当无法为配置类找到任何可供注册的 <code>BotManager</code> 时输出 <code>error</code> 级别的日志来尝试中断整个处理流程。
</def>
<def title="WARN">
当无法为配置类找到任何可供注册的 <code>BotManager</code> 时会输出 <code>warn</code> 日志。
</def>
<def title="IGNORE">
当无法为配置类找到任何可供注册的 <code>BotManager</code> 时仅会输出 <code>debug</code> 调试日志。
</def>
</deflist>

</def>
<def title="simbot.bots.auto-start-bots">

`Boolean`, 是否在自动扫描的 `Bot` 注册后使用 `Bot.start` 启动它们。

默认为 `true`。

</def>
<def title="simbot.bots.auto-start-mode">

`BotAutoStartMode` 枚举, 当 `auto-start-bots` 为 `true` 时， 批量启动 `Bot` 的方式。

默认为 `ASYNC`。

<deflist type="medium">
<def title="SYNC">
依次同步启动。
</def>
<def title="ASYNC">

每个 `Bot` 独立地异步启动。
如果 `auto-registration-failure-policy` 为 `ERROR`,
则异步中任意 `Bot` 如果启动失败都会导致整体失败。

</def>
</deflist>

</def>
<def title="simbot.bots.configuration-json-resources">

`Set<String>`, 需要加载的所有组件下它们对应的所有 `JSON` 格式 的 `Bot` 配置文件。

默认为 `classpath:simbot-bots/*.bot.json`, 即本项目中、资源目录下的 `resources/simbot-bots`
中的所有扩展名为 `.bot.json` 的配置文件。

</def>
<def title="simbot.components.auto-install-providers">

是否通过 SPI 自动加载所有可寻得的组件。

默认为 `true`。

</def>
<def title="simbot.components.auto-install-provider-configures">

是否在加载 SPI providers 时候也同时加载它们的前置配置。 `auto-install-providers` 为 `true` 时有效。

默认为 `true`。

</def>
<def title="simbot.plugins.auto-install-providers">

是否通过 SPI 自动加载所有可寻得的插件。

</def>
<def title="simbot.plugins.auto-install-provider-configures">

是否在加载 SPI providers 时候也同时加载它们的前置配置。 `auto-install-providers` 为 `true` 时有效。

</def>
</deflist>


## 额外的定制化配置

starter 提供了大量的 `*Configurer` 类型来支持你对启动过程中的各个步骤添加**额外**的自定义配置。

<deflist>
<def title="SimbotApplicationConfigurationConfigurer">

针对 `Application` 配置属性的额外配置器。

</def>
<def title="SimbotDispatcherConfigurer">

针对 Spring 下的事件调度器 `EventDispatcher` 的额外配置器。

</def>
<def title="SimbotComponentInstaller">

`Component` 组件安装器。

</def>
<def title="SimbotPluginInstaller">

`Plugin` 插件安装器。

</def>
<def title="SimbotApplicationLauncherPreConfigurer">

构建 `Application` 过程中, 安插在配置、安装其他内容之前的额外配置器。

</def>
<def title="SimbotApplicationLauncherPostConfigurer">

构建 `Application` 过程中, 安插在配置、安装其他内容之后的额外配置器。

</def>
<def title="SimbotApplicationPreConfigurer">

`Application` 构建完成后, (在一定范围内) 安插在其他配置之前的额外配置器。

</def>
<def title="SimbotApplicationPostConfigurer">

`Application` 构建完成后, (在一定范围内) 安插在其他配置之后的额外配置器。

</def>
<def title="SimbotEventDispatcherPostConfigurer">

`Application` 构建完成后, 针对 `EventDispatcher` 的额外配置器。

</def>
<def title="SimbotEventListenerRegistrarPostConfigurer">

`Application` 构建完成后, 针对 `EventListenerRegistrar` 的额外配置器。

</def>
</deflist>

<tip title="小插曲">

直到我用 `Writerside` 写这章文档才知道, `Configurer` 并不是正经的英语单词... Spring, 你害我不浅！

</tip>

## 安装组件以及组件配置

你可以在上面的配置中看到，组件 `Component` 和 `Plugin` 默认情况下都是通过 SPI **自动加载** 的。

因此一般情况下, 你只需要前往 [**官方维护的组件库**](official-components.md) 选择你想要使用的组件, 或者选择某个/些你想要使用的第三方组件, 
然后将它们添加到你的运行时环境中即可。

如果你想要添加更多定制化的配置或注册额外的组件、插件，可参考实现上面提到的 `SimbotComponentInstaller`、`SimbotPluginInstaller`:

<tabs group="Code">
<tab title="Kotlin" group-key="Kotlin">

```Kotlin
@Component // 交给 Spring 管理
open class MyComponentInstaller: SimbotComponentInstaller {
    override fun install(installer: ComponentInstaller) {
        // 配置或安装，同一个工厂实例的多次配置会被合并
        installer.install(...)
        installer.install(...) { ... }
    }
}

@Component // 交给 Spring 管理
open class MyPluginInstaller: SimbotPluginInstaller {
    override fun install(installer: PluginInstaller) {
        // 配置或安装，同一个工厂实例的多次配置会被合并
        installer.install(...)
        installer.install(...) { ... }
    }
}
```

</tab>
<tab title="Java" group-key="Java">

```Java
@Component // 交给 Spring 管理
public class MyComponentInstaller implements SimbotComponentInstaller {
    @Override
    public void install(@NotNull ComponentInstaller installer) {
        // 配置或安装，同一个工厂实例的多次配置会被合并
        installer.install(...);
        installer.install(..., config -> { ... });
    }
}

@Component // 交给 Spring 管理
public class MyPluginInstaller implements SimbotPluginInstaller {
    @Override
    public void install(@NotNull PluginInstaller installer) {
        // 配置或安装，同一个工厂实例的多次配置会被合并
        installer.install(...);
        installer.install(..., config -> { ... });
    }
}
```

</tab>
</tabs>

或者选择 `SimbotApplicationLauncherPreConfigurer` 和 `SimbotApplicationLauncherPostConfigurer`, 直接添加一个在 Application 构建阶段的配置。

<tabs group="Code">
<tab title="Kotlin" group-key="Kotlin">

```Kotlin
@Component // 交给 Spring 管理
public class MyComponentInstaller implements SimbotApplicationLauncherPreConfigurer {
    @Override
    override fun configure(configurer: ApplicationFactoryConfigurer<SpringApplicationBuilder, SpringApplicationEventRegistrar, SpringEventDispatcherConfiguration>) {
        // 配置或安装，同一个工厂实例的多次配置会被合并
        configurer.install(...)
        installer.install(...) { ... }
    }
}
```

</tab>
<tab title="Java" group-key="Java">

```Java
@Component // 交给 Spring 管理
public class MyComponentInstaller implements SimbotApplicationLauncherPreConfigurer {
    @Override
    public void configure(@NotNull ApplicationFactoryConfigurer<SpringApplicationBuilder, SpringApplicationEventRegistrar, SpringEventDispatcherConfiguration> configurer) {
        // 配置或安装，同一个工厂实例的多次配置会被合并
        configurer.install(...);
        installer.install(..., config -> { ... });
    }
}
```

</tab>
</tabs>

## 运行或打包

你可以参考 Spring Boot 针对不同构建工具的执行、打包或其他任何内容的详细说明手册：
- **Maven**: [Spring Boot Maven Plugin Documentation](https://docs.spring.io/spring-boot/docs/3.2.2/maven-plugin/reference/htmlsingle/)
- **Gradle**: [Spring Boot Gradle Plugin Reference Guide](https://docs.spring.io/spring-boot/docs/3.2.2/gradle-plugin/reference/htmlsingle/)

如果是借助 [start.spring.io][spring.start] 等工具生成的项目, 那么也可以参考项目目录中可能自带的 `HELP.md` 文档中的内容。


[spring.start]: https://start.spring.io