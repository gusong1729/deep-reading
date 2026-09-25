# ADR-0001：知识库采用卡帕西 LLM Wiki 架构，不引入 RAG

用户明确要求按 Andrej Karpathy 的 LLM Wiki 方法论构建知识库。决策：采用三层架构（Raw 只读源 / Wiki LLM 全权维护 / Schema 规则文件）+ 三操作（Ingest/Query/Lint）+ index.md/log.md 双索引文件，不引入向量数据库与 RAG。

背景：RAG 是"每次提问临时检索拼凑答案、知识不积累"；LLM Wiki 是"提前把知识编译成持久化 wiki、每次新资料触发关联页增量更新、知识会长"。用户规模（单库数百篇内）用 index.md 渐进式披露即可覆盖，无需额外基础设施。

_Considered Options_: RAG/向量库（被否：知识不积累、需部署）；纯手工笔记（被否：维护成本高、交叉引用易腐坏）。
