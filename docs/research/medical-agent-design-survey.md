# 医疗领域 Agent 优秀工作与 CCF A/B 论文调研（2026-11-11）

> 目的：为主方案找可迁移的设计模式，并支撑"多行业模板迁移"附加分。
> 核实纪律：标注"已亲验"= 我抓取 arXiv/ACL Anthology 原页面确认；"未回源"= 仅有检索摘要，写进正式材料前需自查。**CCF 等级以 CCF 官网第七版目录为准**。

## 一、论文清单

### 核心可借鉴（建议精读）

| # | 论文 | 发表处 / CCF | 核实 | 一句话核心 |
|---|---|---|---|---|
| 1 | **MedAgent-Pro**: Evidence-based Multi-modal Medical Diagnosis via Reasoning Agentic Workflow | arXiv 2503.18968（**未发表**，2025-03） | 检索 | 两级分层：疾病级用 RAG 检索指南生成**可执行诊断计划**，患者级用工具 agent 执行并**逐步骤验证证据**。眼科/心脏病 bAcc 90.4/77.8，远超 GPT-4o 的 56.4/56.8 |
| 2 | **MDAgents**: An Adaptive Collaboration of LLMs for Medical Decision-Making | NeurIPS 2024（**CCF-A**） | 部分（搜到 NeurIPS proceedings DOI 10.52202/079017-2522；arXiv 页无 comments） | 先判**任务复杂度**（低/中/高），再决定单人、多学科团队还是综合团队；简单题不浪费多 agent |
| 3 | **MedAgentBoard**: Benchmarking Multi-Agent Collaboration... | NeurIPS 2025 **Datasets & Benchmarks Track**（**CCF-A**） | **已亲验**（arXiv comments 原文） | **反潮流结论**：多 agent 并非总赢强单模型；文本QA上不赢，工作流自动化上赢（提升任务完整性） |
| 4 | **EHRAgent**: Code Empowers LLMs for Few-Shot Complex Tabular Reasoning on EHR | **EMNLP 2024 主会**（**CCF-B**） | **已亲验**（comments: "Accepted in EMNLP 2024 main conference"） | 把问答变成**写代码 + 执行 + 看报错改代码**的循环；长期记忆复用成功案例。比最强基线高 29.6% 成功率 |
| 5 | **ReConcile**: Round-Table Conference Improves Reasoning via Consensus among Diverse LLMs | ACL 2024 主会（**CCF-A**） | 检索 | 多 agent 各自出答案+置信度，多轮讨论后**置信度加权投票**；医疗多 agent 研究的标准基线 |
| 6 | **MedRAX**: Medical Reasoning Agent for Chest X-ray | **ICML 2025**（**CCF-A**） | 检索 | 不做"让 VLM 硬看图"，而是**编排一组专用视觉工具**（分割 MedSAM、定位 Maira-2、分类 DenseNet）+ ReAct 循环；配套 ChestAgentBench（2,500 题/7 类） |
| 7 | **MEDDxAgent**: Unified Modular Agent Framework for Explainable Automatic DDx | **ACL 2025 主会长文**（**CCF-A**） | **已亲验**（ACL Anthology 2025.acl-long.677） | 编排器 + 病史采集模拟器 + 知识检索 agent + 诊断策略 agent；不假设"资料一开始就齐" |
| 8 | A Survey of LLM-based Agents in Medicine: How far are we from Baymax? | **ACL 2025 Findings**（ACL 为 CCF-A，Findings 非主会） | **已亲验**（comments: "ACL 2025 Findings"） | 四层架构：系统概况 / 临床规划 / 医学推理 / 外部能力增强；四种范式（单 agent、顺序任务链、协作专家、迭代进化） |
| 9 | **MedAgentBench**: EHR Agent Benchmark | **NEJM AI** 2025（非 CCF，医学 AI 顶刊） | 检索 | 300 任务/10 类/100 病人/78 万条记录；**v2 只改设计（提示词+工具+计算器+记忆）就把成功率 70%→98%**，证明"编排 > 换模型" |
| 10 | **AMIE** (Towards conversational diagnostic AI) | **Nature** 2025, DOI 10.1038/s41586-025-08866-7（非 CCF，顶级） | 检索 | 自博弈模拟训练；双盲 OSCE 随机对照，32 个维度赢 30 个。**注意：是模拟环境，作者自己强调未做真实临床验证** |
| 11 | **MedAgents**: LLMs as Collaborators for Zero-shot Medical Reasoning | 搜索称 ACL 2024 Findings（**未回源**，arXiv 页无会议标注） | 部分（已亲验标题/作者/五步流程：招专家→各自分析→汇总报告→迭代讨论→决策） | 零样本角色扮演多学科会诊，无需训练 |
| 12 | **KG4Diagnosis**: Hierarchical Multi-Agent LLM + Knowledge Graph | **AAAI-25 Bridge Program**（**注意：Bridge 不是 AAAI 主会**，PMLR 281:195-204） | 检索 | 全科医生 agent 分诊 + 专科专家 agent；自动构建覆盖 362 种疾病的知识图谱 |
| 13 | **AI Hospital** Benchmarking LLMs in Multi-agent Medical Interaction Simulator | COLING 2025（**CCF-B**） | 检索 | 医生/患者/检查员三 agent 模拟问诊，MVME 基准 |
| 14 | GARMLE-G (Generation-Augmented Retrieval + 临床实践指南) | IEEE **JBHI** 2025（**CCF-C，不是 B**） | 检索（等级由检索纠正） | 先生成再检索指南片段，号称"无幻觉"；高血压/冠心病原型 |
| 15 | 医疗 LLM 弃权研究：MedSAFE（npj Digital Medicine）、MedAbstain（EACL 2026） | npj Digital Medicine / EACL（**EACL 为 CCF-B 系列**） | 检索，未回源 | 把"答/不答"建模成决策问题；用 conformal prediction 校准；区分"恰当弃权率"与"过度弃权率" |

