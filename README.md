# Proxy Rules

个人维护的代理分流规则列表。

## 规则列表

| 规则 | 文件 | 订阅地址 |
| --- | --- | --- |
| Apple Direct | [`apple_direct.list`](rules/apple/apple_direct.list) | [Raw](https://raw.githubusercontent.com/mikukko/proxy-rules/master/rules/apple/apple_direct.list) |
| Augment | [`augment.list`](rules/augment/augment.list) | [Raw](https://raw.githubusercontent.com/mikukko/proxy-rules/master/rules/augment/augment.list) |
| Cursor | [`cursor.list`](rules/cursor/cursor.list) | [Raw](https://raw.githubusercontent.com/mikukko/proxy-rules/master/rules/cursor/cursor.list) |
| Windsurf | [`windsurf.list`](rules/windsurf/windsurf.list) | [Raw](https://raw.githubusercontent.com/mikukko/proxy-rules/master/rules/windsurf/windsurf.list) |

> `Apple Direct` 是后置直连白名单，应放在 AppleProxy、AppleMusic 和 Apple-AI 代理规则之后使用。

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
