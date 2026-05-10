---
name: feedback-triage
description: "ToB 私有化项目持续期客户反馈分流 + cluster + triage。吃进客户邮件/IM/会议汇总,**先分流**「这是变更(L1-L4)还是新 Feature 候选」,然后对新 Feature 候选做 cluster,**写回 feature-backlog.md**(append 新 candidate)。**不**改 SPEC、不出确认单、不动用户故事——只输出 triage-report + 写回 backlog,后续动作由 PM 或其他 skill 接力。当用户说『客户反馈整理』『需求分流』『triage』『把这周邮件归类』『/feedback-triage』时触发。fork 自 pm-product-discovery 的 analyze-feature-requests + triage-requests,大改适配 ToB 工作流。"
---

# feedback-triage — ToB 私有化客户反馈分流 + Triage

## Definition

| 项 | 是 | 不是 |
|---|---|---|
| 触发时机 | 项目跑起来后**周期性**(每周/每月/客户催/攒够一批) | 项目立项期(那是 `epic-prd`) |
| 输入 | 散乱客户原话(邮件、IM、会议汇总、飞书表) | 已经分类好的需求清单 |
| 输出 | `triage-report-YYYY-MM.md` + **append 写回** `feature-backlog.md` | SPEC / 确认单 / 用户故事 |
| 核心职责 | **分流**(变更 vs 新 Feature)+ **cluster**(同主题聚类)+ **triage**(候选档位 / 优先级建议) | 不做执行(不改 SPEC、不签字) |
| 跟工作流 6 节关系 | 是 6 节 L1-L4 变更管理的**前置分流器**;并填补 6 节没覆盖的"新 Feature 候选 → backlog" | 不替代 6 节,L1-L4 执行流程沿用 6 节原文 |

## 工作流定位

```
客户原话(邮件/IM/会议)
   ↓
[本 skill] feedback-triage
   ↓ 三段输出
   ├── 1. 变更类(L1-L4)→ PM 走工作流第 6 节流程
   ├── 2. 新 Feature 候选 → append 到 feature-backlog.md(status: candidate)
   └── 3. 模糊待复核 → PM 优先看
```

## 输入

让用户提供:
- **客户原话**:
  - 直接 paste(邮件正文 / IM 截图文字 / 会议汇总)
  - 上传文件(.md / .txt / .csv / Excel)
  - 飞书表导出
- **本项目上下文**(读取):
  - 当前项目 `epic-prd.md`(看 SOW 范围 / Out-of-Scope)
  - 当前项目 `feature-backlog.md`(看已有 item-XX,判断变更归属)
  - 工作流文档第 6 节 L1-L4 判定规则(本 skill 内置 prompt 已包含,无需用户提供)

如果用户没指明上下文,主动问:**「这批反馈是哪个项目的?」**——分流必须基于具体项目的 SOW 范围。

## L1-L4 判定规则(内置)

来自 Henry 工作流第 6.1 节,本 skill 据此分类:

| 档位 | 判定规则 | 例子 | 提示 PM 的下一步 |
|---|---|---|---|
| **L1 微调** | 只改文案、标签、展示顺序、提示语;不改行为、验收、schema、状态、权限、测试口径 | 「上传」改「提交」 | 直接改 SPEC;**不签字**(SPEC 升次版本) |
| **L2 小变更** | 既有功能内技术细节变化,不改业务级验收 | 加非必填备注字段、校验阈值调整 | 进变更池累积,**直接合并**进下一份阶段需求确认单的"本期要做的"清单;**不单独签** |
| **L3 中变更** | 新增功能,或改业务级验收/流程/权限/验收口径 | 新增 BP 自动评分;上传后立即可见改为审核后可见 | 默认**即时单独签**增量需求确认单;客户邮件豁免可合并下份 |
| **L4 大变更** | 推翻目标 / 不做什么,或扩大 SOW 范围 | 从投后管理扩到投前尽调 | 走 SOW 变更条款;重型 CCB |
| **新 Feature 候选** | **不在当前 SOW 范围**,客户希望"以后做" | 客户提"能不能加 LP 对账模块"(本期不在范围) | append 到 backlog,status: candidate;**不立即处理** |

**判别要点**:
- 看 epic-prd.md 的 In-Scope / Out-of-Scope —— 在 In-Scope 内调整 = L1-L4 变更;否则 = 新 Feature 候选
- 看 backlog 已有 item —— 跟某个 item 强相关 = 那个 item 的 L1-L4 变更;弱相关或无关 = 新 Feature 候选
- 模糊不能定 → 进第三段「待 PM 复核」

## 输出 1:`triage-report-YYYY-MM.md`

