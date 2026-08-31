# agent

# 一、基础概念

## 什么是 Agent，Agent 与 Chatbot 的区别

以 LLM 为大脑，能自主感知环境、规划任务、调用工具、执行动作并根据反馈迭代，直到完成目标的智能体。
核心循环：接收目标 → 思考规划 → 调用工具行动 → 观察结果 → 继续或结束。

Chatbot：单轮/多轮问答，被动响应，无外部行动能力。
Agent：目标驱动、主动多步执行、能操作外部世界（API、文件、浏览器）、能自我修正。

## Workflow 与 Agent 的区别

Workflow：开发者预先编排好流程，LLM 只在其中某些节点工作。可控性强，适合确定性任务。
Agent：LLM 自主决定步骤和路径。灵活，适合开放性任务，但不可控、成本高。
选型原则：能用 Workflow 解决就不要上 Agent（Anthropic《Building Effective Agents》）。

# 二、四大核心组件

## Planning 规划

任务拆解（把大目标拆成子目标）、反思（检查结果并重试）、自我批评。
常见失败：计划脱离实际、一步错步步错 → 需要 checkpoint 和回滚机制。

## Memory 记忆

短期记忆：上下文窗口内的对话历史。
长期记忆：外置存储（向量库/文件/数据库），跨会话保留。
管理手段：摘要压缩、滑动窗口、按重要度淘汰。

## Tools 工具

让模型突破"只会生成文本"的限制：搜索、代码执行、数据库查询、发请求等。
工具质量 > 数量：命名清晰、描述精确、参数少而明确，模型调用准确率才高。

## LLM 大脑

选型考量：指令遵循能力、工具调用稳定性、上下文长度、成本与延迟。

# 四、Function Calling 工具调用

## 执行流程（重点）

1. 应用方在请求里带上 tools 定义（JSON Schema：名称、描述、参数）
2. 模型不执行函数，只输出结构化的 tool_call（调哪个函数、什么参数）
3. 客户端解析并真正执行函数
4. 把函数结果作为消息回传给模型
5. 模型基于结果继续生成，可能再次发起 tool_call（循环）

## 要点

模型只负责"决策"，执行永远在你的代码侧 → 安全边界可控。
tool_choice 可强制/禁止调用工具；支持一次返回多个并行 tool_calls。
参数靠 JSON Schema 约束，schema 写得越清楚幻觉越少。

# 五、MCP（Model Context Protocol）

## 解决什么问题

M 个 AI 应用 × N 个工具原本要写 M×N 次集成。MCP 把工具封装成标准 Server，任何支持 MCP 的 Host 都能直接用 → 变成 M+N。

## 架构

Host（Claude Desktop / Cursor 等宿主应用）内含 MCP Client，与 MCP Server 连接。
Server 对外提供三类能力：

- Tools：可供模型调用的函数
- Resources：可读取的数据（文件、DB 记录）
- Prompts：预置的提示词模板

## 传输方式

stdio：本地子进程通信（最常见的本地场景）。
Streamable HTTP / SSE：远程服务。

# 六、RAG 检索增强生成

## 标准流程

离线：文档加载 → 切分 chunk → Embedding 向量化 → 存入向量库。
在线：用户提问 → 向量化 query → 相似度检索 top-k → 拼进 prompt → LLM 生成回答。

## 解决的问题

知识时效性（训练截止后的事）、私有知识（内部文档）、缓解幻觉（有据可依）。

## 进阶优化

混合检索：向量 + BM25 关键词互补。
Rerank 重排序：粗召回后用交叉编码器精排。
切分策略：按语义/标题切分优于固定长度，chunk 里保留来源元数据。
Agentic RAG：由 Agent 决定查不查、查什么、查几轮、够不够答。

# 七、上下文工程 Context Engineering

比 Prompt Engineering 范围更大：动态决定模型每一步"看见什么"。
组成：系统提示 + 工具定义 + 检索内容 + 历史消息 + 当前任务。
关键手段：

- 历史压缩：接近窗口上限时把旧对话摘要化（compaction）
- 工具结果裁剪：只保留必要字段，避免大 JSON 撑爆上下文
- 结构组织：重要信息放开头结尾（长上下文存在 lost in the middle 问题）

# 八、多智能体 Multi-Agent

## 常见协作模式

Orchestrator-Worker（主从）：一个主 Agent 拆解分发任务，子 Agent 执行后汇报，最常用。
流水线（Pipeline）：多个 Agent 串行接力，上游输出是下游输入。
辩论/投票（Debate/Vote）：多个 Agent 独立作答再互相批判或投票，提升质量。
群聊（Group Chat）：类似 AutoGen 的多方对话，由 manager 决定下一个发言者。

## 权衡

优点：上下文隔离（每个子 Agent 只带自己需要的上下文）、并行加速、职责专一。
代价：token 成倍增加、通信丢失信息、错误会级联传播。
经验法则：单 Agent 能搞定就不要拆多 Agent。

## A2A 协议

Google 开源的 Agent-to-Agent 协议，让不同框架/厂商的 Agent 互相发现与协作，与 MCP（Agent 连工具）互补。

# 九、常见框架

LangChain / LangGraph：生态最大；LangGraph 用状态图编排节点，支持循环和人工介入，适合复杂 Agent。
OpenAI Agents SDK / Responses API：官方轻量方案，handoff 机制做多 Agent 转交。
AutoGen（微软）：以多智能体对话为核心。
CrewAI：角色扮演式团队分工（CEO、员工角色设定）。
Dify / Coze：低代码可视化搭建平台，适合快速做应用。
Vercel AI SDK：前端友好，流式 UI 与 React 集成好。

# 十、评估 Evaluation

## 评估维度

任务成功率（端到端完成度）、工具调用准确率（该调的调没调、参数对不对）、成本与延迟、轨迹合理性（中间步骤是否浪费/绕路）。

基准测试：SWE-bench（代码）、GAIA（通用助手）、WebArena（浏览器操作）。
可观测性：LangSmith / Langfuse 记录每一步的输入输出与耗时，定位失败环节（tracing + span）。

# 十一、安全与常见坑

## 提示注入 Prompt Injection

恶意指令藏在模型会读到的内容里（网页、文档、工具返回值），劫持 Agent 行为。
间接注入最危险：Agent 读外部内容时被下毒。防御：内容隔离标记、最小权限、关键操作人工确认（human-in-the-loop）。

## 其他风险与对策

幻觉：编造事实或工具参数 → RAG 兜底、结构化校验、要求引用来源。
死循环/失控：设置 max_iterations、超时、预算上限。
权限过大：沙箱执行代码、文件/API 白名单、只读优先。
调试困难：务必记录完整 trace（每次 LLM 输入输出的 prompt 和结果）。

# 十二、面试高频问答速记

Agent 三要素：规划 + 记忆 + 工具使用（+ LLM 内核）。
Function Calling 中模型执行函数吗？不执行，只生成调用意图，执行方是你的代码。
MCP 和 Function Calling 区别？FC 是模型厂商 API 层的能力，MCP 是应用层的工具接入标准协议，底层仍走 FC。
RAG 和微调怎么选？知识频繁更新/需要溯源选 RAG；固定领域风格/格式优化选微调；两者可结合。
为什么 Agent 需要 max_iterations？防止 ReAct 死循环烧钱。
上下文窗口不够怎么办？历史摘要压缩、检索按需注入、外置长期记忆。
