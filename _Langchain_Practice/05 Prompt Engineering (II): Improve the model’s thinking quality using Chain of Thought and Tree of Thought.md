## What is Chain of Thought

The concept of CoT (Chain of Thought) originates from academia. It was introduced by Jason Wei and others from Google Brain in their 2022 paper titled 'Chain-of-Thought Prompting Elicits Reasoning in Large Language Models.

It suggests that generating a series of intermediate reasoning steps can significantly enhance the ability of large language models to perform complex reasoning.

### Few-Shot CoT

Few-Shot CoT simply provides some chain-of-thought examples in the prompt. This can enhance the reasoning ability of sufficiently large language models. In simple terms, it means giving one or two examples and clearly writing out the reasoning process in those examples.

![](images/701505/f27cec109dff8947d85507b34ce240a0.png)

In experiments with three large language models, CoT improved performance across a range of arithmetic, common sense, and symbolic reasoning tasks. In the GSM8K math problem benchmark, CoT guidance enabled the large model to achieve state-of-the-art accuracy at the time.

CoT is conceptually very easy to understand and practically very easy to implement. Although simple, this idea can provide a lot of inspiration for our development process.

For example, suppose we are developing an AI flower shop assistant whose task is to help users choose the flowers they want and generate a sales list. In this process, we can use CoT to guide the AI’s reasoning process.

👉 Overall guidance: You need to follow the steps below to reason step by step.

1. Problem understanding: First, the AI needs to understand the user’s needs. For example, the user might say, ‘I am going to a friend’s birthday party today and want to send a bouquet to wish her well.’ We can give the AI a prompt template that includes an example: ‘When faced with XX problem, I first check if I have any relevant knowledge. If I do, I provide the answer; if not, I use tools to search for information and then try to solve it.’ — This provides the AI with an example of a chain of thought.

2. Information search: Next, the AI needs to search for relevant information. For example, it might need to find out which flowers are most suitable for a birthday party.

3. Decision making: Based on the collected information, the AI needs to make a decision. We can use the chain of thought to guide it through the decision-making process, step by step. For example, we can give it an example: ‘When faced with the situation of sending flowers for a birthday party, I first consider the user’s needs, then check the flower inventory, and finally decide to recommend some roses and lilies because these flowers are usually suitable for birthday parties.’ — With this birthday party scenario as an example, the large model can apply similar thinking processes to other scenarios.

4. Generate sales list: Finally, the AI uses the OutputParser to generate a sales list, including the recommended flowers and prices.

In this process, overall, the chain of thought guides the AI from understanding the problem, to searching for information, to making decisions, and finally generating the sales list. This method not only makes the AI’s reasoning process clearer but also ensures that the generated sales list better meets the user’s needs. For each step, more detailed prompt templates can be designed using the chain of thought to guide the model’s thinking with clear and accurate logic.

In fact, the essence of LangChain’s core component, Agent, is to perform good prompt engineering and extensively use pre-set Few-Shot and CoT templates. We will understand this more thoroughly in the subsequent lessons.

> [!IMPORTANT]
> The core component of LangChain, Agent, essentially performs good prompt engineering and extensively uses pre-set Few-Shot and CoT templates.

### Zero-Shot CoT

Few-Shot CoT refers to incorporating reasoning steps into the prompt with examples, guiding the model to produce better results. Zero-Shot CoT, on the other hand, involves directly instructing the model to think step-by-step and reason slowly.

## Chain of Thought Practice

Project Requirements: In this example, you are developing an AI operations assistant. We need to demonstrate how the AI reasons and generates answers based on user needs. Then, the AI reasons based on the current user request, providing specific flower suggestions and explaining why these suggestions were chosen.

In this process, the AI needs to understand the customer’s needs, think step-by-step, and then provide the most logical response.

### CoT template design

### Program full framework

