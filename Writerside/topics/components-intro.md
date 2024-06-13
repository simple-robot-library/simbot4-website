# 概述

<tooltip term="组件"><control>组件</control></tooltip>
是simbot中特定平台功能的主要提供者，
是重要的核心概念之一，
是对一组一个或多个 
<tooltip term="组件标识"><control>组件标识</control></tooltip> 和 
<tooltip term="插件"><control>插件</control></tooltip>
的统称。

例如当我们说 “QQ频道组件”,
就是在描述一个包含了 **QQ频道组件标识 (`QQGuildComponent`)**
和 **QQ频道Bot管理器 (`QQGuildBotManager`)**
的整体。

<note>
举个不恰当的例子。
如果将 simbot 比喻为 Spring Boot，那么组件则可以类似地视为各种 starter。
</note>

