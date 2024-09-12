<tabs id="simbot-core-build" group="build">
<tab title="Gradle(Kotlin DSL)" group-key="kts">

```Kotlin
implementation("love.forte.simbot:simbot-core:%version%")
```

如果使用 Java 而不配合使用 Gradle 的 [Kotlin 插件](https://kotlinlang.org/docs/gradle-configure-project.html#apply-the-plugin),
那么你需要指定依赖的后缀为 `-jvm`。

```Kotlin
implementation("love.forte.simbot:simbot-core-jvm:%version%")
```

</tab>
<tab title="Gradle(Groovy)" group-key="groovy">

```Groovy
implementation 'love.forte.simbot:simbot-core:%version%'
```

如果使用 Java 而不配合使用 Gradle 的 [Kotlin 插件](https://kotlinlang.org/docs/gradle-configure-project.html#apply-the-plugin), 
那么你需要指定依赖的后缀为 `-jvm`。

```Groovy
implementation 'love.forte.simbot:simbot-core-jvm:%version%'
```

</tab>
<tab title="Maven" group-key="maven">

```xml
<dependency>
    <groupId>love.forte.simbot</groupId>
    <artifactId>simbot-core-jvm</artifactId>
    <version>%version%</version>
</dependency>
```

</tab>
</tabs>
<tabs id="simbot-spring-3-build" group="build">
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
    <groupId>love.forte.simbot</groupId>
    <artifactId>simbot-core-spring-boot-starter</artifactId>
    <version>%version%</version>
</dependency>
```

</tab>
</tabs>
<tabs id="simbot-spring-2-build" group="build">
<tab title="Gradle(Kotlin DSL)" group-key="kts">

```Kotlin
implementation("love.forte.simbot:simbot-core-spring-boot-starter-v2:%version%")
```

</tab>
<tab title="Gradle(Groovy)" group-key="groovy">

```Groovy
implementation 'love.forte.simbot:simbot-core-spring-boot-starter-v2:%version%'
```

</tab>
<tab title="Maven" group-key="maven">

```xml
<dependency>
    <groupId>love.forte.simbot</groupId>
    <artifactId>simbot-core-spring-boot-starter-v2</artifactId>
    <version>%version%</version>
</dependency>
```

</tab>
</tabs>





<snippet id="pre-component-install">
<note>
在安装组件库之前，确保你已经安装了可用的核心库实现，比如 
<a href="start-use-core.md">核心库</a>
或 
<a href="Spring-Boot.md">Spring Boot starter</a>
。
</note>

<tabs>
<tab title="核心库">
<include from="refers.md" element-id="simbot-core-build"></include>

<tip>
前往
<a href="start-use-core.md">核心库</a>
了解更多内容。
</tip>

</tab>
<tab title="Spring Boot starter">
<include from="refers.md" element-id="simbot-spring-3-build"></include>

<tip>
前往
<a href="Spring-Boot.md">集成Spring Boot</a>
了解更多内容。
</tip>
</tab>
</tabs>


</snippet>

<snippet id="engine-choose">

你可以前往 [Ktor文档](https://ktor.io/docs/http-client-engines.html)
处选择一个对应所用平台下合适的 `Client Engine`。
这里会根据不同平台提供几个示例，你可以选择其他可用目标。

> 注意应选择使用 `v2.x` 版本的 Ktor。

<tabs group="Platform">
<tab title="JVM" group-key="JVM">

<tabs>
<tab title="Java">

在 Java11+ 的环境下，可以选择 [Java](https://ktor.io/docs/http-client-engines.html#java) 引擎。

<tabs group="build">
<tab title="Gradle Kotlin DSL" group-key="kts">

```kotlin
runtimeOnly("io.ktor:ktor-client-java:$ktor_version")
```

如果不使用Kotlin的[Gradle插件](https://kotlinlang.org/docs/gradle-configure-project.html):

```kotlin
runtimeOnly("io.ktor:ktor-client-java-jvm:$ktor_version")
```

</tab>
<tab title="Gradle Groovy" group-key="groovy">

```groovy
runtimeOnly 'io.ktor:ktor-client-java:$ktor_version'
```

如果不使用Kotlin的[Gradle插件](https://kotlinlang.org/docs/gradle-configure-project.html):

```groovy
runtimeOnly 'io.ktor:ktor-client-java-jvm:$ktor_version'
```

<br />

</tab>
<tab title="Maven" group-key="maven">

```xml
<dependency>
    <groupId>io.ktor</groupId>
    <artifactId>ktor-client-java-jvm</artifactId>
    <version>${ktor_version}</version>
    <scope>runtime</scope>
</dependency>
```

<br />

</tab>
</tabs>

</tab>
<tab title="OkHttp">

[OkHttp](https://ktor.io/docs/client-engines.html#okhttp)
引擎基于 OkHttp，是一个不错的库。

<tabs group="build">
<tab title="Gradle Kotlin DSL" group-key="kts">

```kotlin
runtimeOnly("io.ktor:ktor-client-okhttp:$ktor_version")
```

如果不使用Kotlin的[Gradle插件](https://kotlinlang.org/docs/gradle-configure-project.html):

```kotlin
runtimeOnly("io.ktor:ktor-client-okhttp-jvm:$ktor_version")
```

</tab>
<tab title="Gradle Groovy" group-key="groovy">

```groovy
runtimeOnly 'io.ktor:ktor-client-okhttp:$ktor_version'
```

如果不使用Kotlin的[Gradle插件](https://kotlinlang.org/docs/gradle-configure-project.html):

```groovy
runtimeOnly 'io.ktor:ktor-client-okhttp-jvm:$ktor_version'
```

</tab>
<tab title="Maven" group-key="maven">

```xml
<dependency>
    <groupId>io.ktor</groupId>
    <artifactId>ktor-client-okhttp-jvm</artifactId>
    <version>${ktor_version}</version>
    <scope>runtime</scope>
</dependency>
```

</tab>
</tabs>

</tab>
<tab title="CIO">

[CIO](https://ktor.io/docs/http-client-engines.html#cio) 是一个比较通用的引擎。
在不知道选什么的情况下，可以考虑使用它。

<tabs group="build">
<tab title="Gradle Kotlin DSL" group-key="kts">

```kotlin
runtimeOnly("io.ktor:ktor-client-cio:$ktor_version")
```

如果不使用Kotlin的[Gradle插件](https://kotlinlang.org/docs/gradle-configure-project.html):

```kotlin
runtimeOnly("io.ktor:ktor-client-cio-jvm:$ktor_version")
```

</tab>
<tab title="Gradle Groovy" group-key="groovy">

```groovy
runtimeOnly 'io.ktor:ktor-client-cio:$ktor_version'
```


如果不使用Kotlin的[Gradle插件](https://kotlinlang.org/docs/gradle-configure-project.html):

```groovy
runtimeOnly 'io.ktor:ktor-client-cio-jvm:$ktor_version'
```

</tab>
<tab title="Maven" group-key="maven">

```xml
<dependency>
    <groupId>io.ktor</groupId>
    <artifactId>ktor-client-cio-jvm</artifactId>
    <version>${ktor_version}</version>
    <scope>runtime</scope>
</dependency>
```

</tab>
</tabs>

</tab>
</tabs>

> 引擎大部分情况下可以 runtime only，除非你需要显式地定制化它。

</tab>

<tab title="JavaScript" group-key="JS">
<br />

JavaScript 平台下可以选择 [Js](https://ktor.io/docs/http-client-engines.html#js) 引擎。

<tabs group="build">
<tab title="Gradle Kotlin DSL" group-key="kts">

```kotlin
implementation("io.ktor:ktor-client-js:$ktor_version")
```

</tab>
<tab title="Gradle Groovy" group-key="groovy">

```groovy
implementation 'io.ktor:ktor-client-js:$ktor_version'
```

</tab>
</tabs>

</tab>

<tab title="Native" group-key="Native">
<br />

native 平台目标下，可能需要根据不同的平台类型选择不同的引擎。

<tabs group="NativePlatform">
<tab title="Mingw">
<br />

可以选择 [WinHttp](https://ktor.io/docs/http-client-engines.html#winhttp) 引擎。

<tabs group="build">
<tab title="Gradle Kotlin DSL" group-key="kts">

```kotlin
implementation("io.ktor:ktor-client-winhttp:$ktor_version")
```

</tab>
<tab title="Gradle Groovy" group-key="groovy">

```groovy
implementation 'io.ktor:ktor-client-winhttp:$ktor_version'
```

</tab>
</tabs>

</tab>
<tab title="Linux">
<br />

Linux 下依旧可以选择 [CIO](https://ktor.io/docs/http-client-engines.html#cio) 引擎。

<tabs group="build">
<tab title="Gradle Kotlin DSL" group-key="kts">

```kotlin
implementation("io.ktor:ktor-client-cio:$ktor_version")
```

</tab>
<tab title="Gradle Groovy" group-key="groovy">

```groovy
implementation 'io.ktor:ktor-client-cio:$ktor_version'
```

</tab>
</tabs>

</tab>
<tab title="MacOS">
<br />

可以选择 [Darwin](https://ktor.io/docs/http-client-engines.html#darwin) 引擎。

<tabs group="build">
<tab title="Gradle Kotlin DSL" group-key="kts">

```kotlin
implementation("io.ktor:ktor-client-darwin:$ktor_version")
```

</tab>
<tab title="Gradle Groovy" group-key="groovy">

```groovy
implementation 'io.ktor:ktor-client-darwin:$ktor_version'
```

</tab>
</tabs>

</tab>
</tabs>

</tab>
</tabs>

</snippet>

<snippet id="qg-stdlib-will-be-removed">

<warning>

标准库模块可能会在未来被**弃用并移除**。
前往议题 [#168](https://github.com/simple-robot/simbot-component-qq-guild/issues/168) 了解更多。

</warning>

</snippet>


<warning title="👷文档待施工或施工中 🚧 " id="doc-TODO">

如有需要或疑问，
可前往 
[](communities.md) 与我们取得联系或通过
[](feedback-and-support.md) 中的途径进行问题反馈～

</warning>