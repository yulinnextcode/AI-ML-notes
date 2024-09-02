We spent two lessons thoroughly explaining the principles of prompt engineering and the specific usage of LangChain. Today, we will focus on discussing the second submodule in Model I/O, LLM.

![](images/702140/cd7e1506af5b6a8e382c2c9eab4d7481.jpg)

## LLM History

Google’s 2018 seminal paper “Attention is All You Need” proposed the Transformer architecture, which ignited the current AI boom. The Transformer is the core underlying architecture of almost all pre-trained models. Large-scale language models pre-trained based on the Transformer are also called “foundation models” or “base models.”

In this process, the model learns rich linguistic knowledge such as vocabulary, grammar, sentence structure, and contextual information. This knowledge, learned from large amounts of data, provides a general and rich linguistic representation foundation for subsequent downstream tasks (such as sentiment analysis, text classification, named entity recognition, question-answering systems, etc.), making it possible to solve many complex NLP problems.

In the early days of pre-trained models, BERT was undoubtedly the most representative and influential model. BERT achieves a deep understanding of sentence structure by simultaneously learning the forward and backward contextual information of the text. After BERT, various large pre-trained models have emerged like mushrooms after rain, ushering in a new era in the field of natural language processing (NLP). These models have driven the rapid development of NLP technology, solving many previously difficult problems such as translation, text summarization, and conversational dialogue, providing powerful tools.

![](images/702140/7f1108deceaa4b5281ed431598f1b0a6.jpg)

![](images/702140/95828d4e2234e7130bb2d500455092ef.jpg)

## Pretrained + fine-tuning

However, that being said, large pre-trained models are indeed a boon for engineers. The semantic information and linguistic knowledge acquired by these pre-trained large models can be easily transferred to downstream tasks. NLP practitioners can adapt the model’s head or some parameters according to their needs, which usually involves supervised learning on relatively small labeled datasets to make the model meet the specific requirements of the task.

This is the fine-tuning of pre-trained models. The fine-tuning process is much faster than training a model from scratch and requires much less data, allowing us as engineers to develop and deploy various NLP solutions more efficiently.

![](images/702140/75edd66d67ec8a20326b69514c9d9daf.jpg)

- Pre-training: Training the model on large-scale unlabeled text data with the goal of enabling the model to learn the fundamental expressions, contextual information, and semantic knowledge of natural language, providing a general and rich linguistic representation foundation for subsequent tasks.

- Fine-tuning: Based on the pre-trained model, the model can be fine-tuned according to specific downstream tasks. Nowadays, you often hear people from various industries say: “Our advantage is domain knowledge! We can’t compete with large models from home and abroad, but we can use open-source models for vertical domains! Create vertical models!”— What is a vertical model? It actually refers to fine-tuning open-source models based on domain-specific data.

The application model of pre-training + fine-tuning has obvious advantages. First, pre-trained models can transfer a large amount of general language knowledge to various downstream tasks. As practitioners, we don’t need to find corpora and train large models from scratch, reducing training time and data requirements. Second, the fine-tuning process can quickly optimize the model according to specific tasks, simplifying the difficulty of model deployment. Finally, the pre-training + fine-tuning architecture is highly scalable and can be easily applied to various natural language processing tasks, greatly improving the usability and popularity of NLP technology in practical applications, bringing us great convenience.

## Run open source LLM using HuggingFace

### Register and install HuggingFace

Step 1: First, log in to the HuggingFace website and obtain your unique Token. (If you have completed the practical examples in the previous lessons, you should already have this API Token.)

Step 2: Install the HuggingFace Library using pip install transformers. For more details, see here.

Step 3: Run huggingface-cli login in the command line to set up your API Token.

![](images/702140/5fa0c088652c8776f5ec50a059b1b1e6.png)

```plain
# 导入HuggingFace API Token
import os
os.environ['HUGGINGFACEHUB_API_TOKEN'] = '你的HuggingFace API Token'

```

### Apply use Meta Llama2 model

After selecting the meta-llama/Llama-2-7b model, you will be able to see the basic information of this model. If you are using Llama for the first time, you need to apply for access.

### Use Llama by HuggingFace

We can use HuggingFace’s Transformers library to call Llama!

