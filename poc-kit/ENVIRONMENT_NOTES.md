# 已执行的环境核验与尚待验证项

核验日期：2026-10-22，时区 Asia/Tokyo。完整响应状态见 environment_snapshot.json。

## 当前已确认

- Docker 中 Nexent 核心服务镜像标签为 v2.7.0，容器处于 running；Langfuse 和 OpenTelemetry 相关容器也在运行。
- Nexent Web 与 Langfuse Web 的 HTTP 请求成功；Config、Runtime、Northbound 的 `/health/live` 和 `/health/ready` 成功。
- Config 的 `/indices/health` 返回 healthy，证明该检查当时能够访问 Elasticsearch。
- 用户已确认文本模型、Embedding 和多模态模型均已配置。本次没有调用这些模型。
- 本地 checkout 是 develop 分支；v2.7.0 tag 与当前 checkout 的提交不同。静态能力核验采用 v2.7.0 tag，而不将 develop 的功能视为当前运行能力。

## v2.7.0 静态核验对实验的影响

- 本地知识库上传控件接受 PDF、MD、TXT 等格式，不接受独立 PNG。图片实验使用嵌入图片的 PDF。
- 上传是异步处理的起点；还需确认解析、嵌入和索引完成，并验证能检索到正文。
- `knowledge_base_search` 通过知识库绑定形成能力，不应要求用户一定能从工具列表中手动勾选该工具。静态默认 top_k 为 3；实际值以运行配置为准。
- 原生检索包装中的文件及引用信息，不等同于已具备表格单元格或图片坐标定位。不能编造未返回的页码或坐标。
- MCP 服务可发现、服务已启用、Agent 实际调用是三个独立验收点。
- Skill 可使用单个 SKILL.md 或多文件 ZIP。上传成功不等于业务执行成功。
- 内置评测支持 Excel 测试集；本小包首轮先手工运行固定题并留存原始输出。后续接入平台评测时沿用冻结题目，不用自动生成题代替固定回归。

## 尚未验证

模型真实响应、工具调用兼容性、Embedding 与索引匹配、PDF 图像抽取、股权图视觉理解、图像事实进入检索、Agent 引用支持性、MCP/Skill 业务闭环和单题完整追踪均待逐轮实验。

进程 ready 不检查全部外部依赖，Langfuse 容器运行也不证明业务链路已成功上报。

本次没有修改已有知识库、Agent、数据库、模型配置、部署配置或 Nexent 源码，没有重启容器。交付包是机制验证材料，不是已部署的业务系统。
