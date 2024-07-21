# LangChain

本教程旨在帮助您利用 `Qwen2-7B-Instruct` 与 `langchain`，基于本地知识库构建问答应用。目标是建立一个知识库问答解决方案。

## 1. 安装

```bash linenums="1"
pip install langchain==0.0.174
pip install faiss-gpu
```

## 2. 基础用法

您可以仅使用您的文档配合 `langchain` 来构建一个问答应用。该项目的实现流程包括加载文件 --> 阅读文本 --> 文本分段 --> 文本向量化 --> 问题向量化 --> 将最相似的前 k 个文本向量与问题向量匹配 --> 将匹配的文本作为上下文连同问题一起纳入提示 --> 提交给 Qwen2-7B-Instruct 生成答案。