```plain
# 导入必要的库
from transformers import AutoTokenizer, AutoModelForCausalLM

# 加载预训练模型的分词器
tokenizer = AutoTokenizer.from_pretrained("meta-llama/Llama-2-7b-chat-hf")

# 加载预训练的模型
# 使用 device_map 参数将模型自动加载到可用的硬件设备上，例如GPU
model = AutoModelForCausalLM.from_pretrained(
          "meta-llama/Llama-2-7b-chat-hf",
          device_map = 'auto')

# 定义一个提示，希望模型基于此提示生成故事
prompt = "请给我讲个玫瑰的爱情故事?"

# 使用分词器将提示转化为模型可以理解的格式，并将其移动到GPU上
inputs = tokenizer(prompt, return_tensors="pt").to("cuda")

# 使用模型生成文本，设置最大生成令牌数为2000
outputs = model.generate(inputs["input_ids"], max_new_tokens=2000)

# 将生成的令牌解码成文本，并跳过任何特殊的令牌，例如[CLS], [SEP]等
response = tokenizer.decode(outputs[0], skip_special_tokens=True)

# 打印生成的响应
print(response)

```

This program is a typical example of using HuggingFace’s Transformers library, which provides a large number of pre-trained models and related tools.

- Import AutoTokenizer: This is a tool for automatically loading the tokenizer related to the pre-trained model. The tokenizer is responsible for converting text into a numerical format that the model can understand.
- Import AutoModelForCausalLM: This is a tool for loading causal language models (used for text generation).
- Use the from_pretrained method to load the pre-trained tokenizer and model. Here, device_map = 'auto' is used to automatically load the model onto available devices, such as GPUs.
- Then, given a prompt: "Please tell me a love story about a rose?", use the tokenizer to convert the prompt into a format that the model can accept, with return_tensors="pt" indicating that PyTorch tensors are returned. The statement .to("cuda") is for GPU device format conversion, as I am running the program on a GPU. If you are using a CPU, you can try removing it.
- Finally, use the model’s .generate() method to generate a response. max_new_tokens=2000 limits the length of the generated text. Use the tokenizer’s .decode() method to convert the output numbers back into text, skipping any special tokens.

## LangChain and HuggingFace Interface

After all this talk, LangChain hasn’t made an appearance yet. Now let’s take a look at how to integrate models from HuggingFace into LangChain.

### Use HuggingFace Hub

The first integration method is through HuggingFace Hub. HuggingFace Hub is an open-source centralized repository mainly used for sharing, collaborating, and storing pre-trained models, datasets, and related components.

We provide a code example for integrating HuggingFace Hub with LangChain.

```plain
# 导入HuggingFace API Token
import os
os.environ['HUGGINGFACEHUB_API_TOKEN'] = '你的HuggingFace API Token'

# 导入必要的库
from langchain import PromptTemplate, HuggingFaceHub, LLMChain

# 初始化HF LLM
llm = HuggingFaceHub(
    repo_id="google/flan-t5-small",
    #repo_id="meta-llama/Llama-2-7b-chat-hf",
)

# 创建简单的question-answering提示模板
template = """Question: {question}
              Answer: """

# 创建Prompt
prompt = PromptTemplate(template=template, input_variables=["question"])

# 调用LLM Chain --- 我们以后会详细讲LLM Chain
llm_chain = LLMChain(
    prompt=prompt,
    llm=llm
)

# 准备问题
question = "Rose is which type of flower?"

# 调用模型并返回结果
print(llm_chain.run(question))

```

### HuggingFace Pipeline

Since HuggingFace Hub cannot yet complete the testing of Llama-2, let’s try another method, HuggingFace Pipeline. HuggingFace’s Pipeline is an advanced tool that simplifies the usage process of many common natural language processing (NLP) tasks, making it easy for users to utilize pre-trained models for tasks without needing to understand the model details in depth.

Let’s take a look at the following example:

````plain
# 指定预训练模型的名称
model = "meta-llama/Llama-2-7b-chat-hf"

# 从预训练模型中加载词汇器
from transformers import AutoTokenizer
tokenizer = AutoTokenizer.from_pretrained(model)

# 创建一个文本生成的管道
import transformers
import torch
pipeline = transformers.pipeline(
    "text-generation",
    model=model,
    torch_dtype=torch.float16,
    device_map="auto",
    max_length = 1000
)

# 创建HuggingFacePipeline实例
from langchain import HuggingFacePipeline
llm = HuggingFacePipeline(pipeline = pipeline,
                          model_kwargs = {'temperature':0})

# 定义输入模板，该模板用于生成花束的描述
template = """
              为以下的花束生成一个详细且吸引人的描述：
              花束的详细信息：
              ```{flower_details}```
           """

# 使用模板创建提示
from langchain import PromptTemplate,  LLMChain
prompt = PromptTemplate(template=template,
                     input_variables=["flower_details"])

