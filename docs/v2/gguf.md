# GGUF

最近，在社区中本地运行 LLM 变得越来越流行，其中使用 llama.cpp 处理 GGUF 文件是一个典型的例子。通过 llama.cpp，您不仅可以为自己的模型构建 GGUF 文件，还可以进行低比特量化操作。在 GGUF 格式下，您可以直接对模型进行量化而无需校准过程，或者为了获得更好的量化效果应用 AWQ scale，亦或是结合校准数据使用 imatrix 工具。在这篇文档中，我们将展示最简便的模型量化方法，以及如何在对 Qwen 模型进行量化时应用 AWQ 比例以优化其质量。

## 1. 生成 GGUF 模型文件

在进行量化操作之前，请确保你已经按照指导开始使用 llama.cpp。以下指引将不会提供有关安装和构建的步骤。现在，假设你要对 Qwen2-7B-Instruct 模型进行量化，首先需要按照如下所示的方式为 fp16 模型创建一个 GGUF 文件：

```bash linenums="1"
python convert-hf-to-gguf.py Qwen/Qwen2-7B-Instruct \  # (1)!
    --outfile models/7B/qwen2-7b-instruct-fp16.gguf  # (2)!
```

  1. 预训练模型所在的路径或者 HF 模型的名称
  2. 所生成的 GGUF 文件的路径

通过这种方式，你已经为你的 fp16 模型生成了一个 GGUF 文件，接下来你需要根据实际需求将其量化至低比特位。

```bash
./llama-quantize models/7B/qwen2-7b-instruct-fp16.gguf models/7B/qwen2-7b-instruct-q4_0.gguf q4_0
```

到现在为止，您已经完成了将模型量化为 4 比特，并将其放入 GGUF 文件中。这里的 `q4_0` 表示 4 比特量化。现在，这个量化后的模型可以直接通过 llama.cpp 运行。