**等级易错点（写材料前务必自查）**：
- JBHI 是 **CCF-C**，常被误传为 B；医学影像 B 类期刊是 TMI。
- MICCAI 属 **交叉/综合/新兴 B 类**（据检索，需按官网自查）。
- AAAI Bridge / Workshop 论文**不能**按 AAAI 主会（A 类）算。
- ACL Findings 不能按 ACL 主会论文陈述（会议是 A 类，但 Findings 收录性质不同，建议写"ACL 2025 Findings"原文）。

## 二、设计模式 → Nexent 落地映射

这是本调研的核心产出。左边是论文做法，右边是 Nexent 上的具体动作。

### 模式 1：规划层与执行层分离（MedAgent-Pro）★最高优先级

| 论文做法 | Nexent 落地 | 对应赛题评分项 |
|---|---|---|
| 疾病级：RAG 检索指南库 → 生成**可执行诊断计划** | 建"核验规则库"（授权决议、披露规则、口径定义）。规划 Skill：先检索规则 → 输出**核验检查清单**（检查项 / 去哪找 / 判据 / 通过条件） | 「检索-推理双驱动协同执行流」原文要求 |
| 患者级：工具 agent 执行，**每步骤验证可靠性**后才进入下一步 | 执行 Skill：逐项执行，每项产出（证据ID, 页码/片段, 结论, 是否支撑）。不支撑则回退重检，不带着不确定往前走 | 「决策溯源」「证据支撑」 |
| 定量+定性证据结合，阈值比较 | 计算交给确定性工具（MCP），判断交给模型 | 「数值计算不靠心算」 |

**为什么这是第一优先级**：他们现在 C 系列/T 系列都是"直接问答"，没有显式的规划层。加上规划层后，赛题要的"双驱动执行流架构"就有了具体形状，而且是可画图、可演示的。

### 模式 2：复杂度路由（MDAgents）+ 反潮流证据（MedAgentBoard）★答辩利器

| 论文做法 | Nexent 落地 |
|---|---|
| 先判复杂度，再决定单人/团队 | 主 agent 前置"分流 Skill"：简单提取题（T01 类）单 agent 直答；跨文档冲突/版本/解除题（T07/T08 类）委托子 agent 多视角 |
| MedAgentBoard：多 agent 不总赢，工作流自动化才赢 | 文档里明确写"**按任务类型选择编排形态**"，并引用 MedAgentBoard (NeurIPS 2025) 作为依据 |

**这段是答辩加分点**：评委常见提问是"你为什么用多 agent？"——标准回答"因为多 agent 更强"会被 MedAgentBoard 打脸。正确回答是"我们按任务复杂度路由，且只在跨文档冲突类任务上启用多 agent，其余走单 agent 以保证确定性"。

### 模式 3：代码即工具 + 执行反馈（EHRAgent, EMNLP 2024）

| 论文做法 | Nexent 落地 |
|---|---|
| 生成代码执行，看报错迭代修正 | P4 的 MCP 计算工具：单位归一、去重、口径检查。agent 调用而非心算 |
| 长期记忆复用成功案例 | **注意**：他们刚踩过记忆串味（T05）。要用"库内标准案例"（知识库里的案例文档），**不要**用跨会话 agent 记忆 |

**这条能直接修掉一个已发生的错误**：C07 那次把 G02 的 25 百万错算成 25 万，导致 4,025 vs 3,025——如果有确定性计算工具，这类错误不会发生。

### 模式 4：显式弃权与校准（MedSAFE / MedAbstain）

| 论文做法 | Nexent 落地 |
|---|---|
| 答/不答是决策问题；区分恰当弃权与过度弃权 | 现有 C05/T05 已做到"保留判断"，升级为结构化输出：`结论: 无法判断` + `缺失项: [额度, 余额]` + `补齐后即可判断: 是` |
| conformal prediction 校准 | 轻量版：按检索命中数/证据覆盖率设阈值，低于阈值强制走"弃证"模板 |

**这是他们已有能力的论文背书**——答辩时说"我们实现了 MedSAFE 提出的显式弃权设计"比说"我们会拒答"有分量得多。

