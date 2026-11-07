# C07阶段C：工具调度记录复核

复核日期：2026-10-24。依据为用户粘贴的执行过程、本地runtime日志中的对应代码输出，以及与运行镜像标签一致的Nexent v2.7.0源码。未修改平台配置或源码，未调用模型。

## 已取得的运行证据

用户提供的流程只显示一次Python执行，随后发起询问资料位置的ask_user内容，没有实际知识库检索或子Agent调用记录。

运行日志中对应检查的输出是：

```text
inputs -> []
outputs -> []
. -> ['outputs', 'inputs']
kb tool available: False
agent available: True
```

这里的函数检查对应knowledge_base_search与agent_3_v1。可确认当前Python命名空间没有前一个名称，但有后一个名称；不能把这个检查理解成后台知识库不存在。实际可用工具可能分布在根Agent与已挂载子Agent中，工具名称也需以本次运行提供的声明为准。

## 当前判定

**阶段C失败发生在取得领域证据之前。** 当前工作台能运行Python并具有一个子Agent入口，但显示的流程没有调用该子Agent，也没有执行知识库检索，随后便询问用户重新提供资料。

因此，当前不是已证明的A1/A2时间推理错误，也没有证据证明文件已丢失。首先需要验证正确的工具/子Agent路径。挂载子Agent自身是否正确绑定POC_TEXT_VERSION，尚未由此日志证明。

## 三个容易混淆的地方

1. inputs/outputs是运行工作区，不能默认等同于平台知识库。目录为空不代表Elasticsearch索引中的文档为空。
2. knowledge_base_search不在当前Python环境，不等于领域Agent没有该能力。若能力在子Agent里，根Agent需要实际调用子Agent；检查名字存在并不等于已经调用。
3. UI中的“智能体工作台”是通用显示名称。仅凭这个名称不能认定没有挂载领域Agent；本次日志反而显示agent_3_v1名称可用。

## 源码核验支持的机制

- v2.7.0的frontend/features/workbench/state.ts中，工作台初始模式为generic_chat，agent_mounts为空；切换部分模式时会清空agent_mounts和会话knowledge_scope。因此新会话实际资源应核对，不能仅凭旧会话成功就推断新会话等价。
- backend/services/workbench_service.py将挂载的持久化Agent编译为agent_{ID}_v{版本}形式的调用名。因此agent_3_v1采用的是Agent 3、版本1的命名；这个名字本身不证明该版本已含最新知识库配置。
- sdk/nexent/core/agents/core_agent.py在运行时向Python执行器发送当前tools和managed_agents；当前工具与子Agent的实际声明是调用依据。
- backend/services/knowledge_scope_service.py按会话的inherit/override/disabled范围及权限解析实际可检索索引。
- sdk/nexent/core/agents/clarification.py要求ask_user以独立Python代码调用。用户贴出的原始DSML标记与这个约定不同；它是工具格式可能未正确解析的信号，是否真正执行应以ask_user事件或澄清卡片为准，不能只看原始文本。

以上为静态机制与运行日志证据。具体是会话范围、发布版本、模型调度还是工具格式导致本次路径中断，尚不能凭单段记录唯一归因。

## 最短后续验证

1. 在当前会话资源选择中确认挂载的是预期领域Agent；核对其实际运行版本内的知识库绑定包含POC_TEXT_VERSION。若修改只在草稿中，需核对当前运行版本是否已包含该修改。
2. 在当前会话发送C07_delegate_probe.txt，明确要求调用已可用的agent_3_v1，先只返回知识库来源与原文。
3. 查看执行过程是否真正出现agent_3_v1调用，以及它内部的知识库检索。若子Agent被调用后仍取不到资料，再核对该Agent的知识库绑定、会话有效索引与检索返回。
4. 若显式派发可成功检索，说明此路径基本可用；修复点优先落在根Agent的任务路由和提示规则。之后仍需在新会话使用原始六月问题回归，不能用探针成功替代原任务通过。
5. 若继续只输出DSML原始标记、没有对应工具事件或卡片，记录为工具调用格式问题，单独检查模型输出与当前CodeAgent调用协议，不先改领域规则。

本文件包含调试判断，不放入被测知识库。C07_delegate_probe.txt只用于诊断调度路径，不作为正式通过率样本。

## 后续状态

用户已重新执行c轮，并确认采用全新会话直接发送原始六月问题。重跑核心结论正确，可以记录本次历史回归通过。首次调度失败记录保留；尚未取得足以证明调度稳定性已修复的配置对照与调用链。详见C07_C_rerun_review.md。
