# 上手 Kotlin

尽管 Simple Robot 是一个努力保证 Java 友好的框架，但是无论如何，它首先是使用 **Kotlin** 开发的。
那么，直接使用 Kotlin 进行开发的体验终究会比使用 Java 要更好。

因此我们在此建议，**优先使用 Kotlin 来进行 Simple Robot 项目开发**。

得益于 Kotlin 的 **Java 兼容性**，你可以在 Kotlin 中使用几乎所有的 Java 库，
例如 Spring。

## 何谓 Kotlin {id='what-is-kotlin'}

引用[Kotlin 官网][KT HOME]中的[介绍](https://kotlinlang.org/docs/getting-started.html):

> Kotlin is a modern but already mature programming language designed to make developers happier. 
> It's concise, safe, interoperable with Java and other languages, 
> and provides many ways to reuse code between multiple platforms for productive programming.
> {style='note'}

引用[Kotlin 中文站][KT CN BOOK]中的[介绍](https://kotlinlang.org/docs/getting-started.html):

> Kotlin 是一门现代但已成熟的编程语言，旨在让开发人员更幸福快乐。 
> 它简洁、安全、可与 Java 及其他语言互操作，并提供了多种方式在多个平台间复用代码，以实现高效编程。
> {style='note'}

引用 [灰蓝天际博客](https://hltj.me/)[^hltj] 中的[介绍](https://hltj.me/kotlin/2017/05/15/kotlin-reference-translated.html):

> Kotlin 是一门支持多范式、多平台的现代静态编程语言。Kotlin 支持面向对象、泛型与函数式等编程范式,
> 它支持 JVM、Android、JavaScript 目标平台 (以及 native 平台) ...
> 
> 而且 Kotlin 具有很多现代（也有称下一代的）静态语言特性：如类型推断、多范式支持、可空性表达、扩展函数、模式匹配等。...
> 
> 另外 100% 的 Java 互操作性，使 Kotlin 能够与既有工具/框架如 Dagger、Spring、Vert.x 等集成，
> 也能让既有的基于 Java 的服务端与 Android 项目逐步迁移到 Kotlin。
> {style='note'}

[^hltj]: 灰蓝天际, GitHub 为 [gltj](https://github.com/hltj), [Kotlin 中文站][KT CN BOOK]和Ktor 中文站的主要维护者与翻译者(来自 GitHub 的自我介绍)。


## 上手 Kotlin {id='use-kotlin'}

**官网、文档与资料**

- [Kotlin 官网][KT HOME]
- [Kotlin 官方文档](https://kotlinlang.org/docs/) (也可在官方文档右上角进入)
- [Kotlin 中文站][KT CN BOOK]
  
**学习与实操**

<list>
<li>
<p><a href="https://play.kotlinlang.org/koans/">Koans</a>:
a series of exercises to get you familiar with the Kotlin syntax and some idioms.
</p>
<p>(Koans: 让你熟悉 Kotlin 语法和关键词的一系列练习)</p>
</li>
<li>
<p><a href="https://play.kotlinlang.org/byExample/">Learn Kotlin by Example</a>:
An official set of small and simple annotated examples designed for those new to Kotlin.</p>
<p>No prior knowledge of any programming language is required.</p>
<p>(一套为 Kotlin 新手设计的官方小而简单的注释示例。无需任何编程语言知识。)</p>
</li>
</list>



## 快速浏览 {id='quick-view'}

> 大部分内容来自/修改自 [Kotlin 官网][KT HOME]。

<tabs>
<tab title="类型定义">

```Kotlin
// 普通类
class SimpleClass

// 抽象类
abstract class SimpleAbstractClass

// 接口
interface SimpleInterface

// 数据类(例如 POJO, DTO 等, 实现 toString, hashCode, equals)
data class SimpleDataClass(val name: String)

// 单例对象
object SimpleObject

// 顶层函数
fun topFun() {
}
```



</tab>
<tab title="面向对象">

```Kotlin
// 抽象类
abstract class Person(val name: String) {
    abstract fun greet()
}

// 接口
interface FoodConsumer {
    fun eat()
    fun pay(amount: Int) = println("Delicious! Here's $amount bucks!")
}

// 普通 final 类
class RestaurantCustomer(name: String, val dish: String) : Person(name), FoodConsumer {
    fun order() = println("$dish, please!")
    override fun eat() = println("*Eats $dish*")
    override fun greet() = println("It's me, $name.")
}
```



</tab>
<tab title="简单的main">

```Kotlin
fun main() {
    val name = "stranger"        // Declare your first variable
    println("Hi, $name!")        // ...and use it!
    print("Current count:")
    for (i in 0..10) {           // Loop over a range from 0 to 10
        print(" $i")
    }
}
```

</tab>
<tab title="异步">

```Kotlin
import kotlinx.coroutines.*

suspend fun main() {                                // 一个可以挂起与恢复的 '挂起' 函数
    val start = System.currentTimeMillis()
    coroutineScope {                                // 创建一个作用域, 用来启动协程
        for (i in 1..10) {
            launch {                                // 启动 10 个并行任务
                delay(3000L - i * 300)              // 延迟/等待
                log(start, "Countdown: $i")
            }
        }
    }
    // 上面的作用域内所有异步任务结束后，执行
    log(start, "Liftoff!")
}

fun log(start: Long, msg: String) {
    println("$msg " +
            "(on ${Thread.currentThread().name}) " +
            "after ${(System.currentTimeMillis() - start)/1000F}s")
}
```

</tab>
<tab title="Lambda">

```Kotlin
fun main() {
    // 调用函数，传入 Lambda
    run { // 大括号内即为入参的 Lambda
        println("Hello, World！")
    }
}

// 顶层函数，参数是一个 Lambda
fun run(func: () -> Unit) { // 直接使用箭头表达式定义 Lambda 函数结构
    // 执行这个 lambda 体
    func()
}
```



</tab>

</tabs>


[KT HOME]: https://kotlinlang.org
[KT CN BOOK]: https://book.kotlincn.net/