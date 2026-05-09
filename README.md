# ToB-AIPM-Skills

ToB 私有化产品 PM 工作流的 Claude Code **plugin marketplace**，覆盖从 epic 立项到 QA 移交的完整链路。

> Marketplace 名：`tob-aipm` ｜ 维护者：[@henrywen98](https://github.com/henrywen98)

## 安装

```text
# 1. 添加 marketplace
/plugin marketplace add henrywen98/ToB-AIPM-Skills

# 2. 安装 PM 工作流 skill 集合（11 个 skill 一次到位，含 code-to-prd）
/plugin install tob-aipm-skills@tob-aipm
```

本地开发 / 调试可以指向当前目录：

```text
/plugin marketplace add ./ToB-AIPM-Skills
```

## Marketplace 一览

| Plugin | 说明 | License |
|---|---|---|
| [`tob-aipm-skills`](./plugins/tob-aipm-skills) | ToB PM 工作流 skill 集合：epic-prd / feature-prd / feature-spec / feature-us / phase-plan / phase-confirm-doc / test-scenarios / dummy-dataset / md-to-docx / brainstorming / code-to-prd | MIT |

## `tob-aipm-skills` 工作流节点

| # | 工作流节点 | Skill | 状态 |
|---|---|---|---|
| 0 | Epic 前置（可选） | `epic-prd` | ✅ |
| 1 | 现状测绘（反向出 PRD） | `code-to-prd` | ✅ |
| 2 | 功能 PRD | `feature-prd` | ✅ |
| 3 | 功能 SPEC | `feature-spec` | ✅（已去 SPEC.docx） |
| 4 | 拆用户故事 | `feature-us` | 🔧 待加 V1 条目编号 |
| 5 | 阶段估人天 | `phase-plan` | 🔧 sprint→phase 模型 + 阶段 ID + 人天 |
| 6 | 阶段需求确认单 | `phase-confirm-doc` | ✅（极简版：本期 + 不做什么 + footer 锚版本） |
| 7 | 测试用例 | `test-scenarios` | ✅ |
| 8 | 测试数据 | `dummy-dataset` | ✅ |
| 工具 | docx 转换 | `md-to-docx` | ✅ |
| 依赖 | 渐进对话 | `brainstorming` | ✅（`feature-spec` 底层依赖） |

## 命名规律

- `epic-` 前缀：跨多功能战略层
- `feature-` 前缀：单功能层（PRD / SPEC / US）
- `phase-` 前缀：单阶段层（计划 / 确认单）
- 工具型 skill 保留通用名（`code-to-prd` / `dummy-dataset` / `md-to-docx` / `brainstorming`）

## 目录结构

```
ToB-AIPM-Skills/
├── .claude-plugin/marketplace.json     # marketplace 入口
├── LICENSE                             # MIT © 2026 henrywen98
├── CREDITS.md                          # 各 skill 上游署名
├── README.md
└── plugins/
    └── tob-aipm-skills/
        ├── .claude-plugin/plugin.json
        ├── LICENSE                     # 合并 MIT 通知（含上游署名）
        ├── commands/                   # /code-to-prd 命令
        └── skills/                     # 11 个 skill
```

## 归属与许可

绝大多数 skill 来自社区上游（MIT），少数为本仓库原创。完整署名表见 [`CREDITS.md`](./CREDITS.md)。

- 根 `LICENSE`：MIT © 2026 henrywen98
- `plugins/tob-aipm-skills/LICENSE`：合并 MIT 通知（henrywen98 + Pawel Huryn + Jesse Vincent + Alireza Rezvani）

## 待办

### 🔧 改造（已迁移，需调整逻辑）

- [ ] **phase-plan** —— sprint→phase 模型转换：输出条目编号（item-01/02/03） + 阶段 ID + 人天估算
- [ ] **feature-us** —— 加 V1 条目编号逻辑（每份阶段确认单内唯一即可）
- [ ] **feature-spec** —— 收尾旧名引用更新（`write-spec` / `user-stories` / `derive-spec` → `feature-prd` / `feature-us` / `feature-spec`）

### 🟡 可选

- [ ] 各 skill 正文中文化（不影响功能，纯阅读体验）