# 创建LLMChain实例
from langchain import PromptTemplate
llm_chain = LLMChain(prompt=prompt, llm=llm)

# 需要生成描述的花束的详细信息
flower_details = "12支红玫瑰，搭配白色满天星和绿叶，包装在浪漫的红色纸中。"

# 打印生成的花束描述
print(llm_chain.run(flower_details))

````

At this point, the attempt to call various open-source models through the HuggingFace interface has successfully concluded. Next, let’s conduct the final test to see if LangChain can directly call local models.

HuggingFaceHub and transformers.pipeline are both tools in the Hugging Face ecosystem that can be used to access and work with machine learning models: 
HuggingFaceHub
Allows users to access hosted inference endpoints for HuggingFace models. Users can also check and compare model results directly from their browser on the Hub. 
Transformers.pipeline
A function that provides a user-friendly interface to access pre-trained models for natural language processing (NLP) tasks. It can be used for tasks such as text generation, question answering, and summarization. 
Here are some more details about each tool: 
Transformers.pipeline
The pipeline() function is zero-shot, meaning it doesn't require fine-tuning the model on a dataset. It also supports multiple languages and has task-specific capabilities. 
HuggingFaceHub
Users can also train their own models if they don't find one that fits their use case. 
Hugging Face ecosystem
The Hugging Face ecosystem includes a collection of code that makes it easier to work with Transformers and other machine learning models. 
Transformers library
A machine learning library that provides tools and APIs for downloading and training pretrained models.

## Use LangChain to call self-defined local LM

Suppose you want to train your own model. For reasons of commercial secrecy, you don’t want to open-source it or upload it to HuggingFace; you just want to run the model locally. How should you utilize LangChain’s features in this case?

We can create a derived class of LLM and define the model ourselves. The LLM base class is located in langchain.llms.base and can be imported using the statement from langchain.llms.base import LLM.

This custom LLM class only needs to implement one method:

- _call method: Used to receive an input string and return a response string.

And an optional method:

- _identifying_params method: Used to help print the attributes of this class.

```plain
# 导入需要的库
from llama_cpp import Llama
from typing import Optional, List, Mapping, Any
from langchain.llms.base import LLM

# 模型的名称和路径常量
MODEL_NAME = 'llama-2-7b-chat.ggmlv3.q4_K_S.bin'
MODEL_PATH = '/home/huangj/03_Llama/'

# 自定义的LLM类，继承自基础LLM类
class CustomLLM(LLM):
    model_name = MODEL_NAME

    # 该方法使用Llama库调用模型生成回复
    def _call(self, prompt: str, stop: Optional[List[str]] = None) -> str:
        prompt_length = len(prompt) + 5
        # 初始化Llama模型，指定模型路径和线程数
        llm = Llama(model_path=MODEL_PATH+MODEL_NAME, n_threads=4)
        # 使用Llama模型生成回复
        response = llm(f"Q: {prompt} A: ", max_tokens=256)

        # 从返回的回复中提取文本部分
        output = response['choices'][0]['text'].replace('A: ', '').strip()

        # 返回生成的回复，同时剔除了问题部分和额外字符
        return output[prompt_length:]

    # 返回模型的标识参数，这里只是返回模型的名称
    @property
    def _identifying_params(self) -> Mapping[str, Any]:
        return {"name_of_model": self.model_name}

    # 返回模型的类型，这里是"custom"
    @property
    def _llm_type(self) -> str:
        return "custom"


# 初始化自定义LLM类
llm = CustomLLM()

# 使用自定义LLM生成一个回复
result = llm("昨天有一个客户抱怨他买了花给女朋友之后，两天花就枯了，你说作为客服我应该怎么解释？")

# 打印生成的回复
print(result)

```

## Summary

Training large models involves using deep learning algorithms on vast amounts of data, typically requiring significant computational resources and time. After training, the model may not be fully suited to specific tasks, so fine-tuning is needed, which involves continuing training on a specific dataset to better adapt the model to the task. To reduce the size of the deployed model and speed up inference, the model is also quantized, which reduces the model parameters from high precision to lower precision.

If you want to continue learning about large models, there are several tools you must study further:

- PyTorch: A popular deep learning framework commonly used for training and fine-tuning models.

- HuggingFace: An open-source community that provides a large number of pre-trained models and fine-tuning tools, especially for NLP tasks.

- LangChain: Excels at leveraging the inference capabilities of large language models to develop new tools or applications for specific tasks.

These tools and libraries play a crucial role throughout the AI model lifecycle, making it easier for researchers and developers to develop and deploy efficient AI systems.






















