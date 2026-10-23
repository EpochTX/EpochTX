# Agent 独立分流（Muse + Hark）

**策略组名称：`Agent`（首字母大写）。** 自 2026-10-23 起，把 Muse 和 Hark 从其他 AI 服务分开选择代理出口。仅包括以下各自专属域名：

- `DOMAIN-SUFFIX,muse.ai`：Muse 个人 Agent
- `DOMAIN-SUFFIX,hark.com`：Hark AI

`meta.ai` / `ai.meta.com` 等共享的 Meta AI 域名**仍归属** [AI 综合规则](../AI/README.md)，避免把 Meta AI、Instagram、WhatsApp 的共享服务错误重定向到 Agent。真实 App 若访问其他 CDN 或通用平台域名，应以连接日志验证后再按专属域名补充，不建议扩大为全部 `meta.com`。

| 规则文件 | 客户端 | 引入方法 |
| --- | --- | --- |
| [Agent.list](Agent.list) | Surge / Loon / Shadowrocket | 作为独立远程规则，绑定到 `Agent` |
| [Agent.yaml](Agent.yaml) | Mihomo / Clash Meta | `rule-providers` 中作为 classical YAML，优先于 `AI` 匹配 |
| [Agent.qx.list](Agent.qx.list) | Quantumult X | `filter_remote` 绑定策略 `Agent` |

规则使用 `https://raw.githubusercontent.com/EpochTX/EpochTX/main/rules/Agent/` 前缀。优先于泛 AI / Google 规则，以确保 Muse 和 Hark 不被已存在的 AI 规则抢先匹配。选择 `Agent` 组的出口节点不影响其他 `AI服务`。
