# Torchlight Infinite CN Agent Knowledge Base Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Create a Chinese-language, evidence-governed repository scaffold that guides AI Agents in researching, maintaining, and answering questions about 《火炬之光：无限》国服 BD and strategy content.

**Architecture:** Use a root `AGENTS.md` as the binding operational contract, with detailed policies in `governance/`, reusable content schemas in `templates/`, and a two-layer knowledge model under `knowledge/core/` and `knowledge/seasons/`. Preserve historical material in `archive/`, and validate the scaffold through deterministic file, metadata, vocabulary, link, and placeholder checks.

**Tech Stack:** Markdown, Git, PowerShell, ripgrep (`rg`). No runtime dependencies.

## Global Constraints

- Game scope is only 《火炬之光：无限》国服.
- The repository must support long-term maintenance across current and future seasons.
- International-server material must not be used as a source, supporting citation, or substitute for national-server evidence.
- Body copy is Simplified Chinese; stable repository paths use English names.
- In-game terms use official national-server names first and may include common aliases.
- Accuracy outranks completeness; unsupported claims must be downgraded or recorded for verification instead of guessed.
- Online Agents may research and add time-sensitive facts; offline Agents may only organize existing repository material and may not claim current-version verification.
- Agents may write directly to the formal knowledge base only after passing the required self-check.
- This implementation creates policy, directories, and empty reusable templates only; it must not add current-season gameplay claims.
- Canonical content status values are `current`, `needs-review`, `partially-outdated`, `archived`, and `unknown`.
- Canonical confidence values are `confirmed`, `supported`, `tentative`, `disputed`, and `unverified`.
- Canonical source grades are `S`, `A`, `B`, and `C`.

---

## File Map

- `AGENTS.md`: Binding instructions, priority order, workflow, hard prohibitions, routing, and completion criteria.
- `README.md`: Human-oriented project overview and navigation.
- `knowledge/core/README.md`: Stable-knowledge scope and placement rules.
- `knowledge/seasons/README.md`: Season-directory convention and rollover procedure.
- `knowledge/qa/README.md`: Reusable Q&A scope and evidence inheritance rule.
- `templates/build-guide.md`: Complete BD metadata and section schema.
- `templates/strategy-guide.md`: Complete strategy metadata and section schema.
- `templates/qa-entry.md`: Reusable Q&A metadata and response schema.
- `templates/source-record.md`: Traceable source record schema.
- `templates/verification-record.md`: Reproducible verification schema.
- `governance/taxonomy.md`: Extensible BD dimensions and strategy topics.
- `governance/source-policy.md`: National-server-only source grading and claim-language rules.
- `governance/freshness-policy.md`: Season, date, state transition, and re-verification rules.
- `governance/quality-checklist.md`: Mandatory pre-write checks for all four task types.
- `governance/pending-verification.md`: Empty operational register with a defined entry schema.
- `governance/conflict-log.md`: Empty conflict register with a defined decision schema.
- `archive/README.md`: Archive entry, preservation, and restoration rules.

### Task 1: Root Operating Contract and Navigation

**Files:**
- Create: `AGENTS.md`
- Create: `README.md`

**Interfaces:**
- Consumes: Approved design at `docs/superpowers/specs/2026-08-16-torchlight-infinite-agents-design.md`.
- Produces: Canonical repository rules and links that every later policy and template must follow.

- [ ] **Step 1: Write `AGENTS.md` with the binding operating contract**

Create these sections in this order:

```markdown
# AGENTS.md

## 项目使命
## 指令优先级
## 内容与服务器边界
## 质量优先级
## 接单后的强制工作流
## 联网与离线模式
## 知识路由
## 来源、可信度与时效
## 直接写入前的质量门槛
## 硬性禁止事项
## 四类任务的完成标准
## 关联规范
```

The content must state, without weakening language:

- This repository covers only national-server information.
- Repository or user-provided international-server material can be identified as out of scope but cannot support a gameplay conclusion.
- Priority order is server/freshness, factual support, BD practicality, maintainability, then consistency.
- The eight-step workflow is task classification, scope determination, connectivity check, repository search, national-server research, fact classification, formal write, and self-check.
- Knowledge routes to `knowledge/core/`, `knowledge/seasons/<season-id>/`, or `knowledge/qa/`; unresolved facts route to `governance/pending-verification.md`; resolved disagreements are recorded in `governance/conflict-log.md`.
- Online and offline permissions match the Global Constraints exactly.
- The canonical source grades, confidence values, and content statuses are listed exactly once and linked to their detailed policies.
- The prohibitions include fabricated citations or numbers, unmarked old-season reuse, treating simulations as in-game tests, relying on search snippets for key facts, deleting conflict evidence, timeless market prices, and relaxed Q&A standards.
- Completion criteria are explicit for BD writing, strategy writing, knowledge maintenance, and player Q&A.

