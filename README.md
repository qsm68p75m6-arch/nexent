# Nexent 赛题3：可进化决策智能体（金融·对外担保披露核验）

华为 ModelEngine Nexent 赛题 3 项目仓库。目标：在 Nexent 平台上搭建一个面向"上市公司对外担保披露核验"的决策智能体，用跨文档证据支撑可追溯的核验结论。

## 当前状态（2026-11-07）

- 可行性验证（POC）已通过 C01-C07（合成资料），C08 已备好未跑——逐条判读见 [docs/progress.md](docs/progress.md)
- 未开始：图片理解轮、MCP 工具与 Skill 编排、真实公开资料、本体演化对照实验
- 一个月任务规划与三人分工：[docs/plan.md](docs/plan.md)

## 目录

| 目录 | 内容 |
|---|---|
| docs/ | 进度真相源（progress.md）、任务规划（plan.md）、设计文档六节（design/）、交接模板（HANDOFF_TEMPLATE.md） |
| poc-kit/ | POC 验证包：8 份合成 PDF（D01-D08）、8 道题（C01-C08）、Agent 提示词、轮次记录模板。从 [poc-kit/START_HERE.md](poc-kit/START_HERE.md) 读起 |
| poc-review/ | 2026-10-24 复核记录：每轮判定、失败诊断（C07 工具调度失败案例在这里，答辩素材） |
| evidence/ | 实验证据归档，命名规范见其中 README |
| real-data/ | 真实上市公司公告评测集（沪光股份 605333 等）：公告 PDF + 评测题 + 预期答案 + 知识库绑定表 |
| configs/ | 平台配置导出（Agent json、知识库清单、MCP 配置）。只放脱敏模板，**密钥严禁入库** |
| context/ | Codex 阶段完整会话导出，新成员 / 新 agent 的背景阅读材料 |

## 新成员 / 新 agent 上手顺序

1. 读本 README；
2. 读 [docs/progress.md](docs/progress.md)（做到哪了）；
3. 读 [context/codex_sessions_2026-10.txt](context/codex_sessions_2026-10.txt)（方案来龙去脉）；
4. 读 [poc-kit/START_HERE.md](poc-kit/START_HERE.md)（验证包怎么用）；
5. 接手前看 [docs/HANDOFF_TEMPLATE.md](docs/HANDOFF_TEMPLATE.md)，走交接流程。

## 协作规矩（三条）

1. **仓库是唯一真相源**：agent 会话里聊出的结论，不落盘就不算数。每完成一个工作包，在 docs/progress.md 加一行（包号 / 结论 / 证据路径）。
2. **证据进 evidence/**：原始回答、检索片段、截图按 `evidence/<题号>_<日期>/` 归档，原始记录不清洗；失败记录同样归档。
3. **换手写 HANDOFF**：换人或换 agent 接手，旧手按模板写交接、新手先读再动。

## 安全红线

configs/ 与任何提交里**不得出现 API key、密码、token**（本仓库公开）。真实密钥只放本地 .env（已被 .gitignore 忽略），configs/ 只放 .env.example 这类只含键名的模板。
