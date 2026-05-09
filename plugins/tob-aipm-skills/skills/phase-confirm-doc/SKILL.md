---
name: phase-confirm-doc
description: |-
  生成客户用印的阶段需求确认单（极简版）：本期要做什么 + 不做什么 + 双方签字 + footer 锚 SPEC 版本号。
  当用户说"出阶段确认单"、"生成需求确认单"、"客户签字单"、"/phase-confirm-doc"时触发。
  也用于增量需求确认单（阻断 L3 变更触发，标题改"增量需求确认单 - 因 CR-XXX"）。
  下游：调 md-to-docx 转 docx，交付给客户用印。
---

# 阶段需求确认单（phase-confirm-doc）

生成客户用印的阶段需求确认单 docx。**极简版客户面**：本期范围 + 不做什么 + 签字 + footer 锚版本。不抄 SPEC 内容、不附变更附件、不内嵌 SPEC 快照——客户拿到的就是一页可签字的合同范围确认。

## 设计原则

1. **客户视角和 PM 视角分离**：客户只看本期做啥；版本追溯靠末尾一行 footer
2. **极简单页**：避免客户在多页文档里迷路；附件、变更来源、SPEC 字段细节都不放主文档
3. **footer 是事后扯皮的退路**：footer 写 `SPEC 版本 v1.4 (commit abc1234)`，几个月后扯皮调出 commit 复原 SPEC 全貌
4. **变更隐式合并**：中途客户提的 L2 / 非阻断 L3 变更，直接溶进"本期要做的"清单，不显式标"这是 CR 转化的"。客户回头核对靠 PM 内部变更池档案 cross-ref

## 输入

| 项 | 说明 | 来源 |
|---|---|---|
| 阶段计划 | 本期条目清单（编号 + 描述）+ 期数 | `phase-plan` 输出 |
| 不做什么 | 本期 Non-Goals 明确剔除项 | PM 整理（从 PRD Non-Goals + 历史变更池筛） |
| 公司名 / 项目名 | 文档抬头 | PM 项目档案 |
| SPEC 仓库快照 | 当前 SPEC 全部最新版本对应的 git commit hash | `git rev-parse HEAD`（在 SPEC repo 里跑） |
| 语义版号 | PM 手动维护 | SPEC frontmatter 或 PM 项目档案 |

## 输出

- markdown 草稿：`<项目工作目录>/confirm-docs/phase-<N>-confirm.md`
- docx：调 `md-to-docx` 转换，输出到 `<项目工作目录>/confirm-docs/phase-<N>-confirm.docx`
- 客户用印后，docx 归档到合同档案

## 增量需求确认单（变体）

阻断 L3 变更（影响条目数 ≥30%）触发时，本 skill 用 `--incremental --cr-id <CR-XXX>` 模式生成增量确认单：

- 标题改"增量需求确认单 - 因 CR-XXX 触发"
- "本期要做的"列受影响条目的调整后状态
- "不做什么"写明"由原合同范围调整"
- footer 仍写当前 SPEC 仓库快照

## 6 步 Checklist

按顺序执行，每步创建 task：

1. **读阶段计划** —— 拿到条目清单 + 期数（来自 `phase-plan` 输出）
2. **整理不做什么** —— PRD Non-Goals + 变更池剔除项 + PM 主观判断
3. **取仓库快照锚** —— 在 SPEC repo 里跑 `git rev-parse --short HEAD`，读 SPEC frontmatter 语义版号
4. **生成 markdown 草稿** —— 套下面模板填充
5. **self-review 4 项** —— 条目齐？Non-Goals 写没写？抬头对吗？footer 锚版本填了吗？
6. **user gate + 调 md-to-docx** —— 用户 approve 后调 `md-to-docx` 转 docx，归档

## markdown 模板

```markdown
# 阶段需求确认单 - 第 <N> 期

**<公司名> · <项目名>**

## 本期要做的

- **item-01**: <条目描述>
- **item-02**: <条目描述>
- **item-03**: <条目描述>

## 不做什么

- 本期不含 <Non-Goal-1>
- 本期不含 <Non-Goal-2>

## 双方签字

| 甲方（客户） | 乙方（我方） |
|---|---|
| 签字：________________ | 签字：________________ |
| 日期：________________ | 日期：________________ |

---

本期对应 SPEC 仓库快照：v<语义版号> (commit <git-hash>)
```

## Self-Review 4 项

写完 markdown 后立即检查，不要等用户提：

1. **条目完整性** —— 阶段计划里所有条目都列了吗？
2. **Non-Goals 写了吗** —— 即使没什么要剔除的，也要写"本期不含 X"或显式"本期无明确剔除项"
3. **抬头正确** —— 公司名 / 项目名 / 期数对吗？错了客户一眼看到
4. **footer 锚** —— SPEC 版本号 + commit hash 都填了吗？没填就不能签字

## User Gate

向用户呈现：

```
确认单 markdown 已写到 <path>，self-review 已过。请审阅：
- 本期条目齐吗？
- Non-Goals 漏没漏剔除项？
- 抬头公司名 / 期数 / 项目名对吗？

确认后我调 md-to-docx 转 docx，请交付侧准备用印。
```

等用户回答。改 → 重 self-review → 再问。

## 档位降档

| 档位 | 主签字载体 | 模板差异 | 备注 |
|---|---|---|---|
| **简化版** | 邮件 | 本 skill 改输出 markdown 段落给 PM 复制到邮件，不出 docx | 中国合同法承认电子合同效力，但邮件正文得明确写出三要素：条目 / 不做什么 / 版本号 |
| **标准版** | docx 用印（默认） | 标准模板 | 本 skill 默认形态 |
| **严格版** | docx 用印 + 阶段确认附件 + 审计签收 | 标准模板 + 多盖章页 | 国资 / 央企 / 法务审计场景 |

## 设计来源

- ToB 工作流文档第 5.5 节"确认单怎么签"
- ToB 工作流文档第 6.1 节"变更分级"——本 skill 处理标准期 + 阻断 L3 增量两种触发
- 旧版参考：`tob_pm_skills/plugins/purvar-prd/commands/purvar.confirm.md`
