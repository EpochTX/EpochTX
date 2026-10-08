# AI 综合分流规则

统一收录主流通用 AI、编程助手、API/模型平台、AI 搜索、图像、视频、音乐、语音服务及国内 AI。更新：2026-10-08。

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

- 共 **193 条**不重复的域名匹配规则（16 个分类）。包括 OpenAI/ChatGPT/Codex，Claude，Gemini/AI Studio/NotebookLM，Copilot，Grok，Perplexity，DeepSeek，Qwen，Kimi，豆包，智谱 GLM，MiniMax，Cursor，Windsurf，Poe，Hugging Face，OpenRouter，Midjourney，Runway，Kling，Suno，ElevenLabs 等。
- 涵盖 ChatGPT 的静态和图片附件域名 `oaistatic.com`、`oaiusercontent.com`、`cdn.openaimerge.com`，以及其它已知的专属资源域名。主要参考 [OpenAI 网络建议](https://help.openai.com/en/articles/9247338-network-recommendations-for-chatgpt-errors-on-web-and-apps) 和 [blackmatrix7 OpenAI 规则](https://github.com/blackmatrix7/ios_rule_script/blob/master/rule/Surge/OpenAI/OpenAI.list)，但**没有无差别照搬**泛用的第三方 CDN、统计、认证或 ASN 规则。
- 只用精确域名及域名后缀；不包含 `google.com`、`googleapis.com`、`microsoft.com`、`bing.com`、`github.com`、`cloudflare.com`、`stripe.com`、`amazonaws.com` 等跨服务域名；它们可能使非 AI 业务误命中。
- **域名规则无法识别 URL 路径**。例如第三方搜索图片链接仍可能由外部图床提供，不一定命中「AI服务」。有些产品的登录、验证码、支付流程依赖共享平台域名，默认仍走其它分流。
- 某些国内 AI 会根据出口 IP 限制地区；若出现故障，给该服务设更高优先级的 DIRECT 或中国节点专属分流。
- 规则集只决定**往哪个策略组走**，不保证节点带宽、丢包、IP 质量或图片下载速度。排查图片卡顿应查看请求记录中的实际域名、命中规则、出口节点、连接/下载耗时。
- 服务随时调整域名，不能保证囊括互联网上的全部 AI。已停用的历史业务和未经核实的通配共享域名不做盲目收录。OpenAI Sora 产品已于 2026-04 停用，未单列旧业务域名。
- 多平台文件来自相同域名数据，调整时应一起更新，避免不同客户端策略漂移。

## 优先级

必须放在 Google、Microsoft、游戏平台、国内通用规则及 FINAL/MATCH 之前。原有 `AI服务` 策略组名称保持不变，无需新增节点组。
