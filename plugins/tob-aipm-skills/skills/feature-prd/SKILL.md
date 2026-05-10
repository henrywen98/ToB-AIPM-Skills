---
name: feature-prd
description: 功能 PRD：单功能业务方向契约。从现状测绘 / 访谈 / 合同片段出发，写目标 / 不做什么 / 用户故事 / 粗验收条件。当用户说"写功能 PRD"、"出 PRD"、"/feature-prd"时触发。下游接 feature-spec（派生字段验收）。**输出严格遵守三条硬约束：self-contained、不引用 prior work、Feature 级不写 spec 细节。**
argument-hint: "<功能描述或问题陈述>"
---

# Feature PRD

写一份**单功能业务方向契约**——目标 / 不做什么 / 用户故事 / 粗验收条件 / 开放问题 / 时间线。下游接 `/feature-spec` 派生字段级验收 / 接口规格 / DDL / 协议。

详细写作指南、模板、各章节深入展开见 [`references/prd-writing-guide.md`](references/prd-writing-guide.md)。

## Usage

```
/feature-prd $ARGUMENTS
```

---

## Core Principles（不可违反）

四条核心原则。**生成前**逐条过一遍，**生成后**再逐条过 §Anti-Patterns 八条做 self-check。

| # | 原则 | ❌ | ✅ |
|---|------|---|---|
| 1 | **Self-Contained**：PRD 是开发者拉齐认知的唯一来源 | "详见原型 PRD §2"、"参考确认单"、"打开 X 文档看" | 把必要的背景、术语、数据结构直接抄进 PRD |
| 2 | **不引用 prior work**：描述产品本身应该是什么，不是它走到这版的历程 | "原型阶段是 mock"、"Phase 2 修复 Phase 1 bug"、"与一期的差异" | 直接描述当前能力，不提历史 |
| 3 | **Feature 级 ≠ Spec 级**：写 WHAT/WHY，不写 HOW | SQL DDL / API 端点路径 / 文件路径 / line number / 端口 / 库选型 / 状态码 | "项目可保留多次评估记录"、"服务重启已完成报告仍可查" |
| 4 | **粗验收条件在 PRD 阶段就给**：每条 P0/P1 内联 3-5 条行为级验收 | `[ ] POST /api/x 返回 200`（字段级，留给 spec） | `[ ] 评估完成后能看到对应记录入库`（行为级） |

**判断 spec vs PRD 的简单方法**：能用 grep 搜出来的具体字符串（端点 / 表名 / 类名 / 库名），几乎都是 spec 级，**不写**。

---

## Workflow

### 1. Understand the Feature

接受任意输入形式：
- 功能名（"SSO support"）
- 问题陈述（"Enterprise customers keep asking for centralized auth"）
- 用户请求（"Users want to export their data as CSV"）
- 模糊想法（"We should do something about onboarding drop-off"）

### 2. Gather Context

对话式询问，不要一次塞完所有问题。优先级高的先问，缺口边写边补：

- **User problem**：解决什么问题？谁感受到？
- **Target users**：服务哪些 user segment？
- **Success metrics**：怎么算成功？
- **Constraints**：技术约束 / 时间线 / 法规 / 依赖
- **Prior art**：之前试过吗？有现成方案吗？

### 3. Pull Context from Available Sources

主动从下列来源补齐上下文（**有什么用什么，没有就跳过**）：

- **代码库**：现有功能、接口、UI、命名约定（用 Read / Grep / Explore agent 摸）
- **既有文档**：上游需求确认单、设计文档、客户访谈、会议纪要
- **历史 PRD / spec**：同领域 PRD 用作风格参考，但**不要在新 PRD 里引用它们**
- **任务系统 / 知识库**：Notion / Linear / Jira 已有 ticket（如已连接）

**关键**：信息收上来后，把必要部分**抄进 PRD**，而不是放链接让读者去看。

### 4. Generate the PRD

**生成前 self-check**：逐条过 §Core Principles 四条。
**生成后 self-check**：逐条过 §Anti-Patterns 八条。

按以下结构产出（详细写作指南见 [`references/prd-writing-guide.md`](references/prd-writing-guide.md)）：

