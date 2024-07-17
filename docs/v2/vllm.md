# vLLM

我们建议您在部署 Qwen 时尝试使用 vLLM，它易于使用，且具有最先进的服务吞吐量、高效的 **注意力键值内存管理**（通过 PagedAttention 实现）、连续批处理输入请求、优化的 CUDA 内核等功能。

## 1. 安装

默认情况下，你可以通过 `pip` 来安装 vLLM：

```bash
pip install vLLM>=0.4.0
pip install ray  # 推荐安装
```

但如果你正在使用 CUDA 11.8，请查看官方文档中的注意事项，以获取有关安装的[帮助](https://docs.vllm.ai/en/latest/getting_started/installation.html)。 此外，建议通过安装`ray`，以便支持分布式服务。

## 2. 离线推理