- [ ] **Step 2: Verify the root contract contains every non-negotiable rule**

Run:

```powershell
$required = @(
  '仅.*国服',
  '国际服',
  '联网',
  '离线',
  'knowledge/core/',
  'knowledge/seasons/',
  'knowledge/qa/',
  'pending-verification.md',
  'conflict-log.md',
  'confirmed',
  'needs-review',
  '不得编造'
)
$text = Get-Content -Raw -LiteralPath 'AGENTS.md'
$missing = $required | Where-Object { $text -notmatch $_ }
if ($missing) { throw "AGENTS.md 缺少规则: $($missing -join ', ')" }
```

Expected: command exits successfully with no output.

- [ ] **Step 3: Write the human-facing `README.md`**

Include:

- A two-paragraph project purpose and explicit statement that the scaffold contains no current-season claims.
- A “从这里开始” list linking to `AGENTS.md`, all six governance documents, all five templates, the three knowledge areas, and the archive.
- A short routing example covering a stable mechanic, a season BD, a reusable Q&A entry, an unsupported claim, and an archived guide.
- A “维护原则” summary: national server only, evidence before claims, season metadata required, uncertainty made visible, and history preserved.

- [ ] **Step 4: Verify root links target planned paths**

Run:

```powershell
$expectedLinks = @(
  'AGENTS.md',
  'governance/taxonomy.md',
  'governance/source-policy.md',
  'governance/freshness-policy.md',
  'governance/quality-checklist.md',
  'governance/pending-verification.md',
  'governance/conflict-log.md',
  'templates/build-guide.md',
  'templates/strategy-guide.md',
  'templates/qa-entry.md',
  'templates/source-record.md',
  'templates/verification-record.md',
  'knowledge/core/README.md',
  'knowledge/seasons/README.md',
  'knowledge/qa/README.md',
  'archive/README.md'
)
$readme = Get-Content -Raw -LiteralPath 'README.md'
$missing = $expectedLinks | Where-Object { $readme -notmatch [regex]::Escape($_) }
if ($missing) { throw "README.md 缺少链接: $($missing -join ', ')" }
```

Expected: command exits successfully with no output. Targets are allowed not to exist until Tasks 2–4.

- [ ] **Step 5: Commit the root contract**

```powershell
git add -- AGENTS.md README.md
git commit -m "docs: add national-server agent contract"
```

### Task 2: Governance Policies and Operational Registers

**Files:**
- Create: `governance/taxonomy.md`
- Create: `governance/source-policy.md`
- Create: `governance/freshness-policy.md`
- Create: `governance/quality-checklist.md`
- Create: `governance/pending-verification.md`
- Create: `governance/conflict-log.md`

**Interfaces:**
- Consumes: Canonical rules and enumerations from `AGENTS.md`.
- Produces: Detailed policy definitions referenced by all templates and knowledge files.

- [ ] **Step 1: Create the extensible taxonomy**

Write `governance/taxonomy.md` with:

- BD dimensions for hero and trait, core skill and archetype, progression stage, budget, use case, and content state.
- Exact progression labels: `开荒`, `过渡`, `成型`, `极限`.
- Exact budget labels: `低`, `中`, `高`, `极限`.
- Strategy topics covering season start, archetype choice, crafting, skill and talent growth, currency and resource planning, market trading, Netherrealm progression and farming, bosses, season mechanics and events, efficiency and risk analysis, account growth, and tools or simulation methods.
- Extension rule: add a narrowly defined child category when new mechanics do not fit; do not rename existing categories without updating every reference.
- Classification examples that describe routing only and contain no gameplay facts.

- [ ] **Step 2: Create the source and claim policy**

Write `governance/source-policy.md` with:

- National-server-only scope and explicit international-server exclusions.
- `S`, `A`, `B`, and `C` definitions from the design.
- Rule that a database or simulator receives a grade based on provenance, season match, and reproducibility rather than tool category alone.
- Minimum source-record fields matching `templates/source-record.md`.
- Claim language table:

