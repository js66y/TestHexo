---
title: LlamaIndex
date: 2026-03-30T09:55:39+08:00
tags: LlamaIndex
---

LlamaIndex基础概念
{% asset_img 1774277828825.png LlamaIndex预设agent %}
<!-- more -->
1. 自己实现智能体
2. AgentWorkflow多智能体
3. FunctionAgent作为与构建的智能体
- ctx = Context(workflow)用来存储维护状态
- 运行工作流workflow.run(user_msg=)
- 流式的输出for event in handler.stream_events():
- 在工具中使用ctx.wait_for_event()类定义HumanResponseEvent来进行人在回路中
- AgentWorkflow默认使用移交的形式进行多智能体协作

自定义工作流
- class MyWorkflow(Workflow):@step定义需要指定输入输出状态[继承Event]和step步骤
- 通过输出的类型判断走哪个下一步
- 并发的节点ctx.collect_events(ev,[节点列表])
- 通过继承工作流可以添加新的运行节点和边
- 工作流中可以嵌套使用工作流或者使用工作流解绑@step(workflow=TestWorkflow)添加节点和边
- 数据读取器SimpleDirectoryReader 读取一个 Document对象
- VectorStoreIndex.from_documents(documents, transformations=)将Doc转换成Node对象
    - pipeline = IngestionPipeline(transformations=[TokenTextSplitter(), ...])
    - nodes = pipeline.run(documents=documents)
- 向量索引index = VectorStoreIndex(nodes)
- .persist索引写入磁盘，load_index_from_storage索引导出磁盘
    - 使用向量数据库存储Chroma
    1. 初始化 Chroma 客户端
    2. 在 Chroma 中创建一个 Collection 来存储您的数据
    3. 在 StorageContext 中将 Chroma 指定为 vector_store
    4. 使用该 StorageContext 初始化您的 VectorStoreIndex
- 查询阶段index.as_query_engine().query()[自定义的相似度额值和topk]
- 结构化llm，使用baseModel来限制输出的格式
    - 也可以使用llm.structured_predict()来运行结构化限制