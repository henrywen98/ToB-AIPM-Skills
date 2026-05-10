---
name: epic-prd
description: "ToB 私有化项目立项期一次性产物:输出项目骨架 epic-prd.md(9 字段)+ 初始化 feature-backlog.md(Feature 候选清单 SSOT)。用于多功能项目立项时对齐「这个项目都包含哪些 Feature」给客户/销售/老板看。当用户说『写 Epic PRD』『多功能项目立项』『项目立项需求骨架』『/epic-prd』时触发。**单功能项目跳过**,直接走 feature-prd。**不是** authoritative 文档(真正的 authoritative 是 feature-spec)。"
---

# epic-prd — ToB 私有化项目立项 Epic PRD

## Definition

| 项 | 是 | 不是 |
|---|---|---|
| 触发时机 | 多功能项目**立项期一次性**(报价 / SOW 阶段) | 项目跑起来后的持续期(那是 `feedback-triage`) |
| 读者 | 客户 / 销售 / 老板 / PM 自己 | 开发 / QA(他们读 feature-spec) |
| 唯一关键输出 | **Feature 候选清单**(写在 feature-backlog.md) | 字段验收 / UX / 技术方案 |
| 文档地位 | 项目骨架,**非 authoritative** | 不是合同(那是阶段需求确认单)、不是字段权威(那是 feature-spec) |
| 颗粒度 | 一个项目 = 一个 Epic = 一份 backlog | 不跨项目;跨项目 portfolio 视角不在本 skill 范围 |
| 单功能项目 | **跳过**,直接 feature-prd | — |

## 工作流定位

来源:Henry 的 ToB 私有化产品-需求侧工作流第 0 步 + 附录 C。

```
[本 skill] epic-prd
   ↓ 输出 Feature 候选清单(item-01..NN)
feature-prd(每个 item 一个)
   ↓
feature-spec → feature-us → phase-plan → phase-confirm-doc → ...
```

**simplified-Z 数据架构**:
- `feature-backlog.md` 是 Epic 内 SSOT,item-XX 在 Epic 内唯一(对齐工作流附录 G V1 标识符)
- `epic-prd.md` 是 narrative 派生方,Feature 清单引用 backlog 而**不复制**
- 持续期 `feedback-triage` skill 也读写同一份 backlog(append 新 candidate)

## 输入

让用户提供(对话式按需问,不一次问完):
- **项目名 + 客户名**(必填)
- **立项材料**:可以是合同 / SOW 片段 / 客户需求邮件 / 销售调研笔记 / 老系统现状摘要 / 0-1 原型描述
- **甲方对接人 / 法人代表 / 技术负责人**(Stakeholders)
- **合同号 / SOW 引用**(可选,如果还没签也行)
- **客户的明显约束**:私有化部署?国资 / 央企?是否有合规审计要求?

如果用户没主动提,**主动追问 1-2 个最关键的**(不要列清单)。

## 输出 1:`epic-prd.md`(9 字段,~80-120 行)

按下面的固定模板写。**字段不增不减**,每段长度按需要伸缩。

```markdown
---
project: <项目名>
customer: <客户名>
created: YYYY-MM-DD
status: draft  # draft | reviewed | locked
---

# Epic PRD — <项目名>

## 1. Epic Name
<一行,可识别的项目代号,例:杭州产投-FOF 平台 V1>

## 2. Elevator Pitch
For <目标客户/角色> who <核心痛点>,
the <项目名> is a <类别,如「私有化股权投资管理平台」>
that <核心价值,1 句>,
unlike <现状或客户在用的替代方案>.

## 3. Background / Why now
<2-3 段:客户为什么这个时间点要做、什么变化触发的、不做的代价>

## 4. Goals
<1-3 条目标,每条一行;可选附简单成功标准——客户验收口径,不是 SaaS 增长指标>
1. ...
2. ...

## 5. Stakeholders
| 角色 | 姓名 | 备注 |
|---|---|---|
| 甲方对接人 | ... | 日常沟通 |
| 甲方法人代表 | ... | 阶段确认单用印 |
| 甲方技术负责人 | ... | 技术过审参与 |
| 乙方 PM | Henry | 本文档 owner |
| 乙方交付负责人 | ... | 商务签字 |

## 6. 合同 / SOW 引用
- 合同号:<如有>
- 相关条款:<引用条款编号 + 一句话摘要>
- SOW 范围摘要:<本项目交付边界,1-3 段>

## 7. Feature 候选清单
**详见 [`./feature-backlog.md`](./feature-backlog.md)**(item-01 .. item-NN)。

本节不复制 backlog 内容,只做指针。所有 Feature 候选的增删改在 backlog 文件里维护,本文件不动。

## 8. In-Scope / Out-of-Scope
### In-Scope(本项目做)
- ...

### Out-of-Scope(本项目明确不做)
- ...
- <对客户预期管理至关重要,务必写出>

## 9. 风险
<3-5 条核心风险,每条一行>
- 技术风险:...
- 合规风险:...
- 客户预期风险:...
- AI 功能风险(如有):精度 / 召回 / 降级 / 人工复核
- 排期 / 依赖风险:...
```