```markdown
---
project: <项目名>
period: YYYY-MM(或 YYYY-MM-DD ~ YYYY-MM-DD)
triaged_at: YYYY-MM-DD
total_items: <总条数>
---

# Feedback Triage Report — <项目名> · <period>

## 摘要
- 总反馈条数:N
- 变更类:N(L1: a, L2: b, L3: c, L4: d)
- 新 Feature 候选:N(cluster 后 X 个主题)
- 待 PM 复核:N
- 主要来源客户:...

---

## 1. 变更类(在已有 Feature 范围内调整)

### L1 微调(直接改 SPEC,不签字)
| # | 客户原话(摘) | 来源 | 关联 item | AI 判定理由 |
|---|---|---|---|---|
| 1 | "登录页那个『提交』改成『确认』" | 王经理 邮件 2026-05-03 | item-02 BP 上传 | 文案改动,不动行为 / 验收 / schema |

→ **PM 下一步**:批量改 SPEC,SPEC 升次版本;不出确认单。

### L2 小变更(进变更池,合并下份阶段确认单)
| # | 客户原话 | 来源 | 关联 item | AI 判定理由 |
|---|---|---|---|---|
| ... | | | | |

→ **PM 下一步**:进变更池,下一份阶段需求确认单清单加上。

### L3 中变更(默认即时单独签增量确认单)
...

### L4 大变更(走 SOW 变更)
...

---

## 2. 新 Feature 候选(SOW 范围外,append 到 backlog)

### 主题 A:<聚类主题名,如「LP 季度对账」>
- 来源原话:
  - "我们 LP 那边每季度对账要 3 天,能不能..." — 张总监 IM 2026-05-10
  - "对账数据能不能直接从托管行 API 拉?" — 李经理 会议纪要 2026-05-15
- AI 建议优先级:P1
- AI 建议量级:L
- **写回 backlog**:item-NN(已 append)

### 主题 B:<...>
...

→ **PM 下一步**:看 backlog 的新 candidate,决定是否纳入下一轮 epic-prd / 续约 SOW。

---

## 3. 模糊 / 待 PM 复核

| # | 客户原话 | 来源 | 模糊原因 |
|---|---|---|---|
| 1 | "你们这个 AI 评分准确率不行啊" | 王经理 会议吐槽 2026-05-20 | 没说要怎么改,可能是 L1 提示语 / L3 算法升级 / 投诉,需 PM 跟进 |

→ **PM 下一步**:优先处理模糊条目,补充上下文后回到本 skill 重跑或手动归档。

---

## 模式 / 洞察(可选)
- <跨主题观察:比如「客户反复提对账,可能反映他们财务流程痛点而不是平台问题」>
- <段-客户模式:某个客户特别集中提某类需求>
- <矛盾反馈:不同客户提相反诉求>
```

## 输出 2:写回 `feature-backlog.md`(append)

对每个新 Feature 候选 cluster,append 一行到 backlog:

```markdown
| item-NN | <主题 A 名> | <一句话价值> | <AI 建议 P 级> | <AI 建议量级> | TBD | candidate | feedback-triage YYYY-MM |
```

**契约**:
- **append 不覆盖**:不动 backlog 已有行
- **item-NN 顺序续编**:读 backlog 现有最大 item 号 + 1
- **status 强制 `candidate`**:status 不是 committed 也不是 dropped——交给 PM 后续决定
- **来源强制写 `feedback-triage YYYY-MM`**:可追溯回 triage-report
- **更新 frontmatter `last_updated`**:今天日期

## 工作步骤

1. **接受反馈输入**:paste / 文件 / CSV。如无输入,问用户。
2. **确认项目上下文**:问用户「这批反馈是哪个项目的?」,读对应 epic-prd.md + feature-backlog.md。
3. **逐条解析 + 分流**:对每条原话,AI 判定 L1/L2/L3/L4/新Feature/模糊,给候选档 + 一句理由。**不要让用户一条一条确认**——批量给出,PM 复核异常项即可。
4. **新 Feature cluster**:把所有「新 Feature 候选」按主题聚类。**聚类阈值**:同主题至少 2 条来源(避免 1 条孤立反馈就开 item);1 条但客户明确「这是个 feature 不是变更」也可以单开。
5. **写 triage-report**:三段齐全。
6. **append backlog**:把新 cluster 写回 backlog,status: candidate。
7. **下一步提示**:
   - "L1 变更 N 条,要我直接生成 SPEC 修改清单吗?"
   - "L3 中变更 N 条,要我起草增量需求确认单吗?"(调用 `/phase-confirm-doc`)
   - "新 Feature 候选 X 个已写回 backlog,下次立项可纳入"

## 边界(明确不做)

- **不**修改 SPEC(只**提示** PM 该改 SPEC)
- **不**出阶段需求确认单 / 增量确认单(那是 `phase-confirm-doc`)
- **不**改用户故事
- **不**做客户沟通(回邮 / 答复)
- **不**判 SOW 是否要变(L4 标出,**PM + 交付侧** 决定)

对齐工作流 6.4 节:**AI 主导文档工作和客观判断,PM 主导沟通和合同动作。**

## 跟原 fork 源(triage-requests / analyze-feature-requests)的差异

| 维度 | 原 skill(SaaS 通用) | 本 skill(ToB 私有化) |
|---|---|---|
| 输出 | 1 份 triage report,P1-P4 优先级 | 三段(变更 / 新 Feature / 模糊),分流是核心 |
| 上下文 | 产品 OKR、用户 segment、增长指标 | 项目 SOW 范围、合同条款、L1-L4 判定规则 |
| 优先级框架 | Opportunity Score、Impact/Effort/Risk | L 档位(已有功能)+ 新 Feature 写回 backlog |
| 写回数据源 | 无(只是报告) | append 到 feature-backlog.md(SSOT) |
| 客户视角 | MAU、留存、churn | 单客户深度、合同验收、阶段交付 |
| 接力下游 | user stories / experiments | feature-spec(L1-L4 变更)/ 下一轮 epic-prd(新 Feature) |

## 关联

- **上游 epic-prd**(立项时创建 backlog)
- **同读 feature-backlog.md**(本 skill append 写入)
- **下游提示**:`feature-spec`(L1)、变更池(L2-L4)、`phase-confirm-doc`(L3 增量签)、下一轮 `epic-prd`(新 Feature 候选)