| Confidence | Permitted wording |
|---|---|
| `confirmed` | 可以使用确定陈述，并紧邻关键依据 |
| `supported` | 使用“现有国服证据支持”等限定语 |
| `tentative` | 使用“初步推测”并说明缺失证据 |
| `disputed` | 并列冲突结论、条件和当前取舍 |
| `unverified` | 不进入确定性正文，只登记待验证项 |

- Rule that `C` sources cannot independently support a key mechanic, numeric claim, recommendation, or market conclusion.
- Rule that search-result snippets are discovery aids only.

- [ ] **Step 3: Create freshness and transition policy**

Write `governance/freshness-policy.md` with required metadata:

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

Explain that angle-bracket values are authoring prompts removed or replaced in completed entries. Define every status and these transitions:

- New season or relevant patch: `current` → `needs-review`.
- Mixed validity after review: `needs-review` → `partially-outdated`.
- Successful national-server verification: `needs-review` → `current`.
- No longer applicable but historically useful: any active state → `archived` and move to `archive/` with provenance retained.
- Insufficient version information: any non-archived state → `unknown`.

State that old-season conclusions never become current-season conclusions by default.

- [ ] **Step 4: Create the mandatory quality checklist**

Write `governance/quality-checklist.md` with checkboxes grouped under:

- Common checks: national-server-only evidence, connectivity disclosure, metadata, traceable claims, fact/test/simulation/inference separation, conflict handling, link validity, and unresolved claim downgrade.
- BD checks: audience, stage, budget, use, difficulty, core conditions, alternatives, progression, offense, defense, resource loop, operation, failure cases, testing limits, and sources.
- Strategy checks: scope, prerequisites, steps, cost, return, risk, time, branches, exit conditions, common errors, sample limits, and sources.
- Q&A checks: direct answer, conditions, exceptions, season, evidence, linked guide, and recheck triggers.
- Maintenance checks: destination, duplicate detection, affected links, state transition, preserved history, and conflict register.

Finish with a binding rule: a failed key check blocks a definite claim, though the evidence or question may be recorded for verification.

- [ ] **Step 5: Create the empty verification and conflict registers**

Write `governance/pending-verification.md` with usage rules and this empty-entry schema:

```markdown
## VER-YYYYMMDD-NNN：命题简述

- 状态：open | testing | resolved | closed-insufficient-evidence
- 适用服务器：国服
- 适用赛季：
- 待验证命题：
- 现有证据与等级：
- 缺失证据：
- 建议验证方法：
- 关联文档：
- 建立日期：
- 最近更新：
- 处理结论：
```

Write `governance/conflict-log.md` with usage rules and this empty-entry schema:

```markdown
## CON-YYYYMMDD-NNN：冲突简述

- 状态：open | resolved | superseded
- 适用服务器：国服
- 适用赛季：
- 冲突命题：
- 证据 A、等级与条件：
- 证据 B、等级与条件：
- 差异原因分析：
- 当前采用结论：
- 采用理由：
- 关联待验证编号：
- 关联文档：
- 建立日期：
- 最近更新：
```

Explain that the blank fields are schema labels, not unresolved repository work.

- [ ] **Step 6: Verify canonical vocabularies and exclusions**

Run:

```powershell
$source = Get-Content -Raw 'governance/source-policy.md'
$fresh = Get-Content -Raw 'governance/freshness-policy.md'
$quality = Get-Content -Raw 'governance/quality-checklist.md'
foreach ($grade in @('`S`','`A`','`B`','`C`')) {
  if ($source -notmatch [regex]::Escape($grade)) { throw "缺少来源等级 $grade" }
}
foreach ($value in @('current','needs-review','partially-outdated','archived','unknown','confirmed','supported','tentative','disputed','unverified')) {
  if ($fresh -notmatch [regex]::Escape($value)) { throw "缺少时效或可信度值 $value" }
}
if ($source -notmatch '国际服') { throw '来源规范缺少国际服排除规则' }
if ($quality -notmatch '国服') { throw '质量清单缺少国服检查' }
```

Expected: command exits successfully with no output.

- [ ] **Step 7: Commit governance policies**

```powershell
git add -- governance
git commit -m "docs: define evidence and freshness governance"
```

### Task 3: Reusable Content Templates

