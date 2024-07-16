# GPTQ

GPTQ 是一种针对类 GPT 大型语言模型的量化方法，它基于近似 **二阶信息** 进行一次性权重量化。在本文档中，我们介绍下如何使用 `transformers` 库加载并应用量化后的模型，以及如何通过 AutoGPTQ 来对您自己的模型进行量化处理。

## 1. 在 Transformers 中使用 GPTQ 模型

现在，Transformers 正式支持了 AutoGPTQ，这意味着我们能够直接在 Transformers 中使用量化后的模型。

```python linenums="1"
from transformers import AutoModelForCausalLM, AutoTokenizer


device = "cuda"
model = AutoModelForCausalLM.from_pretrained(
    "Qwen/Qwen2-7B-Instruct-GPTQ-Int8",
    device_map="auto"
)
tokenizer = AutoTokenizer.from_pretrained("Qwen/Qwen2-7B-Instruct-GPTQ-Int8")
prompt = "Give me a short introduction to large language model."
messages = [
    {"role": "system", "content": "You are a helpful assistant."},
    {"role": "user", "content": prompt}
]
text = tokenizer.apply_chat_template(
    messages,
    tokenize=False,
    add_generation_prompt=True
)
model_inputs = tokenizer([text], return_tensors="pt").to(device)

generated_ids = model.generate(
    model_inputs.input_ids,
    max_new_tokens=512
)
generated_ids = [
    output_ids[len(input_ids):] 
    for input_ids, output_ids in zip(model_inputs.input_ids, generated_ids)
]

response = tokenizer.batch_decode(
        generated_ids, skip_special_tokens=True
        )[0]
```

## 2. 在 vLLM 中使用 GPTQ 模型

vLLM 已经支持了 GPTQ，这意味着我们可以直接使用 GPTQ 模型，或者将 AutoGPTQ 训练得到的模型与 vLLM 结合使用。实际上，其用法与 vLLM 的基本用法相同。

???+ note "使用 GPTQ 模型"

    === "运行 vLLM 服务"

        ```bash
        python -m vllm.entrypoints.openai.api_server --model Qwen/Qwen2-7B-Instruct-GPTQ-Int8
        ```
    
    === "CURL发送请求"

        ```bash linenums="1"
        curl http://localhost:8000/v1/chat/completions \
          -H "Content-Type: application/json" \ 
          -d '{
            "model": "Qwen/Qwen2-7B-Instruct-GPTQ-Int8",
            "messages": [
              {"role": "system", "content": "You are a helpful assistant."},
              {"role": "user", "content": "Tell me something about large language models."}
            ],
          }'
        ```
    
    === "Python发送请求"

        ```python linenums="1"
        from openai import OpenAI

        openai_api_key = "EMPTY"
        openai_api_base = "http://localhost:8000/v1"

        client = OpenAI(
            api_key=openai_api_key,
            base_url=openai_api_base,
        )

        chat_response = client.chat.completions.create(
            model="Qwen/Qwen2-7B-Instruct-GPTQ-Int8",
            messages=[
                {"role": "system", "content": "You are a helpful assistant."},
                {"role": "user", "content": "Tell me something about large language models."},
            ]
        )
        print("Chat response:", chat_response)
        ```

## 3. 使用 AutoGPTQ 量化你的模型

如果你想将微调后的模型量化为 GPTQ 模型，我们建议你使用 AutoGPTQ 工具。推荐通过安装源代码的方式获取并安装最新版本的该软件包。




