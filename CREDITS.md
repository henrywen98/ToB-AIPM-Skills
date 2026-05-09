# 归属与致谢 / Credits

本仓库是 ToB 私有化产品 PM 工作流的 Claude Code marketplace，由两个 plugin 组成：`tob-aipm-skills`（10 个 skill 集合）与 `code-to-prd`（独立工具）。绝大多数 skill 来自社区上游，按 MIT 许可证保留原作者署名；少数为本仓库原创。

## Skill 来源一览

| 现名 | 旧名 | 上游来源 | License | 改造范围 |
|---|---|---|---|---|
| `epic-prd` | `create-prd` | [phuryn/pm-skills](https://github.com/phuryn/pm-skills) (`pm-execution`) | MIT © Pawel Huryn | 仅改 `name` 与 `description`，正文未动 |
| `feature-prd` | `write-spec` | [phuryn/pm-skills](https://github.com/phuryn/pm-skills) (knowledge-work-plugins / product-management) | MIT © Pawel Huryn | 仅改 `name` 与 `description`，正文未动 |
| `feature-spec` | `derive-spec` | **原创**（henrywen98） | MIT © henrywen98 | 全新起草 + 9 步 checklist + user gate 段落 |
| `feature-us` | `user-stories` | [phuryn/pm-skills](https://github.com/phuryn/pm-skills) (`pm-execution`) | MIT © Pawel Huryn | 仅改 `name` 与 `description`，正文未动 |
| `phase-plan` | `sprint-plan` | [phuryn/pm-skills](https://github.com/phuryn/pm-skills) (`pm-execution`) | MIT © Pawel Huryn | 仅改 `name` 与 `description`，正文未动 |
| `phase-confirm-doc` | — | **原创**（henrywen98，模板灵感参考 `purvar-prd/commands/purvar.confirm.md`） | MIT © henrywen98 | 极简版：本期 + 不做什么 + footer 锚版本 |
| `test-scenarios` | `test-scenarios` | [phuryn/pm-skills](https://github.com/phuryn/pm-skills) (`pm-execution`) | MIT © Pawel Huryn | 仅改 `description`，正文未动 |
| `dummy-dataset` | `dummy-dataset` | [phuryn/pm-skills](https://github.com/phuryn/pm-skills) (`pm-execution`) | MIT © Pawel Huryn | 仅改 `description`，正文未动 |
| `md-to-docx` | `md-to-docx` | henrywen98 私有 plugin `purvar-hld` | MIT © henrywen98 | 仅改 `description`；pandoc 后处理脚本保留 |
| `brainstorming` | `brainstorming` | [obra/superpowers](https://github.com/obra/superpowers) | MIT © Jesse Vincent | 完全未改（被 `feature-spec` 渐进对话依赖） |
| `code-to-prd` | `code-to-prd` | [alirezarezvani/claude-skills](https://github.com/alirezarezvani/claude-skills/tree/main/product-team/code-to-prd) | MIT © Alireza Rezvani | 完全未改（整插件复制，含 commands + scripts） |

## License 说明

- **本仓库根 `LICENSE`**：MIT © 2026 henrywen98 —— 覆盖 marketplace 元数据与原创 skill。
- **`plugins/tob-aipm-skills/LICENSE`**：合并 MIT 通知，列出本仓库 + Pawel Huryn + Jesse Vincent 三方版权。
- **`plugins/code-to-prd/LICENSE`**：上游 MIT 原文，保留 © Alireza Rezvani。

如发现遗漏或署名错误，欢迎提 issue。
