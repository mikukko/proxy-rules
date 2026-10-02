# Crypto 代理规则

整理日期：2026-10-02。`crypto.list` 共 401 条规则，采用不含策略的 `DOMAIN` / `DOMAIN-SUFFIX` 格式，可作为 Surge Rule Set 使用。

## Bybit 漏分流的原因

整理时检查的 blackmatrix7 `Crypto/Crypto.list` 中，Bybit 条目只有 `DOMAIN-SUFFIX,bybit.com`，无法匹配 iOS App 实际使用的 `api3.bybit.biz`。`bybit.biz` 也收录在 [v2fly 的 Bybit 清单](https://github.com/v2fly/domain-list-community/blob/master/data/bybit) 中。

本规则已加入：

```text
DOMAIN-SUFFIX,bybit.biz
```

这条规则覆盖 `bybit.biz`、`api3.bybit.biz` 及其他子域名，适用于 443 和其他端口，无需另写 `DOMAIN,api3.bybit.biz`。Bybit 分组还覆盖社区清单的 API/备用域名及 [Bybit 官方文档](https://bybit-exchange.github.io/docs/v5/guide) 列出的地区 API 入口。日本和香港合作方只匹配文档列出的 API 主机，避免把其整个金融网站归入 Crypto。

## 覆盖范围

| 类别 | 主要服务 |
| --- | --- |
| 交易所 | Bybit、Binance、OKX、Gate、HTX/Huobi、Kraken、KuCoin、Deribit、Bitget、MEXC、Coinbase、Crypto.com、BingX、Bitfinex、Bitstamp、Gemini、BitMart、CoinEx、Phemex、Upbit、Backpack 等 |
| 钱包与连接 | MetaMask、Phantom、Rabby、Trust Wallet、OneKey、SafePal、Ledger、Trezor、imToken、TokenPocket、TronLink、UniSat、WalletConnect、Bitget Wallet |
| 行情与研究 | CoinMarketCap、CoinGecko、CoinGlass、TradingView、AICoin、DeFiLlama、DEX Screener、DEXTools、Nansen、Glassnode、CryptoQuant 等 |
| DeFi、DEX 与 NFT | Uniswap、PancakeSwap、Jupiter、Raydium、Sushi、1inch、Aave、Compound、Curve、dYdX、Hyperliquid、Lighter、OpenSea、Polymarket 等 |
| 区块浏览器 | Etherscan、BscScan、Solscan、Tronscan、Blockchair、OKLink、Tokenview 等 |
| 链与 RPC | Ethereum、Solana、Bitcoin、Base、Arbitrum、Optimism、Avalanche、Infura、Alchemy、Helius、Quicknode、dRPC、NodeReal、Flashbots、The Graph 等 |
| 数字资产支付 | RedotPay、StraitsX、Nexo、CoinGate、Cryptomus 等 |

根域名后缀规则同时覆盖其网页、API、WebSocket、静态资源和其他子域名。另一个根域名上的 API/CDN 必须单独收录，不能由主站域名推断出来。

清单保留上游中 FTX、Bittrex、LocalBitcoins 等历史域名以兼容旧链接；收录不代表服务仍在运营。社区归类也不等于官方认可。该文件是已知域名的覆盖快照，不能保证覆盖所有交易所、所有地区及未来新增的 App 请求；新增主机名仍应结合连接日志和来源补齐。未在手机上执行实机复测。

## 接入 Surge

可将原来的 Crypto 订阅替换为：

```ini
RULE-SET,https://raw.githubusercontent.com/mikukko/proxy-rules/master/rules/crypto/crypto.list,Crypto,extended-matching,update-interval=21600
```

也可将 `crypto.list` 导入客户端，用本地规则文件绑定 `Crypto` 策略；Surge Mac 支持直接引用本地绝对路径，请将示例路径替换为实际路径：

```ini
RULE-SET,/path/to/crypto.list,Crypto,extended-matching
```

将 Crypto 放在通用 Cloudflare、Proxy、Global、Direct 及 IP 规则之前。Surge [按从上到下的顺序使用首个命中的规则](https://manual.nssurge.com/rules/overview.html)，因此通用规则先命中后，后面的 Crypto 不会再被检查。规则集内不用填写 `Crypto` 策略，也不用写 `:443`。[域名匹配语义](https://manual.nssurge.com/rules/domain.html) 和 [Rule Set 用法](https://manual.nssurge.com/rules/ruleset.html) 见官方手册。

更新或导入规则后，重新建立 Bybit 连接，在请求详情中确认 `api3.bybit.biz` 命中这份规则并使用 `Crypto`。如果仍命中其他规则，检查加载状态、规则位置以及是否处于规则模式。对只有 IP 的请求，`extended-matching` 可在客户端取得 TLS SNI / HTTP Host 时辅助匹配，无法识别的 IP 请求不在域名清单覆盖范围内。

## 来源与整理方法

基础清单来自：

- [blackmatrix7 Crypto](https://github.com/blackmatrix7/ios_rule_script/blob/master/rule/Surge/Crypto/Crypto.list) 和 [Cryptocurrency](https://github.com/blackmatrix7/ios_rule_script/blob/master/rule/Surge/Cryptocurrency/Cryptocurrency.list)：沿用已有生态域名，补充明确的根域名匹配。
- [v2fly category-cryptocurrency](https://github.com/v2fly/domain-list-community/blob/master/data/category-cryptocurrency)：递归合并其引用的服务文件，补齐交易所 API、CDN、备用域名和钱包服务。

另据官方页面补充以下域名：

| 补充项 | 依据 |
| --- | --- |
| Bybit 地区入口 | [Bybit API Integration Guidance](https://bybit-exchange.github.io/docs/v5/guide) |
| Bitget、Bitget Wallet | [Bitget API](https://www.bitget.org/docs/uta/quick-start)、[Wallet WebSocket](https://web3.bitget.com/en/docs/market/websocket) |
| Gate API 入口 `gateio.ws` | [Gate API v4](https://www.gate.com/docs/developers/apiv4/en/) |
| MEXC 文档站 | [MEXC API](https://www.mexc.io/api-docs/futures/integration-guide) |
| Coinbase CDN | [Coinbase 发布的隐私政策 PDF](https://ctf-assets-01.coinbasecdn.net/c5bd0wqjc7v0/7yR5JAFpTqMiDaMbyKQhao/adcb89d37cca5ebfdbad563ce83586fa/Coinbase_Global_Privacy_Policy_-_exported_20240125_-_before_changes.pdf) |
| CoinEx、BitMart、Phemex、Upbit | [CoinEx](https://www.coinex.com/)、[BitMart](https://www.bitmart.com/)、[Phemex](https://phemex.com/)、[Upbit](https://www.upbit.com/) |
| Rabby、Backpack、Ledger | [Rabby](https://rabby.io/)、[Backpack](https://backpack.app/)、[Ledger](https://www.ledger.com/) |
| Jupiter、DEX Screener、Solscan | [Jupiter](https://developers.jup.ag/)、[DEX Screener](https://dexscreener.com/)、[Solscan](https://solscan.io/) |
| Alchemy、Helius、Quicknode | [Alchemy](https://www.alchemy.com/docs/get-started)、[Helius RPC](https://www.helius.dev/docs/api-reference/endpoints)、[Quicknode](https://www.quicknode.com/guides/quicknode-products/how-to-use-multichain-endpoint) |

每条规则的来源记录在 [`sources.json`](sources.json)，并记录下载内容的 SHA-256。清单没有使用宽泛的 `binance`、`huobi`、`metamask`、`okx` 关键词匹配，也未添加共享 Cloudflare、CloudFront、AWS、Zendesk、GitHub、AppsFlyer 根域名或 CDN 公用 IP 段。已有来源明确指向服务专属主机的共享平台条目使用精确 `DOMAIN` 匹配。

从上游排除 `crunchbase.com`、通用 `ipfs.io`，以及 Wisekey、Wynd 的通用身份认证/AI/带宽服务域名，避免扩大 Crypto 策略的适用范围。只为来源中明确出现的域名生成规则，未根据品牌名称猜测备用域名。

清单合并了 GPL-2.0 和 MIT 来源，保留作者信息及许可证，详见 [`THIRD-PARTY-NOTICES.md`](THIRD-PARTY-NOTICES.md) 和 [`LICENSE`](LICENSE)。