### 模式 5：多模态用工具而非硬看（MedRAX, ICML 2025）★直接可用

| 论文做法 | Nexent 落地 |
|---|---|
| 不让 VLM 硬看图，编排专用视觉工具 | 图片轮：股权图不要指望 VLM 直读，挂专用工具（OCR/版面分析/图结构抽取）作为 MCP |
| ReAct 循环调度工具 | Skill 里写"先试 OCR → 失败换版面分析 → 再失败标注'图片不可解析'" |

**这直接回答了他们的 D02 解析失败问题**：图像 PDF 本来就不该走文本解析链路，应该走专门的图像工具链。

### 模式 6：基准设计（MedAgentBench, NEJM AI 2025）

| 论文做法 | Nexent 落地 |
|---|---|
| 任务分类（检索/聚合/写入/排序）+ 步数分层（1步/2-3步/3步以上分别报告） | 他们的 T01-T10 补两个维度标签：**任务类型** + **所需步数**，评测报告按维度分组，而不是只给总分 |
| v2 证明改设计比换模型提升大（70%→98%） | 这正是他们的路线：不换模型、改编排。**这句话可以直接引用来解释"为什么我们不追大模型"** |

### 模式 7：置信度共识（ReConcile, ACL 2024）——有条件采用

只在"跨文档冲突"类任务启用：两个 agent 独立作答，不一致时并列两种结论 + 各自证据，不强行合并。**不要**对所有题都开，否则违反模式 2 的结论。

## 三、医疗场景选择建议（对应附加分）

他们已做金融（担保核验）。要做医疗，**必须能拿到公开、可核验的数据**——这是他们沪光案例成功的根本原因。三个候选：

| 场景 | 与担保核验的结构同构度 | 公开数据可得性 | 建议 |
|---|---|---|---|
| **临床路径 / 诊疗规范合规核验** | ★★★★★（多文档、规则+事实、时限、版本） | 国家卫健委临床路径、NICE guidelines、MedlinePlus（MedAgent-Pro 用的就是它） | **首选** |
| 医保政策适用性核验 | ★★★★（条件推理、政策变更有版本） | 国家医保局公开文件充足，但病例数据不公开 | 次选（只做政策侧，不碰真实病历） |
| 药品说明书一致性核验 | ★★★★（多份说明书、剂量/禁忌冲突） | NMPA 公开说明书 | 备选，数据最干净 |

**推荐叙事**："同一套核验模板（规则检索 → 检查清单 → 逐项取证 → 冲突并列 → 版本追溯），从金融担保迁移到临床路径，只替换知识库和规则定义，编排与 Skill 完全复用。"——这句话就是附加分"多行业模板轻量化迁移适配验证"的答案。

## 四、不要照抄的地方

1. **别把"多 agent"当卖点堆砌**。MedAgentBoard 已证伪。要写"按任务复杂度路由"。
2. **AMIE 的结论不能直接引用成"AI 超过医生"**。作者自己强调是模拟 OSCE 环境、文本对话形式，未做真实临床验证。引用时必须带这个限定。
3. **"无幻觉"这类表述不要用**。GARMLE-G 自称 hallucination-free，但同期的神经内科 RAG 研究（npj Digital Medicine 2025）发现 RAG 系统仍会编造引用。写"证据可溯源 + 缺证保留判断"更稳。
4. **记忆类设计慎用**。EHRAgent 的长期记忆在医疗里是加分，在他们这个平台上刚出过跨会话串味事故。要用"库内案例"而非"agent 记忆"。

## 五、可直接执行的三个动作（按性价比排序）

1. **写一页"设计模式映射表"进设计文档**（半天）：把上面模式 1、2、4 写进"整体方案设计"，每条配一句论文出处。这是纯写作，不需要跑实验，直接提升文档档次。
2. **给 Agent 加"核验计划"前置 Skill**（1-2 天）：选 2 道题（T03 跨文档、T07 版本）试，对比"有规划层 vs 无规划层"的答案结构差异。这个对照实验本身就是答辩素材。
3. **给 T01-T10 补任务类型/步数标签**（半天）：评测报告按维度分组重出一次，比只给"10/10 通过"更有说服力。

## 六、核实状态汇总

- **已亲验（抓原页面）**：EHRAgent 的 EMNLP 2024 主会 comments；MedAgentBoard 的 NeurIPS 2025 D&B comments；MEDDxAgent 的 ACL 2025 长文页面；Survey 的 ACL 2025 Findings comments；MedAgents 的标题/作者/五步流程。
- **未回源（仅检索摘要）**：MedAgent-Pro、MDAgents 的会议归属、MedRAX、MedAgentBench、AMIE、KG4Diagnosis、AI Hospital、GARMLE-G、弃权类研究、CCF 等级（JBHI=C、MICCAI=B）。
- **写进正式材料前**：上述"未回源"条目逐条去 arXiv comments 或会议 proceedings 页确认；CCF 等级以 [CCF 官网第七版目录](https://www.ccf.org.cn/Academic_Evaluation/By_category/) 为准。
