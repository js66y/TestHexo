---
title: LangGraph
date: 2026-03-30T09:54:25+08:00
tags: LangGraph
---

LangGraph基础概念
<!-- more -->
- LangGraph
  - state
    - Schema[如何存]
    - Reducers[如何修改]
  - Edges
    - Conditional[根据函数判断]
  - Graph API
    - send[根据函数内send进行路由后拼接（MapReduce）]
    - Command[更新状态后update，跳转到特定节点goto[不同域支持graph=Command.PARENT]]
  - 高级特性
    - 流模式
      - values(每个跟新输出完整),
      - updates(只输出更新的),
      - messages(逐token输出),
      - custom(输出自定义write的内容),
      - debug
    - 持久化
      - Checkpointer(短期记忆)
      - BaseStore(长期记忆)
    - 时间回溯
      - get_state_history获取所有时间点
      - update_state在某个时间点后继续执行
    - 子图
      - 将子图绑定到节点
      - 将子图放在父图的一个节点
    - 多智能体
      - 主管
      - Handoffs


LangGraph
工作流记忆
  - 短期记忆state 继承的是TypedDict不是basemodel
  - 长期记忆Checkpointer,在编译图的时候设置[MemorySaver,SqliteSaver,PostgresSaver] thread_id隔离不同的记忆
代理记忆
  - 短期记忆invoke的config，作用域是单词调用
  - 长期记忆create_react_agent的store，作用域是全局跨thread
  - agent中的Checkpointer,会话级的记忆
- create_react_agent创建代理
- response_format结构化输出
- 创建流程StateGraph(State).add_node().add_edge().compile()   .stream() / .invoke()
- 工具调用作为节点BasicToolNode(tools=[tool])
- 带有判断的条件边.add_conditional_edges()
- 人在循环中，工具中使用interrupt，这将暂停并可恢复
  - 人在循环中其实是通过.stream传入了一个Command(resume=)
- .stream()中的config中添加thread_id进行记忆隔离
- 工具函数返回一个Command对象更新state:return Command(update=state_update)
- 任意时刻使用graph.update_state对state的修改
- 历史与回溯graph.get_state_history(config)这里面的checkpoint_id用于回溯
  - 使用graph.get_state(config)获取指定checkpoint的检查点
- invoke 可设置最大迭代次数，stream有异步的astream，不同的传输形式stream_mode=["updates", "messages", "custom"]
- 工具中使用args_schema=BaseModel定义输入数据格式
- model = model.bind_tools(tools)告知模型函数的信息
- state记忆的管理，在create_react_agent的时候添加summarization_node或者trim_messages
- 多agent组织方式[主管，群组，移交]
- 编排器Send 对象通常必须配合 add_conditional_edges 使用。Send的使用类似于mapreduce
- 评估器llm.with_structured_output(Feedback)工具调用节点搭配动态路由实现校验BaseModel格式校验
- 同样有与构建的agent create_react_agent
- Annotated对state进行规约,在第二个参数中添加Reducer，将按照Reducer进行数据的更新
- 不使用添加边的方式添加执行顺序.add_sequence([step_1, step_2, step_3])
- 在节点还可以做的事情：运行时配置，节点缓存，延迟执行
- 流式输出的各种模式stream_mode=["updates", "custom"]

高级内容
- 检查点：从检查点恢复
- 人在回路中：批准拒绝，审查编辑，验证输入
- 记忆：编辑消息，总结消息，移除消息
- 记忆类型：语义记忆，场景记忆，程序记忆

mcp服务定义from mcp.server.fastmcp import FastMCP