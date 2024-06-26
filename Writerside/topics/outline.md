# 概述

Simple Robot v4 是一个基于 Kotlin 协程的多平台 Bot 风格[Kotlin 多平台](https://kotlinlang.org/docs/multiplatform.html)事件调度框架，
异步高效、Java友好~

## 多平台实现 {id="multiplatform-targets"}

Simple Robot v4 基于[**KMP**](https://kotlinlang.org/docs/multiplatform.html) 实现多平台, 
支持 [native-target-support](https://kotlinlang.org/docs/native-target-support.html)
中所述的 `Tier 1`、`Tier 2`、`Tier 3` 三个级别的全部目标以及 `wasm-js` 平台目标。

<tip>

涉及到Ktor的模块暂时无法支持 `wasm-js` 平台。

</tip>


> 在部分需要 Ktor 依赖的模块中, 实现目标中会缺少那些 Ktor 尚不支持的平台。
> 这些目标大部分是 `Tier 2` 或 `Tier 3` 中的。

<procedure collapsible="true" title="支持目标列表">
<list>
<li>macosX64</li>
<li>macosArm64</li>
<li>iosSimulatorArm64</li>
<li>iosX64</li>

<li>linuxX64</li>
<li>linuxArm64</li>
<li>watchosSimulatorArm64</li>
<li>watchosX64</li>
<li>watchosArm32</li>
<li>watchosArm64</li>
<li>tvosSimulatorArm64</li>
<li>tvosX64</li>
<li>tvosArm64</li>
<li>iosArm64</li>

<li>androidNativeArm32</li>
<li>androidNativeArm64</li>
<li>androidNativeX86</li>
<li>androidNativeX64</li>
<li>mingwX64</li>
<li>watchosDeviceArm64</li>
</list>
</procedure>

## Java友好

Java是重要的伙伴, Kotlin天生与Java具有极强的兼容性。

simbot4 基于 **Java11**, 为所有模块提供 JVM 平台实现以及相应的 **模块化(JPMS)** 信息、
提供 **Spring Boot** 的 starter 快速快发实现、
以及异步、阻塞、预处理桥接器（可用于转化为响应式类型）的多种风格的挂起函数桥接API。

有关其中的部分详细信息, 可前往参考 
<a href="java-friendly.md" /> 。

## Bot风格

simbot4 主要用于对接那些 Bot 平台, 例如 QQ频道机器人、米游社大别野机器人、KOOK机器人等, 
因此 simbot 的 API 设计也是以 Bot 为起点进行设计的。

## 高性能异步事件调度框架

既然是 Bot, 那么就少不了事件。simbot4 基于 [**Kotlin 协程库**](https://kotlinlang.org/docs/coroutines-overview.html)
实现高性能、无阻塞、轻量且灵活的异步调度机制。