```plain
# 设置环境变量和API密钥
import os
os.environ["OPENAI_API_KEY"] = '你的OpenAI API Key'

# 创建聊天模型
from langchain.chat_models import ChatOpenAI
llm = ChatOpenAI(temperature=0)

# 设定 AI 的角色和目标
role_template = "你是一个为花店电商公司工作的AI助手, 你的目标是帮助客户根据他们的喜好做出明智的决定"

# CoT 的关键部分，AI 解释推理过程，并加入一些先前的对话示例（Few-Shot Learning）
cot_template = """
作为一个为花店电商公司工作的AI助手，我的目标是帮助客户根据他们的喜好做出明智的决定。

我会按部就班的思考，先理解客户的需求，然后考虑各种鲜花的涵义，最后根据这个需求，给出我的推荐。
同时，我也会向客户解释我这样推荐的原因。

示例 1:
  人类：我想找一种象征爱情的花。
  AI：首先，我理解你正在寻找一种可以象征爱情的花。在许多文化中，红玫瑰被视为爱情的象征，这是因为它们的红色通常与热情和浓烈的感情联系在一起。因此，考虑到这一点，我会推荐红玫瑰。红玫瑰不仅能够象征爱情，同时也可以传达出强烈的感情，这是你在寻找的。

示例 2:
  人类：我想要一些独特和奇特的花。
  AI：从你的需求中，我理解你想要的是独一无二和引人注目的花朵。兰花是一种非常独特并且颜色鲜艳的花，它们在世界上的许多地方都被视为奢侈品和美的象征。因此，我建议你考虑兰花。选择兰花可以满足你对独特和奇特的要求，而且，兰花的美丽和它们所代表的力量和奢侈也可能会吸引你。
"""
from langchain.prompts import ChatPromptTemplate, HumanMessagePromptTemplate, SystemMessagePromptTemplate
system_prompt_role = SystemMessagePromptTemplate.from_template(role_template)
system_prompt_cot = SystemMessagePromptTemplate.from_template(cot_template)

# 用户的询问
human_template = "{human_input}"
human_prompt = HumanMessagePromptTemplate.from_template(human_template)

# 将以上所有信息结合为一个聊天提示
chat_prompt = ChatPromptTemplate.from_messages([system_prompt_role, system_prompt_cot, human_prompt])

prompt = chat_prompt.format_prompt(human_input="我想为我的女朋友购买一些花。她喜欢粉色和紫色。你有什么建议吗?").to_messages()

# 接收用户的询问，返回回答结果
response = llm(prompt)
print(response)

```

### Tree of Thought

ToT is a framework for solving complex problems. It guides language models to search a tree of coherent language sequences (intermediate steps to solve the problem) rather than simply generating an answer. The core idea of the ToT framework is to enable the model to generate and evaluate its reasoning capabilities and combine them with search algorithms (such as breadth-first search and depth-first search) for systematic exploration and verification.

![](images/701505/6eec83ffe1a5f37d245520535d65f8a0.png)

The ToT framework defines specific thinking steps and the number of candidates for each step for each task. For example, to solve a mathematical reasoning task, it first breaks it down into three thinking steps, proposes multiple solutions for each step, and retains the top five candidate solutions. Then, it searches for the optimal solution among multiple thinking paths.

The advantage of this method is that the model can better solve problems by observing and evaluating its own thinking process, rather than just generating output based on input. This is very useful for complex tasks that require deep reasoning. Additionally, by introducing techniques such as reinforcement learning and beam search, the performance of search strategies can be further improved, allowing the model to perform better when solving new problems or facing unknown situations.

The ToT framework provides a new approach to solving complex problems by combining the generative capabilities of language models, search algorithms, and reinforcement learning to achieve better results.

## Summary

The concepts of Chain of Thought (CoT) and Tree of Thoughts (ToT) are very interesting and explore how to guide large language models for deeper reasoning.

- The core idea of CoT is to enhance the model’s reasoning ability by generating a series of intermediate reasoning steps. In the two application methods, Few-Shot CoT and Zero-Shot CoT, the former provides chain-thinking examples to the model, while the latter directly instructs the model to reason step-by-step.
- ToT further expands on the idea of CoT by solving complex problems through searching a tree of coherent language sequences. I demonstrated how to use the ToT framework in practical applications with an example of flower selection.