**Files:**
- Create: `templates/build-guide.md`
- Create: `templates/strategy-guide.md`
- Create: `templates/qa-entry.md`
- Create: `templates/source-record.md`
- Create: `templates/verification-record.md`

**Interfaces:**
- Consumes: Canonical taxonomy, status, confidence, source grades, and checklist rules from `governance/`.
- Produces: Copyable schemas for every formal content type.

- [ ] **Step 1: Create the BD guide template**

Start `templates/build-guide.md` with this metadata schema:

```yaml
---
title: "<国服官方术语优先的 BD 名称>"
document_type: build
server: 国服
season: "<明确赛季标识>"
created_at: YYYY-MM-DD
verified_at: YYYY-MM-DD
status: current | needs-review | partially-outdated | archived | unknown
confidence: confirmed | supported | tentative | disputed | unverified
hero: "<英雄>"
hero_trait: "<英雄特性>"
core_skill: "<核心技能>"
progression_stage: 开荒 | 过渡 | 成型 | 极限
budget: 低 | 中 | 高 | 极限
use_cases: []
operation_difficulty: 低 | 中 | 高
recheck_triggers: []
sources: []
---
```

Follow it with these exact sections: 一句话定位, 适用范围, 核心机制与成立条件, 英雄与特性, 技能与辅助技能, 天赋, 装备与关键词条, 平价替代与不可替代项, 成长路线, 输出机制, 防御机制, 资源循环, 操作方法, 优点, 缺点与失败条件, 不适用场景, 测试条件与局限, 已知争议, 待验证项, 来源记录, 发布前自检.

Add authoring notes that prompts in angle brackets and example enum alternatives must be replaced by one concrete value in a completed guide. Prohibit exact damage ceilings or “毕业强度” claims without real national-server verification.

- [ ] **Step 2: Create the strategy guide template**

Use the same common metadata names, with `document_type: strategy`, plus `topic`, `audience`, and `player_stage`. Sections must be: 问题与目标, 适用范围, 前置条件, 执行步骤, 成本, 预期收益, 风险, 时间投入, 分预算与阶段方案, 停止或切换条件, 常见错误与纠正, 数据样本, 假设与局限, 待验证项, 来源记录, 发布前自检.

State that market values require season, observation date, sample conditions, and cannot be presented as timeless fixed prices.

- [ ] **Step 3: Create the reusable Q&A template**

Use common metadata with `document_type: qa`, plus `canonical_question`, `aliases`, `audience`, `player_stage`, `related_guides`, and `recheck_triggers`. Sections must be: 简短结论, 条件化回答, 例外情况, 依据与可信度, 关联攻略, 重新核验条件, 来源记录, 发布前自检.

State that a Q&A entry inherits evidence from linked knowledge but must still show season and verification metadata.

- [ ] **Step 4: Create source and verification record templates**

`templates/source-record.md` must capture:

- Record ID formatted `SRC-YYYYMMDD-NNN`.
- Title or identifying description.
- Source type: official, in-game, reproducible-test, community, video, livestream, database, simulator, user-report, or discovery-lead.
- Publisher or tester.
- Publication date and access date.
- URL or repeatable location method.
- National-server and season applicability.
- Supported claim.
- Test conditions or context.
- Grade `S | A | B | C` and grading reason.
- Limitations and linked documents.

`templates/verification-record.md` must capture:

- Record ID formatted `TEST-YYYYMMDD-NNN`.
- Claim under test.
- National-server season and patch context.
- Method, environment, controlled variables, sample size, and raw results.
- Conclusion and confidence.
- Execution date, executor or source, linked source records, linked knowledge, and unresolved questions.
- Explicit distinction between in-game testing, database observation, and simulation.

- [ ] **Step 5: Verify common metadata names are consistent**

Run:

