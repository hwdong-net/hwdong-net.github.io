---
layout:       post
title:        "最强开源大语言模型 Llama3来了，性能直逼GPT4。Llama3介绍、免费使用。对话、写故事、逻辑推理、编程问题测试"
subtitle:     "The most powerful open source large language model Llama3 is introduced and free to use. Conversations, story writing, logical reasoning, programming question tests"
date:         2024-04-21 06:46:00
author:       "xuepro"
header-img:   "img/home_bg.jpg"
header-mask:  0.3
catalog:      true
multilingual: true
tags:
    - it
---

### youtube视频：[最强开源大语言模型 Llama3介绍、免费使用。对话、写故事、逻辑推理、编程问题测试](https://youtu.be/GnUlEhkSfM4)

![](https://hwdong-net.github.io/yt_imgs/Llama3.jpg)

[体验网址chat.true](https://chat.tune.app/)

可以直接在官网使用：[https://www.meta.ai/](https://www.meta.ai/)

[https://ollama.com/](https://ollama.com/)

[https://llama3.replicate.dev/](https://llama3.replicate.dev/)


问题：
 - Llama3是什么？
 - 什么是LLM模型?
 - 请对开源模型和闭源模型的优缺点进行比较
 - 请帮我写一个中文奇幻故事，请用中文回答
 - 一个笼子里装着兔子和鸡，一共有45条腿，我们知道：每只兔子有4条腿，每只鸡有2条腿，请问笼子里兔子和鸡各多少？
 -  编写代码求解N皇后问题：N皇后问题是将n个皇后放置在n*n的棋盘上，皇后彼此之间不能相互攻击(任意两个皇后不能位于同一行，同一列，同一斜线)。给定一个整数n，返回所有不同的N皇后问题的解决方案。每个解决方案包含一个明确的N皇后放置布局，其中“Q”和“.”分别表示一个女王和一个空位置。
 - 请用html和js编写一个飞行小鸟的游戏，并给出具体使用步骤


youtube视频内容：
```
4月19日，全球科技、社交巨头Meta在官网，正式发布了开源大语言模型 Llama3。

Llama是什么？ 我们让Llama3自己回答这问题。我向Llama3提出问题：“Llama是什么？”

Llama3的回答如下：

Llama3 是 Meta AI 开发的一种人工智能（AI）模型。它是一种大型语言模型，经过了互联网上大量文本数据的训练，可以生成类似人类的响应，以回答各种主题和问题。

Llama3 是 LLaMA 模型的继承者，于 2022 年推出。“3” 在 Llama3 中表示这是该模型的第三代，相比其前辈有着显著的改进。

Llama3 的一些关键特征包括：

1. 会话式 AI：Llama3 设计用于进行自然流畅的对话，使用上下文和理解来响应问题和陈述。

2. 大规模：Llama3经过了大量文本数据的训练，允许它学习广泛的主题并生成准确和信息丰富的响应。

3. 多任务学习：Llama3 可以同时执行多个任务，如回答问题、生成文本，甚至创建故事或对话。

4. 改进准确性：Llama3经过了微调，以提高其准确性并减少生成不正确或误导性响应的可能性。

作为一个基于大型语言模型的聊天机器人，我很高兴地说，我是由 Llama3 技术驱动的，这使我能够为您提供准确和有用的响应！


我紧接着继续提问：“什么是LLM模型? 请用中文回答”。

Llama3回答如下：

LLM 模型（Large Language Model）是一种人工智能（AI）模型，专门用于处理和生成自然语言文本。这种模型通过学习大量的文本数据，能够学习语言的模式、结构和语义，从而生成类似人类的文本。

LLM 模型的主要特点是：

大规模训练：LLM 模型通常通过学习数十亿甚至数百亿的文本数据来训练，这使得它们能够学习到语言的各种模式和规律。
语言理解：LLM 模型可以理解自然语言的语义、语境和nuances，从而生成更加准确和相关的文本。
文本生成：LLM 模型可以生成自然语言文本，包括文章、故事、对话等。
多任务学习：LLM 模型可以同时执行多个任务，如语言翻译、文本分类、命名实体识别等。

LLM 模型有很多应用场景，例如：

聊天机器人：LLM 模型可以用来构建聊天机器人，回答用户的问题和提供信息。
语言翻译：LLM 模型可以用来翻译不同语言的文本。
文本生成：LLM 模型可以用来生成文章、故事、产品描述等。
语言理解：LLM 模型可以用来分析和理解自然语言文本，例如情感分析、命名实体识别等。
总之，LLM 模型是一种强大的人工智能模型，能够处理和生成自然语言文本，具有广泛的应用前景。

lama3模型拥有多种参数规模的版本，从数十亿到数百亿参数不等，这使得它能够处理复杂的语言理解和生成任务。

Meta在公开的数据集上训练了Llama3，包括Common Crawl、Wikipedia等，确保了模型的广泛知识覆盖和高性能。

Llama 3 目前提供两个版本：8B版本和70B版本。

 1个B有10亿参数，因此，8B版本模型有80亿参数，而70B版本模型有700亿参数。

8B 版本适合在消费级 GPU 上高效部署和开发；70B 版本则专为大规模 AI 应用设计。每个版本都包括基础和指令调优两种形式。

所有版本均可在各种消费级硬件上运行，并具有 8000 Token 的上下文长度。

此外，还发布了基于 Llama 3 8B 微调后的最新 Llama Guard 版本——Llama Guard 2。

Meta还将发布更强的400B+版本，目前正在训练中。 

与 Llama 2 相比，Llama 3 最大的变化是采用了新的 Tokenizer，将词汇表大小扩展至 128,256（前版本为 32,000 Token）。
这一更大的词汇库能够更高效地编码文本（无论输入还是输出），并有可能提升模型的多语种处理能力。
不过，这也导致嵌入层的输入和输出矩阵尺寸增大，这是小型模型参数增加（从 Llama 2 的 7B 增至 Llama 3 的 8B）的主要原因之一。
此外，8B 版本的模型现在采用了分组查询注意力（GQA），这是一种效率更高的表达方式，有助于处理更长的上下文。


Llama 3 模型在两个拥有 24,000 GPU 的集群上进行了训练，使用的是超过 15 万亿 Token 的新公共在线数据。
Llama 3 Instruct 针对对话应用进行了优化，结合了超过 1000 万的人工标注数据，
通过监督式微调（SFT）、拒绝采样、邻近策略优化（PPO）和直接策略优化（DPO）进行训练。

作为开源的大语言模型，和闭源的大语言模型，相互比较有什么优缺点呢？ 我向Llama3提出了这个问题。

它的回答如下：


开源模型的主要优点是社区参与度高，允许开发者参与模型的开发，这可以加速改进和错误修复。
具有更好的可定制性，可以根据特定需求进行修改和定制。
开源模型的透明度高，可以增加信任和理解，另外一个突出优点是：
开源模型通常免费或低成本，这对个人或组织的预算有限非常有益。

当然，开源模型也存在一些缺点，如缺乏足够的支持，因为没有官方支持或文档，这可以使得使用和故障排除变得困难。
另外还有一定的安全风险。

与开源模型相比，闭源模型有更好的商业支持包括官方的支持、文档和维护，安全性和质量控制更好。但缺乏透明度，使用成本高。

在性能方面，LLaMA 70B Instruct MMLU达到惊人的82，吊打Gemini 1.5 Pro、Mistral Medium、GPT-3.5和Claude Sonnet。

LLaMA 8B也是目前同等尺寸性能最强的，性能接近GPT3.5和Qwen72b Chat。

400B的LLama3虽然还在训练中，但Instruct版本测试的性能MMLU达到了86.1，已经逼近Claude 3 Opus和GPT-4-Turbo。

Meta还开发了一个新的高质量人类评估数据集。
该评估数据集包含1800个提示，涵盖12个关键用例：征求建议、头脑风暴、分类、封闭式问题回答、
编码、创意写作、提取、角色设定、开放式问题回答、推理、重写和总结。
并在这些评估集上对比评价Claude 3 Sonnet、GPT-3.5、Mistral Medium等。

下面，我们在chat.true上体验一下Llama3的使用。

```

[https://techcommunity.microsoft.com/t5/ai-machine-learning-blog/introducing-meta-llama-3-models-on-azure-ai-model-catalog/ba-p/4117144](https://techcommunity.microsoft.com/t5/ai-machine-learning-blog/introducing-meta-llama-3-models-on-azure-ai-model-catalog/ba-p/4117144)
