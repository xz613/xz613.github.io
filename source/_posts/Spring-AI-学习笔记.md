---
title: Spring AI 学习笔记
date: 2026-06-25 17:57:17
tags:
  - Spring AI
  - 大模型
categories:
  - Java
---

## 一、大模型专有名词介绍

### 1. LLM（Large Language Model）大语言模型

基于海量文本数据预训练而成的深度学习模型，能够理解和生成自然语言。常见代表有 GPT、Claude、通义千问、DeepSeek 等。Spring AI 的核心工作，就是把这类模型接入 Java 应用。

### 2. RAG（Retrieval-Augmented Generation）检索增强生成

**核心思路：** 在让大模型回答问题之前，先从外部知识库（文档、数据库等）中检索相关内容，再把检索结果作为上下文一并交给模型生成答案。

**解决的问题：** 模型训练数据有截止日期，不了解企业内部私有数据，且容易产生"幻觉"（编造不存在的信息）。

**典型流程：**

1. 将文档切分（Chunking）并向量化，存入向量数据库
2. 用户提问时，将问题转为向量，检索最相关的文档片段（Top-K）
3. 将检索到的内容拼入 Prompt，交给大模型生成最终回答

**Spring AI 对应能力：** `VectorStore`、`DocumentReader`、`QuestionAnswerAdvisor` 等。

### 3. Fine-tuning 微调

在预训练模型的基础上，用特定领域或任务的数据继续训练，使模型更适应某个场景（如客服话术、医疗问答、代码生成）。


| 对比项  | RAG        | Fine-tuning  |
| ---- | ---------- | ------------ |
| 知识更新 | 更新文档即可，成本低 | 需重新训练，成本高    |
| 适用场景 | 知识问答、文档检索  | 风格/格式/专业能力定制 |
| 数据要求 | 原始文档       | 标注好的训练样本     |


两者可以组合使用：微调提升模型"说话方式"，RAG 补充实时知识。

### 4. Prompt Engineering 提示工程

通过精心设计输入文本（Prompt），引导大模型输出期望的结果，无需修改模型参数。

**常见技巧：**

- **System Prompt：** 设定模型角色与行为边界（如"你是一名 Java 架构师"）
- **Few-shot：** 在 Prompt 中给出几个示例，让模型模仿输出格式
- **Chain of Thought（CoT）：** 要求模型"一步步思考"，提升推理类任务的准确率

**Spring AI 对应能力：** `PromptTemplate`、`ChatClient` 的 system/user 消息构建。

### 5. Embedding 嵌入 / 向量化

将文本（词、句、段落）映射为高维数值向量（如 1536 维浮点数组）。语义相近的文本，其向量在空间中距离更近，这是语义检索的基础。

**Spring AI 对应能力：** `EmbeddingModel`，配合 `VectorStore` 完成文档入库与相似度检索。

### 6. Token 与 Context Window 上下文窗口

- **Token：** 大模型处理文本的最小单位，约 1 个英文单词 ≈ 1~~2 个 Token，1 个汉字 ≈ 1~~2 个 Token。API 按 Token 计费。
- **Context Window（上下文窗口）：** 模型单次能"看到"的最大 Token 数（如 8K、32K、128K）。超出部分会被截断，影响长文档问答效果。

### 7. Hallucination 幻觉

模型生成看似合理、实则错误或与事实不符的内容。RAG、降低 Temperature、明确 Prompt 约束是常见的缓解手段。

### 8. Agent 智能体

让大模型具备**自主规划与执行**能力：根据目标拆解任务、选择工具（查数据库、调 API、搜索网页等）、多轮推理直至完成。

**与简单 Chat 的区别：** Chat 只生成文本；Agent 可以调用外部工具并基于工具返回结果继续推理。

**Spring AI 对应能力：** `ChatClient` + `FunctionCallback` / `@Tool` 注解。

### 9. Function Calling / Tool Calling 函数调用

大模型在生成回复时，可以输出结构化的"工具调用请求"（函数名 + 参数），由应用层执行真实逻辑（查天气、下单、查库存），再把结果回传给模型继续生成。

这是构建 Agent 的核心机制之一。

### 10. Vector Database 向量数据库

专门存储和检索高维向量的数据库，支持高效的相似度搜索（如余弦相似度、欧氏距离）。常见选型：PgVector、Milvus、Redis Vector、Chroma 等。

**Spring AI 支持：** 通过 `VectorStore` 抽象统一接入多种向量存储后端。

### 11. Temperature 温度参数

控制模型输出的随机性，取值通常在 0~2 之间：

- **低（如 0~0.3）：** 输出更确定、保守，适合代码生成、事实问答
- **高（如 0.7~1.0）：** 输出更多样、有创意，适合文案创作、头脑风暴

### 12. Streaming 流式输出

模型逐 Token 返回生成结果，而非等待全部生成完毕再一次性返回。用户体验更流畅，Spring AI 通过 `Flux<String>` 支持 SSE 流式响应。

---

## 二、什么是 Spring AI？

Spring 官方推出的 AI 应用开发框架。Spring AI 解决了 AI 集成的根本难题：**将企业数据和 API 与 AI 模型连接起来**。

**核心特性：**

- 统一的 `ChatModel`、`EmbeddingModel` 抽象，屏蔽不同厂商 API 差异
- 内置 RAG 全流程支持（文档读取、分块、向量化、检索）
- 原生支持 Function Calling，便于构建 Agent
- 与 Spring Boot 生态无缝集成（自动配置、依赖注入）





