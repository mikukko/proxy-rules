# Proxy Rules

个人维护的代理分流规则列表。

## 规则列表

| 规则 | 文件 | 订阅地址 |
| --- | --- | --- |
| Apple APNs | [`apple_apns.list`](rules/apple/apple_apns.list) | [Raw](https://raw.githubusercontent.com/mikukko/proxy-rules/master/rules/apple/apple_apns.list) |
| Apple Direct | [`apple_direct.list`](rules/apple/apple_direct.list) | [Raw](https://raw.githubusercontent.com/mikukko/proxy-rules/master/rules/apple/apple_direct.list) |
| Apple Proxy | [`apple_proxy.list`](rules/apple/apple_proxy.list) | [Raw](https://raw.githubusercontent.com/mikukko/proxy-rules/master/rules/apple/apple_proxy.list) |
| ByteDance Direct | [`bytedance_direct.list`](rules/bytedance/bytedance_direct.list) | [Raw](https://raw.githubusercontent.com/mikukko/proxy-rules/master/rules/bytedance/bytedance_direct.list) |
| Crypto | [`crypto.list`](rules/crypto/crypto.list) | [Raw](https://raw.githubusercontent.com/mikukko/proxy-rules/master/rules/crypto/crypto.list) |
| Mikukko | [`mikukko.list`](rules/mikukko/mikukko.list) | [Raw](https://raw.githubusercontent.com/mikukko/proxy-rules/master/rules/mikukko/mikukko.list) |

> `Apple APNs` 应优先于其他 Apple 规则加载，以便为推送流量指定独立策略。`Apple Direct` 是地区无关基础设施白名单，放在 `Apple APNs` 之后，仅让已完成商店鉴权的 App 下载、静态 CDN、系统更新和证书等已确认地区无关的流量直连。`Apple Proxy` 紧跟其后，承接 Apple Account、App Store 商店与支付、iCloud、地图和天气等地区敏感服务，并用 Apple 根域名兜底代理所有未明确列入直连白名单的新服务。AppleMusic 和 Apple-AI 等上游代理规则放在两者之后。`ByteDance Direct` 需放在 TikTok 等字节系代理规则之前，避免抖音国内 API 与 CDN 域名被误送入代理。`Mikukko` 收录个人自有域名及其子域名，建议放在通用代理规则之前，并在客户端为其指定普通代理策略，无需链式代理。

规则文件使用通用 Rule Set 写法，目前主要维护域名后缀规则：

```text
DOMAIN-SUFFIX,example.com
```

仓库只提供匹配规则，不包含 `DIRECT`、`PROXY` 等具体策略。请在所使用的代理配置中自行指定策略。

`Crypto` 覆盖交易所及备用/API/CDN 域名、钱包、行情、DeFi、区块浏览器和 RPC 服务，需放在通用 Cloudflare、Proxy、Global、Direct 和 IP 规则之前，并指定 `Crypto` 策略。`DOMAIN-SUFFIX,bybit.biz` 已覆盖 `api3.bybit.biz:443`；域名规则不写端口。来源、覆盖范围及 Surge 接入说明见 [Crypto 说明](rules/crypto/README.md)。

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
