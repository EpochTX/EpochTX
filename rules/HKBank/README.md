# 香港银行（HKBank）跨平台代理分流

更新：2026-10-08。以历史 [HKBank.lsr](../HKBank.lsr) 为基础维护香港本地零售银行、持牌数字银行及其银行专属资源端点，并补齐恒生银行的两个现用官方域名。共 **48 条**域名规则。银行名单可参考[香港金管局持牌机构名录](https://vpr.hkma.gov.hk/eng/regulatory-resources/registers/register-of-ais-and-lros/)；并不声称涵盖全部持牌银行。

## 文件与支持软件

| 文件 | 软件 | 接入形式 |
| --- | --- | --- |
| [HKBank.list](HKBank.list) | Surge / Loon / Shadowrocket | `DOMAIN` / `DOMAIN-SUFFIX` 无策略名的远程规则集 |
| [HKBank.yaml](HKBank.yaml) | Mihomo / Clash Meta / OpenClash / Stash | `behavior: classical`, `format: yaml` |
| [HKBank.qx.list](HKBank.qx.list) | Quantumult X | `host` / `host-suffix`，策略设为 `direct` |
| [旧版 HKBank.lsr](../HKBank.lsr) | 已部署的历史配置 | 保留旧路径兼容，规则与新目录同步 |

原始链接前缀：`https://raw.githubusercontent.com/EpochTX/EpochTX/main/rules/HKBank/`

### 快速引用

- **Surge** 在 `[Rule]` 中：`RULE-SET,https://raw.githubusercontent.com/EpochTX/EpochTX/main/rules/HKBank/HKBank.list,DIRECT`
- **Loon** 在 `[Remote Rule]` 中：`https://raw.githubusercontent.com/EpochTX/EpochTX/main/rules/HKBank/HKBank.list, policy=DIRECT, tag=HKBank, enabled=true`
- **Quantumult X** 在 `[filter_remote]` 中：`https://raw.githubusercontent.com/EpochTX/EpochTX/main/rules/HKBank/HKBank.qx.list, tag=HKBank, force-policy=direct, enabled=true`
- **Mihomo/Clash** 在 `rule-providers:` 中：
```yaml
  HKBank:
    type: http
    behavior: classical
    format: yaml
    path: ./rulesets/HKBank.yaml
    url: "https://raw.githubusercontent.com/EpochTX/EpochTX/main/rules/HKBank/HKBank.yaml"
    interval: 86400
```
并在 `rules:` 靠前位置使用 `- RULE-SET,HKBank,DIRECT`。

## 规则设计

- 当前配置使用 **DIRECT**，与旧订阅一致；这只是出口选择，不代表一定能够绕过地区限制或保障银行账户安全。
- 只维护香港银行及其已明确归属的接口域名，不随意扩展到大陆银行、跨国银行全集、全量 `*.com.hk`、通用 CDN、验证码商、支付服务商或整片云 IP 段。
- 域名匹配不能识别同一主域下的香港 URL 路径；一些跨国银行可能共用全球主域（例如 `sc.com`），为了兼容旧规则予以保留。若想只分流香港业务，需结合实际连接日志再收窄。
- 从旧列表补齐 `hangseng.com.hk` 和 `hangsengbank.com.hk`，其它既有银行和数字银行条目保持不变。
- 不建议为了让银行网站打开而关闭证书校验或给银行流量启用 HTTPS MITM。
- 更新整个客户端配置和远程规则后，在请求日志中确认目标域名命中 `HKBank -> DIRECT`；未接入这套规则的客户端则仍按它原本规则执行。

所有格式均由同一套域名数据维护；修改银行名单时应同步更新 4 份规则文件（含兼容旧版）。
