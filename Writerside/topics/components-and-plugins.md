# 组件与插件

本章节会介绍 simbot4 中的 **组件** 和 **插件** 的概念。

## 组件

在文档中, 常说的
<tooltip term="组件"><control>组件</control></tooltip>
就是是对一组一个或多个 
<tooltip term="组件标识"><control>组件标识</control></tooltip> 和 
<tooltip term="插件"><control>插件</control></tooltip>
的统称。

它是simbot中特定平台功能的主要提供者，是重要的核心概念之一

例如当我们说 “QQ频道组件”, 
就是在描述一个包含了 **QQ频道组件标识 (`QQGuildComponent`)**
和 **QQ频道Bot管理器 (`QQGuildBotManager`)**
的整体。

<note>
举个不恰当的例子。
如果将 simbot 不自量力地比喻为 Spring Boot，那么组件则可以类似地视为各种 starter。
</note>

## 组件标识

参考章节 [组件标识](component-id.md) 。

## 插件

参考章节 [插件](plugin.md) 。
