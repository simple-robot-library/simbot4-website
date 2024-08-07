# Bot配置文件

<tldr>
<p>在使用 <b>Spring Boot</b> 时自动注册 bot 所需的配置文件。</p>
</tldr>

<tip>

如果你在使用 Spring Boot，
将配置文件放在在你的资源目录中:
<path>resources/simbot-bots/</path>，
并以 `.bot.json` 作为扩展名，例如 `mybot.bot.json`。

这个扫描目录是可配置的。
这是属于 simbot4 Spring Boot starter 的配置，可参考
[集成 Spring Boot](Spring-Boot.md)。

</tip>

## 示例

```json
{
    "component": "simbot.qqguild",
    "ticket": {
        "appId": "你的botId",
        "secret": "你的bot secret, 用于获取API的access_token(4.0.0-beta6开始)",
        "token": "你的bot token, 用作API的access_token(4.0.0-beta6之前)"
    }
}
```
{collapsible="true" default-state="expanded" collapsed-title="简单示例"}

```json
{
  "component": "simbot.qqguild",
  "ticket": {
    "appId": "你的botId",
    "secret": "你的bot secret, 用于获取API的access_token(4.0.0-beta6开始)",
    "token": "你的bot token, 用作API的access_token(4.0.0-beta6之前)"
  },
  "config": {
    "serverUrl": null,
    "shard": {
      "type": "full"
    },
    "intents": {
      "type": "raw",
      "intents": 1073741827
    },
    "clientProperties": null,
    "timeout": null,
    "cache": {
      "enable": true,
      "transmit": null,
      "dynamic": null,
      "dispatcher": null
    }
  }
}
```
{collapsible="true" default-state="collapsed" collapsed-title="完整示例"}

## 属性描述

<deflist>
<def title="component">

固定值：`simbot.qqguild`

</def>
<def title="ticket">

bot用于登录的票据信息，必填。

<deflist type="wide">
<def title="appId">

`String`

bot开发配置中的 `appID`

</def>
<def title="secret">

`String`

bot开发配置中的 `AppSecret`, 
`4.0.0-beta6` 版本开始用于获取API访问所需的 `access_token`。

</def>
<def title="token">

`String`

bot开发配置中的 `Token`。
`4.0.0-beta6` 版本**之前**用作API访问所需的 token。

</def>
</deflist>

</def>

<def title="config">
可选项，提供一些额外的可配置属性。

<deflist>
<def title="serverUrl">

`String`

目标服务器地址。默认为 `null`。

当值为特殊值：`"SANDBOX"` 时会选择使用 `QQGuild.SANDBOX_URL_STRING`，
也就是沙箱服务器地址。

| 配置值         | 实际值                                   |
|-------------|---------------------------------------|
| `null`      | `"https://api.sgroup.qq.com"`         |
| `"SANDBOX"` | `"https://sandbox.api.sgroup.qq.com"` |
| 其他          | 与配置值一致                                |


</def>
<def title="shard">

`ShardConfig`

分片信息配置，默认为 `Full`。

根据 `type` 值的不同，可使用不同的属性。

<deflist>
<def title="type='full'">

无额外属性，代表一个全量单片。

```json
{
    "type": "full"
}
```

</def>
<def title="type='simple'">

```json
{
   "type": "simple",
   "value": 0,
   "total": 1
}
```

<deflist type="medium">
<def title="value">对应的分片信息属性。</def>
<def title="total">对应的分片信息属性。</def>
</deflist>

</def>
</deflist>


</def>
<def title="intents">

`IntentsConfig?`

要订阅的事件的 intents 信息。默认 `1073741827`，
也就是订阅：
- 频道相关事件
- 频道成员相关事件
- 公域消息相关事件

根据 `type` 的不同可选的属性不同。

> 有关 `intents` 的更多信息可参考 [官方文档](https://bot.q.qq.com/wiki/develop/api-v2/dev-prepare/interface-framework/event-emit.html#事件订阅Intents)

<deflist>
<def title="type='raw'">

直接使用 `intents` 原始的标记位最终数值。

所有的事件对应的位值信息可前往参考 [官方文档](https://bot.q.qq.com/wiki/develop/api/gateway/intents.html) 。

```json
{
  "type": "raw",
  "intents": 1073741824
}
```

</def>
<def title="type='nameBased'">

通过名称寻找所有可用的 `EventIntents` 并合并为最终的 `intents`。
名称基于继承了 `EventIntents` 的 object 的简单名称，例如 `Guilds`。

名称支持开头大写或小写的驼峰，例如 `Guilds`, `guilds`,
或者全大写或全小写的snake(下划线)格式，例如 `PUBLIC_GUILD_MESSAGES`, `public_guild_messages`。

虽然支持的格式比较宽松，但名称匹配仍然是**区分大小写**的，
比如一个混用大小写的snake格式就是不允许的：`public_GUILD_messages` ❌。

> 开头小写的驼峰以及snake格式自 `v4.0.0-beta7` 开始支持，
> 在此之前仅支持开头大写的驼峰格式，例如 `Guild`。

```json
{
  "type": "nameBased",
  "names": ["Guilds", "PublicGuildMessages"]
}
```

</def>
<def title="type='bitBased'">
<note>自 <code>4.0.0-beta7</code> 开始支持</note>

通过 `intents` 的位索引值来配置 `intents` 的结果。

```json
{
  "type": "bitBased",
  "bits": [0, 1, 30]
}
```

上面的 `0, 1, 30` 即代表订阅 `1<<0 | 1<<1 | 1<<30`。

`bits` 的元素值应当在 0 ~ 31 之内，但是代码内不做校验。

</def>
</deflist>

</def>
<def title="clientProperties">

`Map<String, String>?`

用作 `Signal.Identify.Data.properties` 中的参数。

```json
{
  "config": {
    "clientProperties": {
      "k1": "v1",
      "foo": "bar"
    }
  }
}
```

</def>
<def title="timeout" type="wide">

`TimeoutConfig?`

与部分超时相关的配置信息。
当任意属性不为 `null` 时会为 bot 中用于请求API的 `HttpClient`
配置 [HttpTimeout][HttpTimeout] 插件。

默认为 `null`。

<deflist>
<def title="apiHttpRequestTimeoutMillis">

`Long?`

API请求中的超时请求配置。参考 [HttpTimeout][HttpTimeout] 中的相关说明。

默认为 `null`。

</def>
<def title="apiHttpConnectTimeoutMillis">

`Long?`

API请求中的超时请求配置。参考 [HttpTimeout][HttpTimeout] 中的相关说明。

默认为 `null`。

</def>
<def title="apiHttpSocketTimeoutMillis">

`Long?`

API请求中的超时请求配置。参考 [HttpTimeout][HttpTimeout] 中的相关说明。

默认为 `null`。

</def>
</deflist>

</def>
<def title="cache">

`CacheConfig?`

缓存相关配置。

```json
"config": {
    "cache": {
        "transmit": {
            "enable": true
        }
    }
}
```

有关 `transmit` 的详细描述，
请参考 `TransmitCacheConfig` 的文档注释或 API Doc。

</def>
</deflist>
</def>
</deflist>

[HttpTimeout]: https://ktor.io/docs/timeout.html
