# Bot配置文件

<include from="refers.md" element-id="doc-TODO"></include>

> 注释实际上是不支持的，
> 如果要复制，记得清理掉注释

```json
{
    "component": "simbot.telegram",
    "ticket": {
        // 你的*完整的*Bot Token, 别忘了前面的'bot'
        "token": "bot123456789:aaaabbbbcccc"
    },
    // 一些可选地属性，config以及里面的属性都是可选的，且基本上默认为 `null`。
    "config": {
        "server": null,
        "proxy": null,
        "longPolling": null
    }
}
```

如果你希望通过长轮询的方式达成主动 **_订阅_** 事件的效果，
记得配置 `longPolling`.

```json
{
    "component": "simbot.telegram",
    "ticket": {
      // 你的*完整的*Bot Token, 别忘了前面的'bot'
      "token": "bot123456789:aaaabbbbcccc"
    },
    // 一些可选地属性，config以及里面的属性都是可选的，且基本上默认为 `null`。
    "config": {
        "server": null,
        "proxy": {
          "type": "http",
          "url": "http://localhost:7790"
        },
        //配置长轮询来'订阅'事件
        "longPolling": {
            // 长轮询内部的每次事件接收量上限
            "limit": 100
        }
    }
}
```

