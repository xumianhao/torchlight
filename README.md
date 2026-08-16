# 《火炬之光：无限》国服攻略知识库

本仓库为《火炬之光：无限》国服建立可长期维护、可追溯、按赛季演进的攻略知识库，服务于 BD 攻略、策略攻略、知识维护与玩家问答。所有正式结论以国服证据为基础，并保留来源、时效与不确定性信息。

这是知识库脚手架，不包含任何当前赛季的玩法、数值、BD 强度或市场结论。后续维护者应先遵循根级运行契约，再按治理规范和模板新增经过核验的内容。

## 从这里开始

- [运行契约](AGENTS.md)
- 治理规范：[分类体系](governance/taxonomy.md)、[来源政策](governance/source-policy.md)、[时效政策](governance/freshness-policy.md)、[质量清单](governance/quality-checklist.md)、[待验证清单](governance/pending-verification.md)、[冲突日志](governance/conflict-log.md)
- 内容模板：[BD 攻略](templates/build-guide.md)、[策略攻略](templates/strategy-guide.md)、[问答条目](templates/qa-entry.md)、[来源记录](templates/source-record.md)、[验证记录](templates/verification-record.md)
- 知识区域：[稳定知识](knowledge/core/README.md)、[赛季知识](knowledge/seasons/README.md)、[可复用问答](knowledge/qa/README.md)
- [历史归档](archive/README.md)

## 路由示例

- 可跨赛季复用的稳定机制，写入 `knowledge/core/`。
- 仅适用于某赛季的 BD，写入 `knowledge/seasons/<season-id>/`。
- 基于已核验资料、能够重复回答的问题，写入 `knowledge/qa/`。
- 缺少可确认国服证据的说法，登记到 `governance/pending-verification.md`。
- 已失效但需要保留证据脉络的攻略，移入 `archive/` 并保留其赛季和失效原因。

## 维护原则

仅处理国服；先有证据再有结论；赛季元数据为必填项；让不确定性可见；保留历史和冲突证据。
