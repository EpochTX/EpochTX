# AI 综合分流规则

统一收录海外及国际化 AI：通用 AI、Agent、编程助手、API/模型平台、AI 搜索、图像、视频、音乐、语音服务。**中国大陆 AI 不纳入本策略**。更新：2026-10-09。

## 文件与平台

| 文件 | 客户端 | 用法 |
| --- | --- | --- |
| [AI.list](AI.list) | Surge、Loon、Shadowrocket | 普通远程规则集，**不含策略名**，由客户端绑定「AI服务」 |
| [AI.yaml](AI.yaml) | Mihomo / Clash Meta、OpenClash、Stash | rule-provider：`type: http, behavior: classical, format: yaml` |
| [AI.qx.list](AI.qx.list) | Quantumult X | `[filter_remote]` 远程规则，示例 `force-policy=AI服务` |

原始地址前缀：`https://raw.githubusercontent.com/EpochTX/EpochTX/main/rules/AI/`

### 配置示例

Surge `[Rule]`：`RULE-SET,https://raw.githubusercontent.com/EpochTX/EpochTX/main/rules/AI/AI.list,AI服务`

Loon `[Remote Rule]`：`https://raw.githubusercontent.com/EpochTX/EpochTX/main/rules/AI/AI.list, policy=AI服务, tag=AI综合, enabled=true`

Mihomo / Clash `rule-providers`：
```yaml
  AI:
    type: http
    behavior: classical
    format: yaml
    path: ./rulesets/AI.yaml
    url: "https://raw.githubusercontent.com/EpochTX/EpochTX/main/rules/AI/AI.yaml"
    interval: 86400
```
在 `rules:` 中：`- RULE-SET,AI,AI服务`。

Quantumult X `[filter_remote]`：`https://raw.githubusercontent.com/EpochTX/EpochTX/main/rules/AI/AI.qx.list, tag=AI综合, force-policy=AI服务, enabled=true`。

## 范围和原则

- 共 **169 条**不重复规则（原 146 条 + blackmatrix7 OpenAI 缺失的 22 条 + Cloudflare 验证域名 1 条；保留原 12 类及兼容扩展）。包括 OpenAI/ChatGPT/Codex、Claude、Gemini/AI Studio/NotebookLM、Copilot、Grok、Meta Muse、Meta AI、Perplexity、Poe、AI.com、Cursor、Windsurf、Hugging Face、OpenRouter、Midjourney、Runway、Suno、ElevenLabs 等国际服务。
- 涵盖 ChatGPT 的静态和图片附件域名 `oaistatic.com`、`oaiusercontent.com`、`cdn.openaimerge.com`，以及其它已知的专属资源域名。主要参考 [OpenAI 网络建议](https://help.openai.com/en/articles/9247338-network-recommendations-for-chatgpt-errors-on-web-and-apps) 和 [blackmatrix7 OpenAI 规则](https://github.com/blackmatrix7/ios_rule_script/blob/master/rule/Surge/OpenAI/OpenAI.list)，已按用户要求**完整补齐其全部 35 条规则**（旧文件已有 13 条，本次新增 22 条），另补充 `challenges.cloudflare.com` 供 Cloudflare 登录验证。
- 现在包含精确域名、域名后缀、`DOMAIN-KEYWORD`、`IP-CIDR` 和 `IP-ASN`。blackmatrix7 原规则包含 `stripe.com`、`auth0.com`、`sentry.io`、`IP-ASN,20473` 等共享平台或大范围 IP 段，**可能使非 AI 服务流量被错误分到 AI服务**。这是完全兼容旧规则的明确代价。
- **域名规则无法识别 URL 路径**。例如第三方搜索图片链接仍可能由外部图床提供，不一定命中「AI服务」。有些产品的登录、验证码、支付流程依赖共享平台域名，默认仍走其它分流。
- 中国大陆 AI 服务（包括 DeepSeek、通义千问、Kimi、豆包、智谱 GLM、MiniMax、腾讯元宝、文心、科大讯飞、Trae、MarsCode、Kling、PixVerse 等）不包含在此规则集中，仍由现有客户端的其它规则及兜底策略处理；**不代表一定直连**。
- 规则集只决定**往哪个策略组走**，不保证节点带宽、丢包、IP 质量或图片下载速度。排查图片卡顿应查看请求记录中的实际域名、命中规则、出口节点、连接/下载耗时。
- 服务随时调整域名，不能保证囊括互联网上的全部 AI。已停用的历史业务和未经核实的共享域名不做盲目收录。
- 已逐条比对并完整纳入旧版「智能助理」的 [blackmatrix7 OpenAI.list（35 条）](https://github.com/blackmatrix7/ios_rule_script/blob/master/rule/Surge/OpenAI/OpenAI.list)：已有 13 条，缺失 22 条全部追加。新增兼容规则统一放在文件末尾的 `blackmatrix7` 小节，便于日后审计、回滚。另独立追加 `challenges.cloudflare.com`（不属于这 35 条），用于 Cloudflare Challenge。
- 多平台文件来自相同域名数据，调整时应一起更新，避免不同客户端策略漂移。

## 优先级

必须放在 Google、Microsoft、游戏平台、国内通用规则及 FINAL/MATCH 之前。原有 `AI服务` 策略组名称保持不变，无需新增节点组。
