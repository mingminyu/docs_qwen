# Ollama

Ollama 帮助您通过少量命令即可在本地运行 LLM，它适用于 MacOS、Linux 和 Windows 操作系统。现在，Qwen2 正式上线 Ollama，您只需一条命令即可运行它：

```bash
ollama run qwen2
```

## 1. 快速开始

访问官方网站 [Ollama](https://ollama.com/download) ，下载并安装 Ollama。您还可以在网站上搜索模型，在这里您可以找到 Qwen2 系列模型。

除了默认模型之外，您可以通过以下方式选择运行不同大小（0.5B、1.5B、7B 以及 72B）的 Qwen2-Instruct 模型：

```bash linenums="1"
ollama run qwen2:0.5b
```

## 2. 运行 GGUF 文件

有时您可能不想拉取模型，而是希望直接使用自己的GGUF文件来配合Ollama。假设您有一个名为 qwen2-7b-instruct-q5_0.gguf 的Qwen2的GGUF文件。在第一步中，您需要创建一个名为 Modelfile 的文件。该文件的内容如下所示：
