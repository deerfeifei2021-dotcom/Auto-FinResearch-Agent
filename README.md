# 🚀 Auto-FinResearch-Agent: 企业级智能风控图谱引擎

基于 GraphRAG 与多 Agent 协同的企业级财报深度分析与风险预警系统 (准商业化阶段)

## 💡 核心痛点与商业价值
在现代企业尽调与金融风控中，核心风险（如隐蔽担保、复杂关联交易、对赌协议）往往潜藏在数十万字的跨期财报与海外新闻中。传统大模型应用（如基础 RAG）仅停留在文本摘要，极易丢失实体间的隐藏联系，导致风控盲区，且纯 AI 决策缺乏企业落地所需的严谨性。人工审查不仅效率极低，且法律风险极高。

## 🧠 核心架构跃迁 (多 Agent 协作)
本项目突破了传统 RAG 局限，采用多 Agent 流水线架构处理十万字级别的超长上下文，核心创新如下：

1. **GraphRAG 知识引擎**: 将拉取的超长文本抽提成实体关系图谱，Agent 可基于图谱节点进行 Multi-Hop（多跳）深度关联检索。
2. **长链辩论与长期记忆**: 引入 Long-term Memory 追踪企业历史状态。同时首创“风控多 Agent 辩论框架（Debate）”，财务 Agent 与法务 Agent 针对高危节点进行 Proposer-Critique 的多回合交叉推演。
3. **HITL (人类在环) 机制**: 对于高危风险，系统输出长链推理链路并挂起，交由人类专家（法务/CIO）最终确认，形成业务闭环。

## 📊 系统架构图

```mermaid
graph TD
    %% 数据源层
    subgraph DataSources [异构数据源接入]
        A[历史财报 PDF] --> D(Data Fetcher Agent)
        B[实时金融资讯 API] --> D
        C[合规法律库] --> D
    end

    %% 核心知识引擎
    subgraph GraphRAGEngine [图谱知识引擎 - 高消耗节点]
        D --> E{长文本切片 & 向量化}
        E --> F[(ChromaDB 向量库)]
        E --> G[LLM 实体与关系抽取]
        G --> H((企业风控知识图谱))
    end

    %% 多 Agent 逻辑层
    subgraph MultiAgentDebate [多智能体长链辩论核心]
        F -. Context .-> I
        H -. MultiHop .-> I
        I(Financial Analyst Agent<br>财务指标推演) <==>|回合制辩论与自我修正| J(Risk Auditor Agent<br>合规漏洞批判)
        K[Long-Term Memory<br>历史研判记忆] --> I
        K --> J
    end

    %% 决策与展示层
    subgraph UILayer [展示与专家介入层]
        I --> L[CIO Agent 综合决策]
        J --> L
        L --> M[Streamlit 可视化大屏]
        M --> N{风险等级评估}
        N -- 高危风险 --> O((HITL: 移交人类法务))
        N -- 常规分析 --> P[自动生成洞察报告]
    end

    %% 样式美化
    style GraphRAGEngine fill:#f9f2f4,stroke:#d9534f,stroke-width:2px,stroke-dasharray: 5 5
    style MultiAgentDebate fill:#e8f4f8,stroke:#5bc0de,stroke-width:2px
    style O fill:#ffeb3b,stroke:#f39c12,stroke-width:2px
