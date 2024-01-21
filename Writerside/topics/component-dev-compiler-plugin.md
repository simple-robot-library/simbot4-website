# 编译器插件

如果你使用 `Kotlin` + `Gradle` 开发组件（这也是我们推荐的组合）, 那么当你公开了一个挂起函数的 API、
或实现一个具有挂起函数的 simbot4 标准 API (例如 `Bot`、**事件** 等) 时，
你极大概率（我们也强烈推荐）你需要使用到标准库在使用的编译器插件: 
[kotlin-suspend-transform-compiler-plugin][kstcp]。

<warning title="K2编译器">

[kotlin-suspend-transform-compiler-plugin][kstcp] 暂不支持 **K2** 编译器。
但是 simbot4 的目标是基于 Kotlin 2.0 进行构建, 因此 **K2** 的支持是早晚需要实现的。

此插件有关 **K2**的信息可追踪
[#44](https://github.com/ForteScarlet/kotlin-suspend-transform-compiler-plugin/issues/44),
并如果可以的话希望能为其贡献一份力量！🙏

</warning>

此编译器插件的目的是根据挂起函数生成对 Java 或其他特定平台友好的非挂起函数。

## 安装

### 安装编译器插件

<note>

你可以前往 [kotlin-suspend-transform-compiler-plugin][kstcp] 了解全部的配置方式。

</note>

添加插件 [`love.forte.plugin.suspend-transform`](https://plugins.gradle.org/plugin/love.forte.plugin.suspend-transform)。

```Kotlin
plugins {
    id("love.forte.plugin.suspend-transform") version "%kstcpVersion%"
}

// 编译器插件的配置会在下文介绍
```

### 安装 simbot 编译器插件辅助库

在你 Gradle 项目的 `buildSrc/build.gradle(.kts)` 中添加 simbot4 提供的辅助模块: `simbot-gradle-suspendtransforms`。

> 暂时没有将它直接作为 Gradle 插件, 因此目前只能在 `buildSrc` 中添加。未来也许会直接作为一个 Gradle Plugin 提供。

```Kotlin
implementation("love.forte.simbot.gradle:simbot-gradle-suspendtransforms:%version%")
```

### 配置编译器插件

```Kotlin
// 配置编译器插件
suspendTransform {
    // runtime 和 注解都由 simbot 标准库定制，不需要添加
    includeRuntime = false
    includeAnnotation = false
    
    // SuspendTransforms 以及它的这些信息都是辅助模块 `simbot-gradle-suspendtransforms` 提供的。
    addJvmTransformers(
        // @JvmBlocking
        SuspendTransforms.jvmBlockingTransformer,
        // @JvmAsync
        SuspendTransforms.jvmAsyncTransformer,

        // @JvmSuspendTrans
        SuspendTransforms.suspendTransTransformerForJvmBlocking,
        SuspendTransforms.suspendTransTransformerForJvmAsync,
        SuspendTransforms.suspendTransTransformerForJvmReserve,

        // @JvmSuspendTransProperty
        SuspendTransforms.jvmSuspendTransPropTransformerForBlocking,
        SuspendTransforms.jvmSuspendTransPropTransformerForAsync,
        SuspendTransforms.jvmSuspendTransPropTransformerForReserve,
    )
}
```

## 使用

### 标记注解
目前, 编译器插件的标记注解有两个：

- `@SuspendTrans`
- `@SuspendTransProperty`

并且它们各自都有一个用于简写的 `typealias`:

- `@ST`
- `@STP`

它们通常作用在类型上或挂起函数上。

> 它似乎暂时不支持 **顶层函数**。

举个例子，如下一个接口, 在编译后会产生几个新的非挂起桥接函数：

<compare type="top-bottom" first-title="编译前" second-title="编译后">

```Kotlin
class Data

interface Foo {
    @ST
    suspend fun run(): Data 
}
```

```Kotlin
class Data

interface Foo {
    @ST
    @JvmSynthetic // 会自动附加此注解来面向 Java 隐藏
    suspend fun run(): Data // 原本的函数
    
    // 生成的阻塞API
    fun runBlocking(): Data { ... } 
    
    // 生成的异步API，返回值结果是 CompletableFuture
    fun runAsync(): CompletableFuture<out Data> { ... } 
    
    // 生成的预处理API，返回值结果是 SuspendReserve, 可以用来做进一步转化。
    fun runReserve(): SuspendReserve<Data> { ... } 
}
```

</compare>

生成的桥接函数最终逻辑都仍然是被标记的那个挂起函数。

`@STP` 与 `@ST` 的区别在于，前者只能标记在**没有参数**的挂起函数上，并且生成的桥接"函数"实际上是属性类型。
还是上面的例子，我们稍作修改, 在编译后会产生几个新的非挂起桥接"属性"：

<compare type="top-bottom" first-title="编译前" second-title="编译后">

```Kotlin
class Data

interface Foo {
    @STP // 以属性的形式生成桥接函数
    suspend fun value(): Data 
}
```

```Kotlin
class Data

interface Foo {
    @STP
    @JvmSynthetic // 会自动附加此注解来面向 Java 隐藏
    suspend fun value(): Data // 原本的函数
    
    // 生成的阻塞API，使用 @STP 的时候，阻塞API的结果默认没有 "Blocking" 后缀。
    val value: Data get() { ... } 
    
    // 生成的异步API，返回值结果是 CompletableFuture
    val valueAsync: CompletableFuture<out Data> get() { ... } 
    
    // 生成的预处理API，返回值结果是 SuspendReserve, 可以用来做进一步转化。
    val valueReserve: SuspendReserve<Data> get() { ... } 
}
```

</compare>

这样在 Java 中，使用者便可以以 Getter API 的风格去使用它们了。

标记注解都存在一些参数，可以定制生成的API的函数/属性名。

<compare type="top-bottom" first-title="编译前" second-title="编译后">

```Kotlin
interface Foo {
    @ST(blockingBaseName = "getValue", blockingSuffix = "")
    // 生成的阻塞函数的基础名称是 'getValue', 并且去除了后缀名 "Blocking". 
    // 其他的 API (例如异步API) 不受影响
    suspend fun value(): Data 
}
```

```Kotlin
interface Foo {
    @ST(...)
    @JvmSynthetic // 会自动附加此注解来面向 Java 隐藏
    suspend fun value(): Data // 原本的函数
    
    // 生成的阻塞API, 名称受到了注解参数的影响而发生变化
    fun getValue(): Data { ... } 
    
    // 异步API和预处理API
    fun valueAsync(): CompletableFuture<out Data> { ... } 
    fun valueReserve(): SuspendReserve<Data> { ... } 
}
```

</compare>



<warning>
当使用编译器插件标记注解的参数调整了生成的桥接函数的签名后,
它的所有实现/继承者, 如果也需要添加标记注解, 那么参数请尽可能保证是
<control>相同的</control>, 否则会产生多套桥接函数, 造成预期外的影响。
</warning>


### 实现挂起函数时

当你实现 `Event`、`Bot` 等 simbot4 标准库中具有挂起函数的类型时，它们基本上都已经通过此插件做了转化。

你需要观察：

Ⅰ: 如果继承的函数的返回值对于你来说 **没有** 进一步扩展

那么你可以直接将其标记 `@JvmSynthetic`: 

```Kotlin
interface SourceType { // 假设这是个你需要去实现的某个类型 
    @ST // 它标记了一个编译器插件的注解，说明此函数会生成对应的非挂起函数
    suspend fun run(): Int // 返回值是 Int, 它没有(也不可能)有进一步扩展类型
}

class MySourceType : SourceType { // 假设这是你提供的实现类
    @JvmSynthetic // 只使用 @JvmSynthetic: 因为你不需要生成配套的非挂起函数来覆盖
    override suspend fun run(): Int {
        return ...
    }
}
```

Ⅱ: 继承的函数的返回值对于你来说 **有进一步扩展**

那么你需要标记一个与被继承函数**相同**的编译器插件标记注解。

举个例子:

假设你需要实现的:

```Kotlin
open class Data

interface SourceType { // 假设这是个你需要去实现的某个类型 
    @ST(blockingBaseName = "getValue") 
    // 它标记了一个编译器插件的注解 @ST，说明此函数会生成对应的非挂起函数, 并且有 blockingBaseName 参数
    suspend fun value(): Data // 返回值是 Data, 你扩展了它
}
```

假设你的实现:

```Kotlin
class MyData : Data() // 你对 Data 类型的扩展实现

class MeSourceType : SourceType { // 假设这是你提供的实现类
    @ST(blockingBaseName = "getValue") 
    // 你必须标记与被继承函数 完全相同 的标记注解，包括它的参数
    override suspend fun value(): MyData { // 这里修改返回值, 让它返回你的进一步扩展的类型
        return ...
    }
}
```

在 simbot4 标准 API 中，大部分被标记的挂起函数都没有参数，因此注意它标记的注解即可，`@ST` 或 `@STP` 中的一个。
注解有可能会被标记在类上而不是方法上，可以注意观察。

但也并非没有具有特殊参数的API，目前，它们分布在如下地方：

- `GuildRelation.guild(...)`
- `GroupRelation.group(...)`
- `ContactRelation.contact(...)`
- `Organization.member(...)`
- `Guild.channel(...)`
- `Guild.chatChannel(...)`

它们主要就是将参数里的各个 `xxxBaseName` 重新指定为了 Getter 风格的 `getXxx`, 例如 `GuildRelation.guild(...)`:

```Kotlin
public interface GuildRelation {
    @ST(blockingBaseName = "getGuild", blockingSuffix = "", asyncBaseName = "getGuild", reserveBaseName = "getGuild")
    // 将 baseName 都重新指定为了 getGuild, 并且移除了阻塞API的后缀。
    public suspend fun guild(id: ID): Guild?

    public val guilds: Collectable<Guild>

    @STP
    public suspend fun guildCount(): Int
}
```

之所以这么做，是因为这些API实际上都比较符合 Getter 的风格，但是因为它们都有参数，而不能直接使用 `@STP`, 
所以便使用这种方式手动配置来生成 Java 中更为友好也更符合习惯的 API。

<note>
你在实现各类挂起函数的时候，也可以以此方式为基础提供 Java 友好的 API。
</note>


[kstcp]: https://github.com/ForteScarlet/kotlin-suspend-transform-compiler-plugin