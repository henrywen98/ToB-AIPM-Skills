# PRD Writing Guide（详细参考）

主 skill 入口见 [`../SKILL.md`](../SKILL.md)。本文档收录详细写作指南——各章节的深入展开、模板、常见错误。主 SKILL.md 给出原则和速查表，本指南给细节。

---

## Problem Statement

- 2-3 句话讲清用户问题
- 谁受影响、出现频率、量化（"月均 N 份"、"X% 用户"）越好
- 不解决的代价：用户痛点、商业影响、竞争风险
- 基于证据：用户研究、客服数据、指标、客户反馈

**反例**："系统不够好用，需要改进。"
**正例**："企业管理员每月人工同步用户名单耗时 8-10 小时；忘记吊销已离职员工导致 SOC2 审计风险。"

---

## Goals

- 3-5 条 outcome（不是 output）
- 每条回答："How will we know this succeeded?"
- Outcome（终态）vs Output（动作）：
  - ❌ "build onboarding wizard"（动作）
  - ✅ "reduce time-to-first-value by 50%"（终态）

### User Goals vs Business Goals

当两者都存在时，**分两个子表**：

- **User Goals**：用户获得什么、衡量方式聚焦用户行为指标
- **Business Goals**：项目交付价值、商业指标（验收 / 续约 / 收入 / 占比）

单一类型时（纯内部工具或纯商业目标）不需要硬拆。

---

## Non-Goals

- 3-5 件本期明确不做的事
- 邻近能力但本版本不在范围内
- **每条标 rationale**：影响不大 / 复杂度过高 / 独立 initiative / 时机未到
- 防止实施期 scope creep + 与 stakeholder 设期望

---

## User Stories

### 强制 canonical 格式

```
作为 [具体角色]，我想 [能力描述]，以便 [收益/动机]
```

英文等价：`As a [user type], I want [capability] so that [benefit]`

### 好的 user story 标准（INVEST）

- **Independent**：可独立开发交付
- **Negotiable**：细节可讨论，story 不是合同
- **Valuable**：交付价值给用户（不只是团队）
- **Estimable**：团队可粗略估工
- **Small**：一个 sprint 内可完成
- **Testable**：有明确的验证方式

### 常见错误

- **太泛**："As a user, I want the product to be faster" — 具体什么要快？
- **方案规定**："As a user, I want a dropdown menu" — 描述需求，不是 UI 控件
- **缺收益**："As a user, I want to click a button" — 为什么？做什么？
- **太大**："As a user, I want to manage my team" — 拆成具体能力
- **内部视角**："As the engineering team, we want to refactor the database" — 这是任务不是 user story
- **角色太泛**："As a user" 太泛 → 用具体 persona（"作为团队管理员"、"作为系统管理员"、"作为新用户"）

---

## Requirements

### 三档分类

- **Must-Have (P0)**：本版本不可缺。问："砍掉它还能解决核心问题吗？"——不能则 P0
- **Nice-to-Have (P1)**：显著改善体验，但核心 use case 没有也能跑。常作 fast-follow
- **Future Considerations (P2)**：本版明确不做但要为之留位，避免做出阻碍未来的架构决策

### P0 / P1 写作格式（强制）

每条用以下结构（**不要单行表格**）：

```
**P0-X 一句话标题（描述能力，不是实现）**
- [ ] 粗验收条件 1（行为级，用户可观察）
- [ ] 粗验收条件 2
- [ ] 粗验收条件 3
（3-5 条；P1 1-2 条即可）
```

P2 可用单行表格（架构留位无需 criteria）：`| # | 项 | 说明 |`

### P0 拆块（推荐）

P0 涉及多个子系统时（前后端 + 第三方集成）可拆 (A) / (B) 两块，标注**可否并行 / 联调依赖**。

### 验收条件原则

- ✅ 行为级 / 用户可观察："点击按钮 1 秒内跳转新 tab"
- ❌ 字段级 / 接口级（spec 层级，不写）："`POST /api/xxx` 返回 200"
- ✅ 可二元判断："列头可按总分升降排序"
- ❌ 形容词："快速"、"用户友好"、"性能优秀"——定义具体行为
- ✅ 含负面/边界条件："无该按钮权限的用户看不到按钮"、"未评估项目显示'—'"
- ✅ 包含 happy path + error case + edge case

### MoSCoW Framework（参考映射）

- **Must have** → P0
- **Should have** → P1
- **Could have** → P1（低优先级）
- **Won't have (this time)** → Non-Goal

### 分档建议

