# evidence 证据归档规范

每轮实验一个目录：`evidence/<题号或包号>_<日期>/`，例如 `evidence/C08A_20261107/`。

目录内三件套：

- `answer.md`：Agent 原始回答全文（不删改）
- `retrieval.txt`：当次实际检索片段 / 工具返回
- `screenshot.png`：平台截图（会话与知识库绑定状态可见）

原则：

- 原始记录不清洗、不摘要；结论与判读写进 docs/progress.md 一行即可。
- **失败记录同样归档**——失败 → 诊断 → 修复是赛题"调试迭代经验"和答辩最有力的素材（范例见 poc-review/C07_tool_dispatch_diagnosis.md）。
