# 常见问题


## 包引用异常/找不到注解/无法引用注解 {collapsible="true"}

如果你发现你的代码所有simbot相关的内容都出现了包引用异常，并且它们的特点是：
- 你使用的IDE是 IntelliJ IDEA，而且版本**不是最新**的。
- 你实际正常引入了相关的依赖，也可以在依赖树/库中找到对应的类，换言之警告的类实际上是**存在的**。
- 红色警告**附着在类上**，而不是某个包路径节点上。
- 强行运行的话是**可正常运行**的。

例如图例所示:

![FAQ-包引用异常.png](FAQ-包引用异常.png)

> 图例用的是simbot3时代的截图，但是本质上都是一样的，不用纠结截图中的详细代码。

那么这通常是因为Kotlin插件版本过低而导致类型无法解析。

> 如果你不是在使用 IntelliJ IDEA 的情况下出现上述问题，
> 也可以尝试下列解决方案。如果不行，可以前往 [](feedback-and-support.md)
> 交流或反馈详细情况。

那么你可以：

### 1.更新IDEA

你需要确保你的IDEA的版本不能太落后，至少要在当前最新版本的三个小版本范围内。
举个例子，假如目前IDEA的最新版本为 `2023.1`，那么你现在使用的IDEA至少也得是 `2022.1+`。

<tip title="仅是猜测">

**三个小版本**范围的说法其实只是凭感觉，无依据，不绝对。总而言之，IDEA的版本**越新越好**。

</tip>

### 2.禁用Kotlin插件

如果你真的不想更新IDEA，那么你可以尝试禁用IDEA的 `Kotlin` 插件，
如果你不打算也没有使用Kotlin的话。只要你编写的语言不是 `Kotlin`，那么禁用掉此插件也是可以使用simbot的。

> 但是这样也可能使得一些由 Kotlin 而来的异常/警告检测失效，
> 比如尝试在 Java 中引用 `internal` 级别的类型。

## 如何主动获取Bot {collapsible="true"}

在非事件的情况下（例如一个定时任务或初始化任务），你可以通过构建的 `Application` 获取到 `BotManagers`，
而又可以通过各个 `BotManager` 进一步获取到你想要的 `Bot`。

<tabs group="code">
<tab title="Kotlin" group-key="Kotlin">

```Kotlin
val application: Application = ... // 你的 Application
// 可以得到 BotManagers
val botManagers = application.botManagers

// 你可以选择直接使用 allBots()、firstBot() 等辅助API来遍历或快捷获取某种bot
botManagers.allBots().forEach { bot -> ... }

// 也可以自行遍历，精准选择你所需要的bot
for (botManager in botManagers) {
    // 假设这里你想要寻找QQ机器人的BotManager
    if (botManager is QQGuildBotManager) {
        // 找到指定 appid 的bot，
        // 然后你可以保存、操作，或者做点儿什么。
        val bot = botManager[123456789.ID]
        
        // 你已经找到了你需要的bot，结束循环
        break
    }
}
```

</tab>
<tab title="Java" group-key="Java">

```Java
var application = ...; // 你的 Application
// 可以得到 BotManagers
var botManagers = application.getBotManagers();
// 你可以选择直接使用 allBots()、firstBot() 等辅助API来遍历或快捷获取某种bot
botManagers.allBots().iterator().forEachRemaining(bot -> { ... });

// 也可以自行遍历，精准选择你所需要的bot
for (var botManager : botManagers) {
    // 假设这里你想要寻找QQ机器人的BotManager
    if (botManager instanceof QQGuildBotManager qgBotManager) {
        // 找到指定 appid 的bot，
        // 然后你可以保存、操作，或者做点儿什么。
        var bot = qgBotManager.get(Identifies.of(123456789));

        // 你已经找到了你需要的bot，结束循环
        break;
    }
}
```

</tab>
</tabs>

在集成 Spring 的情况下获取 `Application`，你可以参考: [](Spring-Boot.md#get-bot) 。

## ClassNotFoundException: kotlin.enums.EnumEntriesKt {collapsible="true"}

<tip title="相关参考">
<list>
<li><a href="https://github.com/orgs/simple-robot/discussions/183">Discussions#183</a></li>
</list>
</tip>

出现此问题，你大概率在通过 Maven 工具使用 Spring Boot，且版本大概率是 2.x 。
在Maven中，Spring Boot 的 
`spring-boot-parent`
会通过 `properties` 标签中的 `kotlin.version` 控制一个 Kotlin 的版本，
有关这种版本管理的说明，你可以参考Spring官方文档的这些内容：

<list>
<li>
<a href="https://docs.spring.io/spring-boot/3.3.3/reference/features/kotlin.html#features.kotlin.dependency-management">
Spring Boot 3.3+ 文档: <b>Kotlin Support / Dependency Management</b>
</a>
</li>
<li>
<a href="https://docs.spring.io/spring-boot/docs/3.2.9/reference/html/dependency-versions.html#appendix.dependency-versions.properties">
Spring Boot 3.2及以下 (包括 2.x) 文档: <b>Dependency Versions / Version Properties</b>
</a>

(有需要可自行更改链接路径中的版本号)
</li>
</list>

因此，在Maven中如果想要修复此问题，使用 `properties` 为属性 `kotlin.version` 指定一个较高的 Kotlin 版本即可。

<note>
你可以指定为当前simbot版本 
<control>%version%</control> 
所使用的 Kotlin 版本
<control>%kt-version%</control> 或更高版本。
</note>

```xml
<properties>
    <kotlin.version>%kt-version%</kotlin.version>
</properties>
```

如果你的情况与上述不同，可前往
[讨论区](https://github.com/orgs/simple-robot/discussions)
或
[issues](https://github.com/simple-robot/simpler-robot/issues)
反馈详情。