<show-structure for="chapter,procedure" depth="3"/>

# 2. 实现 BotManager

<note>

只建议使用 Kotlin 实现。BotManager 包含 **挂起函数**，无法通过 Java 实现。

</note>

<procedure title="实现 BotManager 的基本步骤">
    <step><a href="component-dev-impl-component.md">实现 <code>Component</code>。</a></step>
    <step><a href="component-dev-impl-bot-and-manager-impl-bot.md">实现 <code>Bot</code>。</a></step>
    <step>实现 <code>BotManager</code>。</step>
    <step>可选地支持 SPI</step>
</procedure>

<procedure title="Component">

因为 `Bot` 需要明确属于一个 `Component`，因此在这里，`Component` 是必须的了。

我们使用在
<a href="component-dev-impl-component.md" /> 
中介绍的 `FooComponent`，后续将不再赘述。

</procedure>

## 实现 BotManager {id="impl-bot-manager"}

实现完 `FooBot`, 接下来我们要实现创建、保存它们的 `FooBotManager` 了。

### 定义 FooBotManager {id="def-foo-bot-manager"}

依照官方组件的习惯，我们会定义一个 `FooBotManager` 抽象类，其中默认实现对配置类的处理。

> 当然，这并不是强制的，你可以依照你认为合适的风格来实现它。

那么首先，让我们定义一个 `FooBotManager` 抽象类：

```Kotlin
abstract class FooBotManager : BotManager, JobBasedBotManager() {

    // 1️⃣
    abstract override val job: Job
    // abstract override suspend fun join()
    // abstract override fun onCompletion(handle: OnCompletion)
    // abstract override fun cancel(cause: Throwable?)
    // abstract override val isActive: Boolean
    // abstract override val isCompleted: Boolean

    // 2️⃣

    override fun configurable(configuration: SerializableBotConfiguration): Boolean {
        TODO("Not yet implemented")
    }

    override fun register(configuration: SerializableBotConfiguration): FooBot {
        TODO("Not yet implemented")
    }
    
    abstract fun register(id: String, password: String): FooBot 

    abstract override fun all(): Sequence<FooBot>

    abstract override fun get(id: ID): FooBot
    
    override fun find(id: ID): FooBot? = try {
        get(id)
    } catch (nb: NoSuchBotException) {
        null
    }
}
```

和 `FooBot` 类似，**1️⃣** 中的部分与协程、任务、任务状态与挂起相关。由于我们继承了 `JobBasedBotManager`, 
因此后续可以直接靠实现 `val job: Job` 来快捷地完成对它们地实现。
**2️⃣** 以下的部分则是需要我们手动实现的。

首先可以看到，我们添加了一个额外的抽象方法：

```Kotlin
abstract fun register(id: String, password: String): FooBot 
```

它便是通过**明确的**参数来构建 bot 的一种 **"专属方法"**。几乎每个 `BotManager` 都会有属于自己的 **"专属方法"** 。

接下来看看 `configurable` 和 `register`，它们需要校验并使用一个 `SerializableBotConfiguration` 的类型。
让我们为 `FooBot` 创建一个。

#### FooBotConfiguration

创建一个 `FooBotConfiguration`, 使其实现 `SerializableBotConfiguration`, 标记为可序列化，并且要求它的序列化名称与 `FooComponent` 的 ID 一致。

```Kotlin
@Serializable
@SerialName(FooComponent.ID_VALUE)
class FooBotConfiguration(
    val id: String, 
    val password: String
) : SerializableBotConfiguration()
```

由于登录bot需要用到 id 和密码，因此我们将这两个的参数作为必要的序列化属性。

接下来，为我们的 `FooBotConfiguration` 配置它的多态序列化信息。这需要在 `FooComponent` 中配置。

```Kotlin
class FooComponent : Component {
    override val id: String get() = ID_VALUE
    override val serializersModule: SerializersModule get() = SerializersModule

    companion object Factory : ComponentFactory<FooComponent, FooComponentConfiguration> {
        const val ID_VALUE: String = "com.example.foo"
        
        @JvmField
        val SerializersModule: SerializersModule = SerializersModule {
            // 👇此处添加 FooBot 配置类的序列化信息
            //   这样可以支持一些自动扫描的环境下（比如 Spring Boot）
            //   可以自动反序列化 bot 的配置文件。
            serializableBotConfigurationPolymorphic {
                subclass(FooBotConfiguration.serializer())
            }
        }

        override val key: ComponentFactory.Key = object : ComponentFactory.Key {}
        
        override fun create(context: ComponentConfigureContext, configurer: ConfigurerFunction<FooComponentConfiguration>): FooComponent {
            省略...
        }
    }
}
```

#### 实现 configurable 和 register {id="impl-configurable-register"}

配置类创建好了，让我们实现 `configurable` 和 `register` 中的校验和实现的逻辑。

