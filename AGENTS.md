# Obsidian LLM Wiki 维护规则

这个 vault 遵循 Karpathy `llm-wiki` 思路：保留原始资料层、维护型 wiki 层和 LLM 维护规则层。

## 角色分工

- `RAW/` 是原始资料层。可以读取，但不要改写、重命名、内联总结或“顺手整理”这里的源文件，除非用户明确要求。
- `WIKI/` 是被维护的知识层。可以在这里创建、修订、链接和合并页面。
- `_system/` 保存模板、检查清单和维护说明。
- `index.md` 是内容地图。新增、删除、重命名或实质性修改 wiki 页面后，都要更新它。
- `log.md` 是追加式日志。每次导入、查询归档、清理、健康检查或结构调整后，都要追加一条带日期的记录。
- `INBOX/` 是暂存区，用来放用户还没有确认导入的资料。

## 页面约定

内部链接使用 Obsidian wiki 链接，例如：`[[WIKI/Concepts/LLM Wiki|LLM Wiki]]`。

每个被维护的 wiki 页面都应该以 YAML frontmatter 开头：

```yaml
---
type: concept
status: seed
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources:
  - "[[RAW/example.md]]"
tags:
  - wiki
---
```

允许的 `type` 值：

- `home`：首页或导航页
- `source-summary`：原始资料摘要
- `concept`：概念页
- `entity`：人物、组织、地点、产品等实体页
- `workflow`：工作流页
- `question`：被归档的问题与回答
- `synthesis`：跨资料综合分析页

允许的 `status` 值：

- `seed`：新建页面，只有初步整理
- `active`：常用且相对成熟
- `needs-review`：可能不完整、过时、矛盾或链接不足
- `deprecated`：保留历史，但已被其他页面取代

## 导入工作流

当用户要求导入或处理某个资料时：

1. 从 `RAW/` 或 `INBOX/` 读取资料。
2. 识别核心观点、实体、概念、证据和开放问题。
3. 在 `WIKI/Sources/` 创建或更新源摘要页面。
4. 更新相关的概念页、实体页、工作流页或综合页。
5. 在源摘要和相关页面之间添加有用的双向链接。
6. 更新 `index.md`。
7. 按下面格式向 `log.md` 追加记录：

```markdown
## [YYYY-MM-DD] ingest | 资料标题
- 来源：[[RAW/source-file.md]]
- 更新：[[WIKI/Sources/source-title]], [[WIKI/Concepts/example]]
- 说明：用一句话说明这次更新改变了什么。
```

## 查询工作流

当用户围绕 vault 提问时：

1. 先读取 `index.md`。
2. 搜索并读取最相关的 wiki 页面。
3. 只有在需要核验或补充上下文时，才读取原始资料。
4. 回答时链接到用过的 wiki 页面和原始资料。
5. 如果答案具有长期价值，主动提出归档为 `WIKI/Questions/` 或 `WIKI/Synthesis/` 页面；如果用户已经要求维护知识库，则可以直接归档。

## 健康检查工作流

健康检查时重点寻找：

- 存在于 `WIKI/` 但没有登记到 `index.md` 的页面。
- 被提到三次以上、但还没有独立页面的重要概念。
- 没有入链或出链的孤立页面。
- 跨页面互相冲突的说法。
- `updated` 日期明显过旧的页面。
- 没有链接到任何概念页或实体页的源摘要。
- 已在 `RAW/` 中存在、但还没有对应 `WIKI/Sources/` 摘要的原始资料。

小问题可以直接修复；需要用户判断的问题，简洁记录到 `_system/checklists/`。健康检查结束后，也要在 `log.md` 中追加记录。

## 编辑纪律

- 优先创建短小、互相链接的页面，而不是很长但边界模糊的笔记。
- 不要删除用户自己写的笔记，除非用户明确要求。
- 保留不确定性。证据不足的判断要标注“需要核验”或设置 `status: needs-review`。
- 向 wiki 页面加入新判断时，尽量至少链接一个来源。
- `log.md` 只追加，不重写历史。
- 修改结构性规则时，同步更新本文件和 [[使用说明]] 中相关内容。
