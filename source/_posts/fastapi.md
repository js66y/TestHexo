---
title: fastapi
date: 2026-03-30T09:53:32+08:00
tags: fastapi
---

fastAPI基础概念
<!-- more -->
1. flask基础是并发
- 并发与并行的区别，并发是接受端，并行是处理端
- async函数必须搭配await函数使用
- Pydantic是数据校验，严格的输出格式限制
- OpenAPI生成api文档
2. 路径参数[类型转换]
- 不同路径节点顺序有意义
3. 查询参数 
- 可选和默认值的设置，设置必选[没有None]
4. 请求体
- pydantic的BaseModel用于接收请求体
5. 查询参数的Annotated验证
- `q: Annotated[str | None, Query(max_length=50)] = None`,这里q为变量名，类型为str或None,来源和要求为Query,默认值为None。
    - 别名
    - 弃用标识
    - 示例
    - 排除在OpenAPI界面中
    - 正则与大小验证
    - 模型是可以嵌套的
    - BaseModel作为类型
        - model_config = {"extra": "forbid"}禁止额外的参数
        - .model_dump()从pydantic中返回属性的dict
    - Field在BaseModel中进行验证
    - Query / Path / Cookie / Header / Form元数据定义
    - AfterValidator逻辑校验器
    - Depends依赖注入
    - Body()请求体
        embed=True则期望输入有一个嵌套层json
    - ->Item vs response_model自动剔除模型中未定义的私密字段
        - 通过继承的方法返回不同的子类
        - 可以设置默认值
        - response_model_exclude_unset=True
        - response_model_include 和 response_model_exclude 返回属性的白名单和黑名单
    - status_code用于指定返回状态代码[1信息，2正常，3重定向，4客户端错误，5服务器错误]
    - 表单数据和query解析方式不同
    - File只是保存在内存，UploadFile保存到内存和磁盘
    - raise HTTPException触发一个错误
    - @app.exception_handler用于覆盖错误响应
    - 路径操作配置除了可以设置[返回类型，返回属性的白黑名单还可以tag,说明和总结与弃用说明]
    - yield作为迭代器，数据库连接池有用
    - Depends依赖注入，定义一个数据处理/生成方案
    - OAuth2安全性: Annotated[OAuth2PasswordRequestForm, Depends()]
        - oauth2_scheme = OAuth2PasswordBearer(tokenUrl="token")  Annotated[str, Depends(oauth2_scheme)]密码流的实现
    - @app.middleware("http")中间件
    - 设置CORS跨域策略app.add_middleware()
    - SQLModel类似于BaseModel创建数据库的模型
        - create_engine(sqlite_url, connect_args=connect_args)创建数据库引擎
    - 后台任务background_tasks: BackgroundTasks  background_tasks.add_task()
