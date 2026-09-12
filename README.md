# Proxy Rules

个人维护的代理分流规则列表。

## 规则列表

| 规则 | 文件 | 订阅地址 |
| --- | --- | --- |
| Apple APNs | [`apple_apns.list`](rules/apple/apple_apns.list) | [Raw](https://raw.githubusercontent.com/mikukko/proxy-rules/master/rules/apple/apple_apns.list) |
| Apple Direct | [`apple_direct.list`](rules/apple/apple_direct.list) | [Raw](https://raw.githubusercontent.com/mikukko/proxy-rules/master/rules/apple/apple_direct.list) |
| Apple Proxy | [`apple_proxy.list`](rules/apple/apple_proxy.list) | [Raw](https://raw.githubusercontent.com/mikukko/proxy-rules/master/rules/apple/apple_proxy.list) |
| Augment | [`augment.list`](rules/augment/augment.list) | [Raw](https://raw.githubusercontent.com/mikukko/proxy-rules/master/rules/augment/augment.list) |
| ByteDance Direct | [`bytedance_direct.list`](rules/bytedance/bytedance_direct.list) | [Raw](https://raw.githubusercontent.com/mikukko/proxy-rules/master/rules/bytedance/bytedance_direct.list) |
| Cursor | [`cursor.list`](rules/cursor/cursor.list) | [Raw](https://raw.githubusercontent.com/mikukko/proxy-rules/master/rules/cursor/cursor.list) |
| Mikukko | [`mikukko.list`](rules/mikukko/mikukko.list) | [Raw](https://raw.githubusercontent.com/mikukko/proxy-rules/master/rules/mikukko/mikukko.list) |
| Windsurf | [`windsurf.list`](rules/windsurf/windsurf.list) | [Raw](https://raw.githubusercontent.com/mikukko/proxy-rules/master/rules/windsurf/windsurf.list) |

> `Apple APNs` 应优先于其他 Apple 规则加载，以便为推送流量指定独立策略。`Apple Proxy` 跟在 `Apple APNs` 之后，承接需要代理的 Apple 服务。`Apple Direct` 是直连白名单，需放在 `Apple Proxy` 之后、AppleMusic 和 Apple-AI 等上游代理规则之前，保证 App Store 商店与下载域名优先直连。`ByteDance Direct` 需放在 TikTok 等字节系代理规则之前，避免抖音国内 API 与 CDN 域名被误送入代理。`Mikukko` 收录个人自有域名及其子域名，建议放在通用代理规则之前，并在客户端为其指定普通代理策略，无需链式代理。

规则文件使用通用 Rule Set 写法，目前主要维护域名后缀规则：

```text
DOMAIN-SUFFIX,example.com
```

仓库只提供匹配规则，不包含 `DIRECT`、`PROXY` 等具体策略。请在所使用的代理配置中自行指定策略。

## 维护

新增服务时，在 `rules/` 下创建对应的服务目录和 `.list` 文件，并同步更新上方的规则列表。例如：

```text
rules/example/example.list
```

规则约定：

- 文件名使用小写字母和连字符。
- 每行只放一条规则。
- 域名使用小写字母，不包含协议和路径。
- 同一文件中不要添加重复规则。
