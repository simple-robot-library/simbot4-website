<primary-label ref="experimental" />

# 在线代码生成器


你可以前往 [在线代码生成器](https://codegen.simbot.forte.love) 
根据所需条件生成一个 `Kotlin` + `Gradle` 的项目，并自带一些简单的示例代码。

<tip>

生成器为纯前端，无后端交互，主要使用 `Compose HTML` 和 `kotlinpoet` 构建并完成代码生成逻辑。
`kotlinpoet` 生成的代码中可能会携带 `public` 修饰符，在 Kotlin 的非严格模式下这是不必要的，如有需要可自行清理删除。

</tip>

如果代码生成器在使用时出现了一些问题、或生成产物不可用等，
也可直接前往
[](feedback-and-support.md)
中提到的地方反馈你遇到的问题。