- 对 P0 要狠：must-have list 越紧越快交付
- 如果什么都是 P0，就什么都不是 P0
- P1 应该是确信会很快建的，不是 wish list
- P2 是"架构保险"——指引设计决策，即使本期不做

---

## Success Metrics

### Leading Indicators（短周期，天到周）

- **Adoption rate**：合格用户中有多少试用了
- **Activation rate**：完成核心动作的比例
- **Task completion rate**：成功完成目标的比例
- **Time to complete**：核心流程耗时
- **Error rate**：用户撞错或死路的频次
- **Feature usage frequency**：用户回归使用频率

### Lagging Indicators（长周期，周到月）

- **Retention impact**：是否提升留存
- **Revenue impact**：是否带升级 / 扩展 / 新收入
- **NPS / 满意度变化**：用户感受是否改善
- **Support ticket reduction**：是否降低支持负担
- **Competitive win rate**：是否帮赢更多 deal

### Setting Targets

- 目标要具体："50% adoption within 30 days" 而不是 "high adoption"
- 基于可比 feature / 行业基准 / 显式假设设定
- 设"success"阈值 + "stretch"目标
- 定义测量方法：什么工具 / 什么 query / 什么时间窗
- 指定评估时点：1 周 / 1 月 / 1 季度后

---

## Open Questions

**强制三列表格**：`| # | 问题 | 责任方 | 是否阻塞 P0 启动 |`

- 每条标责任方（工程 / 设计 / 法务 / 数据 / 客户 PM / 算法 / 运维）
- "是否阻塞 P0 启动"列必填：
  - **是**（必须先闭环才能启动 P0）
  - **否（默认 X 走着，待 Y 确认）**——明确**默认值** + **确认人**
- ❌ 不列工程决策（库选型 / 加密算法 / token 时长）—— spec 层
- ✅ 列产品决策（业务规则 / 权限边界 / 数据保留策略 / 评估可见性）

---

## Timeline & Dependencies

拆三块：

### 1. 时间线

Sprint 排期（建议 N 周 M sprint 粒度，每个 sprint 一句话内容）。

### 2. Hard Deadlines

外部强约束截止日期：合同承诺 / 客户演示日 / 合规截止 / 大型展会。

**没有就明说"暂无外部强约束截止日期"**——不要凭空编。

### 3. 关键依赖

- 客户 PM：哪些 Open Question 必须在哪个节点闭环
- 运维：环境 / 域名 / 证书 / 配额
- 工程团队：跨团队接手的 P0 条目
- 第三方：API 配额 / 协议对齐

---

## Acceptance Criteria 写法（PRD 用粗版，spec 用细版）

### Given/When/Then 格式

```
Given [precondition or context]
When [action the user takes]
Then [expected outcome]
```

示例：
- Given 管理员已为组织配置 SSO
- When 团队成员访问登录页
- Then 自动重定向到组织的 SSO provider

### Checklist 格式（PRD 推荐）

```markdown
- [ ] 管理员可在组织设置填 SSO provider URL
- [ ] 团队成员在登录页看到"用 SSO 登录"按钮
- [ ] SSO 登录创建新账号（如未存在）
- [ ] SSO 登录链接到已存在账号（email 匹配时）
- [ ] SSO 失败时显示明确错误信息
```

### Tips

- 覆盖 happy path、error case、edge case
- 描述期望行为，不写实现
- 包含什么**不该发生**（negative test case）
- 每条独立可测
- 避免歧义词："fast"、"user-friendly"、"intuitive"——定义具体含义

---

## Scope Management

### Scope Creep 信号

- PRD 批准后 requirement 一直加
- "小"改累积成大项目
- 团队建着没人要的功能（"顺便..."）
- launch date 一直推但没显式 re-scope
- stakeholder 加 requirement 但不删

### 预防 Scope Creep

- 每份 PRD 写明 non-goals
- 加 scope 必须删 scope 或推 timeline
- PRD 里清晰区分 v1 vs v2
- 对照原始 problem statement 审 PRD：每件事都服务于它吗？
- 探索 time-box："2 天搞不定 X 就砍"
- 建"parking lot"放好但不在范围内的想法

---

## Tips

- 对 scope 要有 opinion：紧而清晰的 PRD 优于宽而模糊的
- 用户想法太大就建议分 phase，PRD 写第一个 phase
- success metrics 要具体可测，不要"提升用户体验"这种
- non-goals 和 goals 一样重要——防实施期 scope creep
- open questions 要真"open"——不要列你能从上下文回答的问题
- 写完 self-check：假装把 PRD 给完全不知情的开发者，他能看懂 WHAT / WHY 吗？需要看其他材料吗？混入 spec 级细节没？