```Kotlin
abstract class FooBotManager : BotManager {
    // 其他省略..
    
    /**
     * 判断类型是否为 [FooBotConfiguration]
     */
    override fun configurable(configuration: SerializableBotConfiguration): Boolean =
        configuration is FooBotConfiguration

    /**
     * 如果 [configuration] 类型不是 [FooBotConfiguration] 则抛出 [UnsupportedBotConfigurationException].
     * 否则构建 [FooBot]。
     */
    override fun register(configuration: SerializableBotConfiguration): FooBot {
        val fooBotConfiguration = configuration as? FooBotConfiguration
            ?: throw UnsupportedBotConfigurationException("`configuration` !is FooBotConfiguration")

        return register(fooBotConfiguration.id, fooBotConfiguration.password)
    }
    
    // 其他省略..
}
```

很简单不是吗，判断、校验配置类的类型，然后在符合条件的情况下将注册 bot 的逻辑委托给 `register(id, password)` 即可。

### 实现 FooBotManager {id="impl-foo-bot-manager"}

接下来，我们来实现 `FooBotManager`。我们提到过 `BotManager` 是一个特殊的 `Plugin` 类型，
因此实现它的大致流程与 `Plugin` 是类似的。

#### 实现类 {id="impl-foo-bot-manager-impl"}

先来定义一个 `FooBotManager` 的实现类 `FooBotManagerImpl`。

```Kotlin
class FooBotManagerImpl(
    /**
     * 用于控制 [FooBotManagerImpl] 任务周期的 Job
     */
    override val job: Job,
    /**
     * 为每个 bot 准备的 [CoroutineContext], 其中不包含 Job，
     * 在构建 bot 的时候再添加。
     */
    private val coroutineContext: CoroutineContext,
    /**
     * 构建时获取到的 component，用于创建 bot
     */
    private val component: FooComponent
) : FooBotManager() {
    /**
     * 记录创建过的 bot 和它对应的唯一id。
     *
     * 此处的类型应确保并发安全，例如在 JVM 中使用 `ConcurrentHashMap`。
     */
    private val bots = concurrentMutableMap<String, FooBot>()

    override fun register(id: String, password: String): FooBot {
        // 如果 job 已经被终止，则抛出异常
        job.ensureActive()

        val bot = bots.computeValue(id) { key, old ->
            // 如果已经存在同 id 的 bot，抛出异常
            if (old != null && old.isActive) throw ConflictBotException("id: $key")

            val supervisorJob = SupervisorJob(job)
            FooBotImpl(key.ID, password, component, coroutineContext + supervisorJob)
        }!!

        // 当 bot 被终止后, 尝试移除自身
        bot.onCompletion {
            // 在 JVM 中等同于 bots.remove(id, bot)
            bots.removeValue(id) { bot }
        }

        return bot
    }

    /**
     * 得到当前所有的 bot
     */
    override fun all(): Sequence<FooBot> = bots.values.asSequence()

    /**
     * 根据 id 寻找 bot
     */
    override fun get(id: ID): FooBot {
        return bots[id.literal] ?: throw NoSuchBotException("id: $id")
    }
}
```

#### 实现工厂 {id="impl-foo-bot-manager-factory"}

接下来，像 `Plugin` 那样，为它实现一个工厂。按照习惯，我们会将它通过**伴生对象**实现。

需要注意的是，工厂实现是在 `FooBotManager` 的**伴生对象**中，而不是 `FooBotManagerImpl` 哦！按照习惯，
`FooBotManagerImpl` 应当是被隐藏的。

```Kotlin
abstract class FooBotManager : BotManager, JobBasedBotManager() {

    // 其他内容省略..

    companion object Factory : BotManagerFactory<FooBotManager, FooBotManagerConfiguration> {
        override val key: PluginFactory.Key = object : PluginFactory.Key {}

        override fun create(
            context: PluginConfigureContext,
            configurer: ConfigurerFunction<FooBotManagerConfiguration>
        ): FooBotManager {
            val config = FooBotManagerConfiguration().invokeBy(configurer)
            // 寻找对应的 component
            val component = context.components.find<FooComponent>() ?: throw NoSuchComponentException("FooComponent")

            // 合并 application 中的 coroutineContext 和配置类中的 coroutineContext
            val mergedCoroutineContext = config.coroutineContext.mergeWith(context.applicationConfiguration.coroutineContext)
            // 拿到里面的 job
            val job = mergedCoroutineContext[Job]!!
            // 清理 job
            val coroutineContext = mergedCoroutineContext.minusKey(Job)

            return FooBotManagerImpl(job, coroutineContext, component)
        }
    }
}

/**
 * [FooBotManager] 构建时的配置类。
 * 可以提供一些配置，例如 coroutineContext
 */
class FooBotManagerConfiguration {
    var coroutineContext: CoroutineContext = EmptyCoroutineContext
}
```

## 支持 SPI {id="impl_spi_provide"}

`BotManager` 继承 `Plugin`, 因此支持 SPI 的方式与 `Plugin` 一致。
你可以前往参考之前的 **实现插件** 章节中的
<a href="component-dev-impl-plugin.md#impl_spi_provide" />
部分，只不过把其中描述的 `FooPlugin` 换成 `FooBotManager` 就好了。

> 是否支持 SPI 取决于你的功能需求, 这不是必须的。

