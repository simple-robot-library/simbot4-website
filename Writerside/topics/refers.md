<tabs id="simbot-core-build" group="build">
<tab title="Gradle(Kotlin DSL)" group-key="kts">

```Kotlin
implementation("love.forte.simbot:simbot-core:%version%")
```

如果使用 Java 而不配合使用 Gradle 的 `kotlin` 插件, 那么你需要指定依赖的后缀为 `-jvm`。

```Kotlin
implementation("love.forte.simbot:simbot-core-jvm:%version%")
```

</tab>
<tab title="Gradle(Groovy)" group-key="groovy">

```Groovy
implementation 'love.forte.simbot:simbot-core:%version%'
```

如果使用 Java 而不配合使用 Gradle 的 `kotlin` 插件, 那么你需要指定依赖的后缀为 `-jvm`。

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

<tabs id="qg-build" group="build">
<tab title="Gradle(Kotlin DSL)" group-key="kts">

```Kotlin
implementation("love.forte.simbot.component:simbot-component-qq-guild-core:$VERSION")
```

如果使用 Java 而不配合使用 Gradle 的 `kotlin` 插件, 那么你需要指定依赖的后缀为 `-jvm`。

```Kotlin
implementation("love.forte.simbot.component:simbot-component-qq-guild-core-jvm:$VERSION")
```

</tab>
<tab title="Gradle(Groovy)" group-key="groovy">

```Groovy
implementation 'love.forte.simbot.component:simbot-component-qq-guild-core:$VERSION'
```

如果使用 Java 而不配合使用 Gradle 的 `kotlin` 插件, 那么你需要指定依赖的后缀为 `-jvm`。

```Groovy
implementation 'love.forte.simbot.component:simbot-component-qq-guild-core-jvm:$VERSION'
```

</tab>
<tab title="Maven" group-key="maven">

```xml
<dependency>
    <groupId>love.forte.simbot.component</groupId>
    <artifactId>simbot-component-qq-guild-core-jvm</artifactId>
    <version>${VERSION}</version>
</dependency>
```

</tab>
</tabs>
<tabs id="kook-build" group="build">
<tab title="Gradle(Kotlin DSL)" group-key="kts">

```Kotlin
implementation("love.forte.simbot.component:simbot-component-kook-core:$VERSION")
```

如果使用 Java 而不配合使用 Gradle 的 `kotlin` 插件, 那么你需要指定依赖的后缀为 `-jvm`。

```Kotlin
implementation("love.forte.simbot.component:simbot-component-kook-core-jvm:$VERSION")
```

</tab>
<tab title="Gradle(Groovy)" group-key="groovy">

```Groovy
implementation 'love.forte.simbot.component:simbot-component-kook-core:$VERSION'
```

如果使用 Java 而不配合使用 Gradle 的 `kotlin` 插件, 那么你需要指定依赖的后缀为 `-jvm`。

```Groovy
implementation 'love.forte.simbot.component:simbot-component-kook-core-jvm:$VERSION'
```

</tab>
<tab title="Maven" group-key="maven">

```xml
<dependency>
    <groupId>love.forte.simbot.component</groupId>
    <artifactId>simbot-component-kook-core-jvm</artifactId>
    <version>${VERSION}</version>
</dependency>
```

</tab>
</tabs>
<tabs id="onebot-build">
<tab title="OneBot11">
<tabs group="build">
<tab title="Gradle(Kotlin DSL)" group-key="kts">

```Kotlin
implementation("love.forte.simbot.component:simbot-component-onebot-v11-core:$VERSION")
```

如果使用 Java 而不配合使用 Gradle 的 `kotlin` 插件, 那么你需要指定依赖的后缀为 `-jvm`。

```Kotlin
implementation("love.forte.simbot.component:simbot-component-onebot-v11-core-jvm:$VERSION")
```

</tab>
<tab title="Gradle(Groovy)" group-key="groovy">

```Groovy
implementation 'love.forte.simbot.component:simbot-component-onebot-v11-core:$VERSION'
```

如果使用 Java 而不配合使用 Gradle 的 `kotlin` 插件, 那么你需要指定依赖的后缀为 `-jvm`。

```Groovy
implementation 'love.forte.simbot.component:simbot-component-onebot-v11-core-jvm:$VERSION'
```

</tab>
<tab title="Maven" group-key="maven">

```xml
<dependency>
    <groupId>love.forte.simbot.component</groupId>
    <artifactId>simbot-component-onebot-v11-core-jvm</artifactId>
    <version>${VERSION}</version>
</dependency>
```

</tab>
</tabs>
</tab>
</tabs>
<tabs id="tg-build" group="build">
<tab title="Gradle(Kotlin DSL)" group-key="kts">

```Kotlin
implementation("love.forte.simbot.component:simbot-component-telegram-core:$VERSION")
```

如果使用 Java 而不配合使用 Gradle 的 `kotlin` 插件, 那么你需要指定依赖的后缀为 `-jvm`。

```Kotlin
implementation("love.forte.simbot.component:simbot-component-telegram-core-jvm:$VERSION")
```

</tab>
<tab title="Gradle(Groovy)" group-key="groovy">

```Groovy
implementation 'love.forte.simbot.component:simbot-component-telegram-core:$VERSION'
```

如果使用 Java 而不配合使用 Gradle 的 `kotlin` 插件, 那么你需要指定依赖的后缀为 `-jvm`。

```Groovy
implementation 'love.forte.simbot.component:simbot-component-telegram-core-jvm:$VERSION'
```

</tab>
<tab title="Maven" group-key="maven">

```xml
<dependency>
    <groupId>love.forte.simbot.component</groupId>
    <artifactId>simbot-component-telegram-core-jvm</artifactId>
    <version>${VERSION}</version>
</dependency>
```

</tab>
</tabs>
