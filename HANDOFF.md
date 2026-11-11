# HANDOFF — Nexent 赛题3 项目交接

> 最后更新：2026-11-11（zcode 侧边会话收尾时产出）
> 读法：本文档 + `docs/progress.md`（进度真相源）+ `docs/plan.md`（任务规划）三份一起读，谁接手都不需要再问"做到哪了"。

## 一、这是什么项目

华为 ModelEngine Nexent 赛题3"可进化决策智能体"参赛项目。已定主场景：**金融·上市公司对外担保披露核验**；已跑通真实案例：**沪光股份 605333**（制造业，兼作多行业迁移素材）；**医疗方向正在评估转向**（方案见下）。

评分四维各 25%：①智能体编排与 MCP/Skill 深度集成 ②资产激活的业务价值验证 ③算法创新性 ④系统交互体验与能力沉淀。决赛加答辩 40% + 附加分（多行业迁移、真实数据集评测）。

## 二、仓库与本地环境

- **仓库**：https://github.com/qsm68p75m6-arch/nexent.git （公开仓，**密钥严禁入库**）
- **本地工作副本**：`D:\nexent`（git 代理已配 127.0.0.1:7890；身份 lfw）
- **Nexent**：本地 Docker v2.7.0 full 部署，源码在 `C:\Users\anoth\nexent`，容器前缀 `nexent-*`
- **数据库**：对话记录在 `nexent-postgresql` 的 `nexent` 库、`nexent` 模式（`conversation_record_t` / `conversation_message_t`）；记忆在 `memory_records_t` + ES 索引 `mem_embedding-3_2048`
- **磁盘**：C 盘紧张（可用约 22G），Docker 虚拟磁盘 `C:\Users\anoth\AppData\Local\Docker\wsl\disk\docker_data.vhdx` 约 81G

## 三、当前进度（截至 2026-11-11）

### 已完成且已验证

| 项目 | 状态 | 证据位置 |
|---|---|---|
| POC 合成实验 C01–C08 | **六轮全部通过** | `poc-review/`、`evidence/C08_20261111/` |
| 真实评测 T01–T10（沪光） | **内容 10/10 通过**（预评分） | `evidence/T01-T10_20261111/judgement.md` |
| answers.csv 机械复核 | **39/39 项通过**（待甲签收转正） | `evidence/T01-T10_20261111/answers_verification.md` |
| 记忆串味问题 | 已关闭（关记忆+清存量+复测达标） | `evidence/T01-T10_20261111/T05_retest2_clean_20261111.md` |
| D02 图片轮根因 | 三层排查闭环，**原生路线判死（v2.7.0 无 OCR）** | `evidence/D02_image_round_20261111/judgement.md` |
| 团队协作骨架 | 已建并推送 | README / docs/ / evidence/ / configs/ |

### 未完成（按优先级）

1. **图片轮收尾**：主轨 = textlayer 版入库（`poc-kit/import_docs/D02_ownership_diagram_textlayer.pdf`）验证 + 跑题；亮点轨 = 会话内直接传原图给多模态模型读图（验证多模态配置是否接入对话链路）。两条都没跑过。
2. **清理版本库中重复入库的 D08**（C08 a 轮回答检出两份同内容文档）。
3. **甲签收 answers.csv**（机械复核已代劳，只需在报告签收栏签名）→ 预评分转正。
4. **组内拍板**：沪光（制造业）口径是否采用（progress.md 决策记录有建议方案）；是否转医疗方向。
5. **P4 自研 MCP 计算工具**（担保余额/剂量计算，走 OpenAPI→MCP 转换路线）。
6. **P5 检索-推理双驱动 Skill 编排**。
7. **A5 本体演化对照实验**（随机 vs 不确定性 vs 业务影响优先，三种审核策略比本体质量与答题正确率）——**这是设计文档"算法创新性"章节最值钱的证据缺口**。
8. **磁盘治理**：fstrim 已完成，diskpart 压缩待用户管理员执行（预计回血 17G+）。

## 四、关键决策记录

- 2026-11-07：近期本地开发，云服务器暂缓部署。
- 2026-11-08/11：POC 阶段用调试功能即可，暂不发布智能体。
- 2026-11-11：D02 图片轮走双轨（textlayer 主轨 + 会话内多模态亮点轨），**不换 OCR 引擎**（改动平台会破坏评测环境一致性）。
- 2026-11-11：清理全部记忆存量，正式评测期间保持记忆关闭。
- 待定：医疗方向（见 `docs/session-20261111-log.md` 第七节的三套方案）。

## 五、给接手 agent 的硬性提醒

1. **查对话记录不要人工复制**：从 `nexent-postgresql` 的 `nexent.conversation_message_t` 直接 SELECT 导出（`delete_flag='N'`），比让用户粘贴快且全。
2. **判分基准以仓库文件为准**，不以聊天记录为准。answers.csv 未签收前，T 系列结论只能称"预评分"。
3. **原始记录不清洗**：失败的尝试也要归档进 `evidence/`（用户曾删掉失败会话，已提醒）。
4. **评测隔离**：任何缺证/隔离类实验前，必须关闭 agent 记忆并清空 `memory_records_t`，验收标准 = 串味数字零出现 + 记忆表零新增。
5. **本机坑**：`python` 跑脚本文件会静默无输出，用 `python -I -c` 内联；Git Bash 会改写 `/unix` 路径，用 `MSYS_NO_PATHCONV=1` 或 `bash -c` 包裹。

## 六、下一步第一件事

跑图片轮主轨：`pdf测试` 库删掉失败的 D02 → 上传 textlayer 版 → 确认分片详情含股权实体 → 按 C02 原设计出题验证。结果归档到 `evidence/D02_image_round_20261111/`。
