# Bot 和事件的有机结合

当我们拥有了 `EventProcessor`，便可以将其传递。

在 
<a href="component-dev-event-push.md" />
中，我们有介绍如何获取并保存一个 `EventProcessor` 来用于事件推送。

在我们的示例中，我们将它保存在了 `BotManager` 的实现中。
因此，当通过这个 `BotManager` 构建任何 `Bot` 时，便可以将其传递给 `Bot`，使其拥有推送事件的能力。

```Kotlin
class FooBotManager(
    val eventProcessor: EventProcessor,
    val config: FooBotManagerConfiguration
) : BotManager {
    // ... 实现细节省略 ... 

    fun register(...): FooBot {
        // 当注册逻辑中构建Bot时，传递它
        return FooBotImpl(eventProcessor)
    }

    /** 伴生对象实现工厂 */
    companion object Factory : BotManagerFactory<FooBotManager, FooBotManagerConfiguration> {
        // ... 实现细节省略 ...
    }
}

class FooBotImpl(
    val eventProcessor: EventProcessor,
) : FooBot {
    // ... 实现细节省略 ... 

    override suspend fun start() {
        // 我们假设：
        // 这个Bot在启动后，会周期性推送一个事件
        launch {
            while(true) {
                eventProcessor
                    .push(createEvent()) // 推送一个事件
                    .onEachError { result -> 
                        logger.error("ERROR: {}", result, result.content)
                    }
                    .collect()
                    
                // 前后5秒的间隔
                delay(5.seconds)    
            }
        }
        
        // 注意！
        // 此处仅供参考，实际情况需要考虑更多问题
        // 比如重复启动等行为的处理
    }
    
    private fun createEvent(): Event = ...
}
```