---
title: qwen3_int4_gptq
date: 2026-03-30T10:00:14+08:00
tags: 量化
---

<!-- more -->
1.参考文档：
https://huggingface.co/onnx-community/Qwen3.5-0.6B-ONNX-INT4-CPU

使用工具：
- CMake3.31+
- Transformer 4.51
- Olive
- ONNXRuntime Genai

配置过程：
1. 清除代理``$env:HTTP_PROXY="", $env:HTTPS_PROXY=""``
2. pip安装pysocks
3. 设置代理``env:HTTP_PROXY="socks5://127.0.0.1:10808", env:HTTPS_PROXY="socks5://127.0.0.1:10808"``(vray2N)
4. 编译并安装ONNXRuntime Genai

下载模型：
- winget 安装 git-xet
- huggingface进行克隆

!!!! onnxruntime genai现在还不支持Qwen3.5的框架
使用官方的方法
from modelscope import AutoModelForCausalLM, AutoTokenizer