## 输出 2:`feature-backlog.md`(同时初始化)

跟 epic-prd.md **同目录**输出。Epic 内 Feature 候选的 SSOT。

```markdown
---
project: <项目名>
epic: <项目名>
created: YYYY-MM-DD
last_updated: YYYY-MM-DD
---

# Feature Backlog — <项目名>

## 状态说明
- `committed`:已确定本项目阶段做
- `candidate`:候选,待 PM 决定纳入哪期 / 是否纳入
- `dropped`:不做了(保留记录,扯皮举证用)

## Items

| item | 功能名 | 一句话价值 | 优先级 | 量级 | 分期 | status | 来源 |
|---|---|---|---|---|---|---|---|
| item-01 | <功能短名> | <投资经理一键做 X> | P0 | M | Phase 1 | committed | epic-prd 立项 |
| item-02 | ... | ... | P1 | L | Phase 2 | committed | epic-prd 立项 |
| item-03 | ... | ... | P2 | S | TBD | candidate | epic-prd 立项 |

## 字段说明

- **item-XX**:Epic 内唯一编号(不跨 Epic 全局,对齐工作流附录 G V1 标识符)
- **优先级**:P0(必做)/ P1(应做)/ P2(可做)
- **量级**:S(小,≤5 人天)/ M(中,5-15 人天)/ L(大,>15 人天)。立项期估算粗,不是精确人天
- **分期**:Phase 1 / Phase 2 / TBD(等下一份 phase-plan 细化)
- **status**:committed(立项时已锁定) / candidate(待评估) / dropped(明确不做)
- **来源**:`epic-prd 立项` 或 `feedback-triage YYYY-MM`(后者由 feedback-triage skill 写入)
```

## 工作步骤

1. **理解项目背景**:读用户给的立项材料,问 1-2 个关键问题(读者是谁?客户类型?私有化场景?)。
2. **起草 9 字段 epic-prd.md**:每段先写一稿,产出后让用户逐段确认或修订。
3. **从立项材料中提取 Feature 候选**:列 5-15 个,每个一行。**强制每个 Feature 给一句话价值**——不是技术描述。
4. **填 backlog 表格**:优先级 + 量级 + 分期 + status + 来源。这一步要跟用户对一下取舍。
5. **保存两份文件**:同目录,互引。
6. **下一步提示**:告诉用户「下一步可以对每个 P0 item 跑 `/feature-prd` 出业务 PRD」。

## 与 feedback-triage 的关系

- 立项时 epic-prd **创建** backlog,所有 item 来源标记为 `epic-prd 立项`,默认 status `committed` 或 `candidate`(看你立项时锁定程度)。
- 项目跑起来后客户提的新需求(不在当前 SOW 范围) → `feedback-triage` skill **append** 到同一份 backlog,来源标记 `feedback-triage YYYY-MM`,默认 status `candidate`。
- 下一轮项目立项 / 续约时,PM 看 backlog 里所有 candidate,挑出来纳入新一轮 epic-prd —— 闭环。

## 边界(明确不做)

- 不出 UX / 原型 / 技术方案(那是 feature-prd / feature-spec / 技术经理)
- 不写字段验收 / schema / API / 状态机(那是 feature-spec)
- 不做估精确人天(那是 phase-plan)
- 不做合同签字(那是 phase-confirm-doc)
- 不写"Authoritative Specification"——Epic 是项目骨架,不是权威字段定义

## 单功能项目跳过

如果用户的项目就一个功能(没有"多个 Feature 取舍"的诉求),**不要跑本 skill**。直接告诉用户:「你这是单功能项目,跳过 Epic,直接 `/feature-prd`」。

判定:用户能用一句话说清楚要做什么、且没有"先做哪个后做哪个"的取舍 → 单功能。
