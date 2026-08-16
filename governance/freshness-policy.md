# 时效与版本迁移政策

## 必填元数据

每篇正式内容必须使用以下元数据；完成条目必须删除尖括号提示并替换为实际值。

```yaml
server: 国服
season: <明确赛季标识或 stable>
created_at: YYYY-MM-DD
verified_at: YYYY-MM-DD
status: current | needs-review | partially-outdated | archived | unknown
confidence: confirmed | supported | tentative | disputed | unverified
recheck_triggers:
  - <具体补丁、赛季或机制触发条件>
```

尖括号中的值是写作提示，不能保留在已完成的正式条目中；应删除提示或以实际赛季、日期和具体触发条件替换。`stable`仅用于经核验且不绑定单一赛季的稳定国服资料，不能据此跳过重检。

## 内容状态定义

| 状态 | 定义 |
| --- | --- |
| `current` | 已按适用国服赛季或版本核验，且没有已知影响其结论的未处理变更。 |
| `needs-review` | 已知新赛季、补丁或相关机制变化可能影响结论，尚未完成国服复核。 |
| `partially-outdated` | 已复核，且同一文档中同时存在仍有效与已失效或未知的部分；必须标出边界。 |
| `archived` | 不再适用当前维护目标，但对历史脉络仍有价值的已保留资料。 |
| `unknown` | 缺少足以判定适用版本、赛季或时效的信息。 |

## 强制迁移

- 新赛季或相关补丁发布：`current` → `needs-review`。
- 复核后发现有效性混合：`needs-review` → `partially-outdated`。
- 完成国服核验：`needs-review` → `current`。
- 不再适用但具有历史价值：任一活跃状态 → `archived`，移入 `archive/` 并保留来源与溯源信息。
- 版本信息不足：任一非 `archived` 状态 → `unknown`。

“活跃状态”指除`archived`外的状态。迁移应同时更新 `verified_at`、触发原因、受影响的结论及关联链接；有冲突或未知部分时不得借由状态迁移抹除原始证据。

旧赛季结论不会默认成为当前赛季结论。只有完成与当前国服赛季相符的核验后，才能将其标为`current`；名称相同、历史效果相似或国际服资料均不足以替代该核验。