1. **元信息**：作者 / 日期 / 状态 / 面向读者
2. **Problem Statement**：聚焦真实痛点（量化越好），不是产品介绍。2-3 句话讲清问题、谁受影响、不解决的代价
3. **Goals**：3-5 条 outcome（不是 output）。当 user goal 和 business goal 都存在时**拆两个子表**
4. **Non-Goals**：3-5 条，每条标 rationale
5. **User Stories**：**强制 canonical 格式** "作为 [角色]，我想 [能力]，以便 [收益]"，按 persona 分组
6. **Requirements**：P0 / P1 / P2 三档。**每条 P0/P1 内联 3-5 条粗验收条件**（checkbox 格式，行为级）。P2 单行表格即可
7. **Success Metrics**：Leading + Lagging，每条带具体目标值
8. **Open Questions**：三列表格 `| # | 问题 | 责任方 | 是否阻塞 P0 启动 |`。**只列产品决策**，工程决策（库选型 / 加密算法 / token 时长）不放这
9. **Timeline & Dependencies**：拆三块——时间线 / Hard Deadlines（没有就明说"暂无"）/ 关键依赖
10. **风险与缓解**（可选）
11. **后续动作**：明确指向 `/feature-spec` 派生工程 SPEC

### 5. Review and Iterate

生成后：
- 问用户哪些章节需要调整
- 主动 offer 展开特定章节
- 主动 offer follow-up artifact（design brief / engineering ticket 拆分 / stakeholder pitch）

---

## Anti-Patterns（速查表 — 生成后过一遍）

| # | 反模式 | 通用反例 |
|---|--------|----------|
| 1 | 让读者去看其他材料 | "详见 SSO 设计 doc"、"参考上一版 PRD"、"打开原型站点看" |
| 2 | 把 spec 细节写进 PRD | SQL DDL / API 端点路径（`POST /api/v1/...`）/ 文件路径 / line number / 库选型 / status code |
| 3 | 引用 prior work | "原型阶段..."、"Phase 2..."、"与上一期的差异" |
| 4 | 陈述句替代 user story | ❌ "在导航栏点 Export 下载 CSV"<br>✅ "作为分析师，我想点 Export 下载 CSV，以便离线整理数据" |
| 5 | 单行表格当 P0 写完 | ❌ `\| P0-1 \| 接入 SSO \|` ← 缺 3-5 条粗验收条件 |
| 6 | Open Question 列工程决策 | ❌ "用 RS256 还是 HS256"、"Token 5 分钟够不够"<br>✅ "AI 评估的可见权限范围"、"历史评估能否删除" |
| 7 | Goals 写成 outputs 不是 outcomes | ❌ "build onboarding wizard"<br>✅ "reduce time-to-first-value by 50%" |
| 8 | 编造 Hard Deadline | 没有真实外部约束就明说"暂无外部强约束截止日期" |

---

## Output Format

Markdown，header 清晰。stakeholder 应该读 header 和 bold 文字就能 get 大意。

**章节顺序**：见 §Workflow Step 4 上方的列表。

**不要写的章节**：
- ❌ "与上一版的差异" / "与原型的差异"（违反 Anti-Pattern 3）
- ❌ "Appendix：跳转 URL 协议 / OpenAPI 摘要 / SQL DDL"（这些都是 spec）
- ❌ "技术栈一览" / "代码库一览" / "端口约定"（spec 层）

---

## Tips

- 对 scope 要有 opinion：紧而清晰的 spec 优于宽而模糊的
- 用户想法太大就建议分 phase，spec 第一个 phase
- success metrics 要具体可测，不要"提升用户体验"这种
- non-goals 和 goals 一样重要——防实施期 scope creep
- open questions 要真"open"——不要列你能从上下文回答的问题
- 写完 self-check：把 PRD 假装给完全不知情的开发者，他能看懂 WHAT / WHY 吗？需要看其他材料吗？混入 spec 级细节没？
- 末尾明确指向下一步：`/feature-spec` 派生工程 SPEC（含 SQL DDL / 接口规格 / 跨域协议 / 异常路径 / 权限矩阵）