```powershell
$contentTemplates = @(
  'templates/build-guide.md',
  'templates/strategy-guide.md',
  'templates/qa-entry.md'
)
foreach ($file in $contentTemplates) {
  $text = Get-Content -Raw $file
  foreach ($field in @('server:', 'season:', 'created_at:', 'verified_at:', 'status:', 'confidence:', 'recheck_triggers:', 'sources:')) {
    if ($text -notmatch [regex]::Escape($field)) { throw "$file 缺少字段 $field" }
  }
}
$all = ($contentTemplates | ForEach-Object { Get-Content -Raw -LiteralPath $_ }) -join "`n"
foreach ($value in @('current','needs-review','partially-outdated','archived','unknown','confirmed','supported','tentative','disputed','unverified')) {
  if ($all -notmatch [regex]::Escape($value)) { throw "模板缺少枚举值 $value" }
}
```

Expected: command exits successfully with no output.

- [ ] **Step 6: Commit content templates**

```powershell
git add -- templates
git commit -m "docs: add guide and evidence templates"
```

### Task 4: Knowledge-Area and Archive Contracts

**Files:**
- Create: `knowledge/core/README.md`
- Create: `knowledge/seasons/README.md`
- Create: `knowledge/qa/README.md`
- Create: `archive/README.md`

**Interfaces:**
- Consumes: Routing, freshness, and template rules from `AGENTS.md`, `governance/`, and `templates/`.
- Produces: Local placement contracts that prevent stable, seasonal, Q&A, and historical content from being mixed.

- [ ] **Step 1: Define stable-knowledge placement**

Write `knowledge/core/README.md` with:

- Stable knowledge means national-server concepts not intentionally tied to one season; “stable” does not mean permanently true.
- Every formal document still requires `server: 国服`, dates, status, confidence, and recheck triggers.
- Season-specific numbers, markets, builds, activities, and balance conclusions do not belong here.
- If a stable document gains season-dependent sections, move those sections to the relevant season and link both directions.
- Content is organized by focused subject subdirectories only when the first real document requires them; do not pre-create empty topic trees.

- [ ] **Step 2: Define season directory and rollover rules**

Write `knowledge/seasons/README.md` with:

- New season path format `knowledge/seasons/<season-id>/`.
- Required first-level directories for a populated season: `builds/` and `strategies/`.
- `<season-id>` must be a stable, filesystem-safe identifier documented in the season README; official Chinese season name stays in metadata.
- Every populated season gets its own README recording official name, identifier, start context, known patch scope, and last repository review date.
- New-season rollover marks affected prior content `needs-review`; nothing is copied as `current` before verification.
- Cross-season reuse must cite the earlier file and document what was rechecked.
- Do not create a concrete current-season directory in this scaffold.

- [ ] **Step 3: Define Q&A evidence inheritance**

Write `knowledge/qa/README.md` with:

- Q&A entries answer recurring questions and link to underlying stable or seasonal knowledge.
- A Q&A entry is not an evidence-free shortcut; it includes its own server, season, date, status, and confidence metadata.
- Time-sensitive questions route to season-specific evidence and are marked for recheck.
- One-off unsupported questions route to `governance/pending-verification.md`, not to formal Q&A.
- Use `templates/qa-entry.md` for every formal entry.

- [ ] **Step 4: Define archive preservation and restoration**

Write `archive/README.md` with:

- Archive when content no longer applies but remains historically or diagnostically useful.
- Preserve original metadata, source records, prior path, archival date, reason, and replacement link where available.
- Archived material cannot answer current-season questions without fresh verification.
- Restoration requires national-server verification and a normal content status transition; do not silently move a file back.
- Do not delete conflict evidence or source provenance during archival.

- [ ] **Step 5: Verify placement boundaries do not overlap**

Run:

```powershell
$core = Get-Content -Raw 'knowledge/core/README.md'
$seasons = Get-Content -Raw 'knowledge/seasons/README.md'
$qa = Get-Content -Raw 'knowledge/qa/README.md'
$archive = Get-Content -Raw 'archive/README.md'
if ($core -notmatch '不.*赛季|不属于') { throw 'core 缺少赛季排除边界' }
if ($seasons -notmatch 'builds/' -or $seasons -notmatch 'strategies/') { throw 'seasons 缺少内容分区' }
if ($qa -notmatch 'pending-verification.md') { throw 'qa 缺少无证据问题路由' }
if ($archive -notmatch '不得|不能' -or $archive -notmatch '核验') { throw 'archive 缺少当前使用限制' }
```

Expected: command exits successfully with no output.

- [ ] **Step 6: Commit knowledge-area contracts**

```powershell
git add -- knowledge archive
git commit -m "docs: define knowledge and archive boundaries"
```

### Task 5: Cross-Repository Validation and Final Cleanup

**Files:**
- Modify only files from Tasks 1–4 when a validation failure exposes a concrete inconsistency.

**Interfaces:**
- Consumes: Entire scaffold.
- Produces: A clean, internally linked, vocabulary-consistent repository with no current-season claims.

- [ ] **Step 1: Verify every designed artifact exists**

Run:

```powershell
$requiredFiles = @(
  'AGENTS.md','README.md',
  'knowledge/core/README.md','knowledge/seasons/README.md','knowledge/qa/README.md',
  'templates/build-guide.md','templates/strategy-guide.md','templates/qa-entry.md',
  'templates/source-record.md','templates/verification-record.md',
  'governance/taxonomy.md','governance/source-policy.md','governance/freshness-policy.md',
  'governance/quality-checklist.md','governance/pending-verification.md','governance/conflict-log.md',
  'archive/README.md'
)
$missing = $requiredFiles | Where-Object { -not (Test-Path -LiteralPath $_ -PathType Leaf) }
if ($missing) { throw "缺少文件: $($missing -join ', ')" }
```

Expected: command exits successfully with no output.

- [ ] **Step 2: Verify relative Markdown links resolve**

Run:

```powershell
$errors = [System.Collections.Generic.List[string]]::new()
Get-ChildItem -Recurse -Filter '*.md' | ForEach-Object {
  $file = $_
  $text = Get-Content -Raw -LiteralPath $file.FullName
  [regex]::Matches($text, '\[[^\]]+\]\((?!https?://|#)([^)]+\.md(?:#[^)]+)?)\)') | ForEach-Object {
    $raw = $_.Groups[1].Value -replace '#.*$',''
    $target = Join-Path $file.DirectoryName $raw
    if (-not (Test-Path -LiteralPath $target -PathType Leaf)) {
      $errors.Add("$($file.FullName) -> $raw")
    }
  }
}
if ($errors.Count) { throw "失效 Markdown 链接:`n$($errors -join "`n")" }
```

Expected: command exits successfully with no output.

- [ ] **Step 3: Scan for unresolved authoring markers outside templates**

Run:

```powershell
$nonTemplates = Get-ChildItem -Recurse -Filter '*.md' |
  Where-Object { $_.FullName -notmatch '[\\/]templates[\\/]' -and $_.FullName -notmatch '[\\/]docs[\\/]superpowers[\\/]' -and $_.FullName -notmatch '[\\/]\.superpowers[\\/]' }
$hits = $nonTemplates | Select-String -Pattern '\b(TODO|TBD)\b|待定|稍后补充'
if ($hits) { $hits | ForEach-Object { $_.ToString() }; throw '发现未解决的编写标记' }
```

Expected: command exits successfully with no output. Template prompts are intentionally excluded because they are authoring schemas.

- [ ] **Step 4: Scan for accidental international-server endorsement or current-season claims**

Run:

```powershell
$files = Get-ChildItem -Recurse -Filter '*.md' |
  Where-Object { $_.FullName -notmatch '[\\/]docs[\\/]superpowers[\\/]' -and $_.FullName -notmatch '[\\/]\.superpowers[\\/]' }
$currentSeasonPatterns = '当前赛季为|本赛季最强|当前版本伤害|现版本价格'
$claims = $files | Select-String -Pattern $currentSeasonPatterns
if ($claims) { $claims | ForEach-Object { $_.ToString() }; throw '脚手架包含未经研究的当前赛季结论' }
$scopeFiles = @('AGENTS.md','governance/source-policy.md')
foreach ($file in $scopeFiles) {
  if ((Get-Content -Raw $file) -notmatch '国际服') { throw "$file 缺少国际服边界" }
}
```

Expected: command exits successfully with no output.

- [ ] **Step 5: Run Git whitespace and repository checks**

Run:

```powershell
git diff --check
git status --short
```

Expected: `git diff --check` has no output. `git status --short` lists only intentional fixes from this task, or nothing.

- [ ] **Step 6: Fix only concrete validation failures and rerun Steps 1–5**

For each failure, edit the responsible file so it conforms to the approved design and canonical vocabulary. Rerun the exact failing command, then rerun all five validation commands. Do not add gameplay facts or expand the scaffold beyond the File Map.

- [ ] **Step 7: Commit validation fixes when present**

If Step 6 changed files:

```powershell
git add -- AGENTS.md README.md governance templates knowledge archive
git commit -m "docs: align knowledge scaffold validation"
```

If Step 6 changed no files, do not create an empty commit.

- [ ] **Step 8: Record final evidence**

Run:

```powershell
git status --short --branch
git log --oneline --decorate -5
```

Expected: working tree is clean, and the log shows the root contract, governance, templates, and knowledge-boundary commits.
