# 中文语言大模型

> [寻找那些ChatGPT/GPT4开源“平替”们](https://github.com/chenking2020/FindTheChatGPTer)

## 相关项目

| 模型                                                         | 描述                                                         | 地址                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| [Chinese-LLaMA-Alpaca](https://github.com/ymcui/Chinese-LLaMA-Alpaca) | **中文LLaMA模型和指令精调的Alpaca大模型**<u>授权比较有限，只能用作科研，不允许做商用。</u> | [github](https://github.com/ymcui/Chinese-LLaMA-Alpaca)      |
| ChatGLM                                                      | **ChatGLM是清华技术成果转化的公司智谱AI开源的GLM系列的对话模型，支持中英两个语种，目前开源了其62亿参数量的模型。** | [github](https://github.com/THUDM/ChatGLM-6B)                |
| Chinese-Vicuna                                               | 斯坦福学者继推出alpaca后，联手CMU、UC伯克利等，推出一个全新模型——130亿参数的Vicuna（俗称小羊驼、骆马） 另一个中文版的进行了开源Chinese-Vicuna ，github地址为： | [github](https://github.com/Facico/Chinese-Vicuna)           |
| [ChatGLM-6B](https://github.com/THUDM/ChatGLM-6B)            | ChatGLM-6B 是清华唐杰老师实验室释放出来的中文大语言(小)模型  |                                                              |
| [ptuning-v2](https://github.com/THUDM/ChatGLM-6B/tree/main/ptuning) | ptuning-v2是清华唐杰老师实验室发布对GLM的一种微调方法，实现了他们本身发布的p-tuning-v2的论文的方法 |                                                              |
| [GLM-Tuning](https://github.com/mymusise/ChatGLM-Tuning)     | [![Build](https://camo.githubusercontent.com/84f0493939e0c4de4e6dbe113251b4bfb5353e57134ffd9fcab6b8714514d4d1/68747470733a2f2f636f6c61622e72657365617263682e676f6f676c652e636f6d2f6173736574732f636f6c61622d62616467652e737667)](https://colab.research.google.com/github/mymusise/ChatGLM-Tuning/blob/master/examples/finetune.ipynb) 这是[Chengxi Guo](https://github.com/mymusise)等同学实现的GLM微调，最新的版本中同时支持了LoRA和p-tuning |                                                              |
| [Alpaca](https://github.com/tatsu-lab/stanford_alpaca)       | Alpaca是斯坦福在LLaMA上微调对话指令的项目，是万恶之源        |                                                              |
| [Alpaca-LoRA](https://github.com/tloen/alpaca-lora)          | 这个项目开启了LLaMA模型上的LoRA微调，万恶之源2               |                                                              |
| [Alpaca-ChToken](https://github.com/ymcui/Chinese-LLaMA-Alpaca) | 复旦的Yiming Cui和Ziqing Yang修复了Alpaca的中文token问题，在原来的LLaMA英文token边上并了一个中文的token，我们想把这个项目整合到整体训练里，还没做完 |                                                              |
| [BELLE-7B](https://github.com/LianjiaTech/BELLE)             | [![Open in Colab](https://camo.githubusercontent.com/84f0493939e0c4de4e6dbe113251b4bfb5353e57134ffd9fcab6b8714514d4d1/68747470733a2f2f636f6c61622e72657365617263682e676f6f676c652e636f6d2f6173736574732f636f6c61622d62616467652e737667)](https://colab.research.google.com/github/LianjiaTech/BELLE/blob/main/notebook/BELLE_INFER_COLAB.ipynb) BELLE是贝壳(链家)放出来的中文大模型，我们之后会尝试在这上面做微调。在一个合适的定量benchmark建立以后，我们会对比各个单卡大模型之间的性能。 |                                                              |
| [RWKV-LM](https://github.com/BlinkDL/RWKV-LM)                | RWKV也是一套语言模型的训练架构                               |                                                              |
| [Baize-7B](https://github.com/project-baize/baize-chatbot)   | 白泽是做连续对话的，他收集语料的方法很有意思，之后我们要看一下，但是白泽是在LLaMA上训练的，所以会遇到中文的问题，用到中文要换基模型。 |                                                              |
| [Vicuna](https://github.com/lm-sys/FastChat)                 | 同时有7B和13B，支持中文的模型，这个应该挺厉害的，而且13B用Int4能够压缩到colab使用（但是不知道int4训练会不会出事儿），之后也要试一下 |                                                              |
| [DeepSpeed](https://github.com/microsoft/DeepSpeed/tree/master/blogs/deepspeed-chat/chinese) | 微软开源的一个快速训练RLHF和全局finetune的一个框架           |                                                              |
| [Phoenix](https://github.com/FreedomIntelligence/LLMZoo)     | 港中文深圳的老师同学们发布的Phoenix模型，拥有宽松，支持商业的开源协议. |                                                              |
| [中文OpenInstruct](https://github.com/flagopen/flaginstruct) | 北京智源老师们准备开源出来的数据集.                          |                                                              |
| Luotuo                                                       | 骆驼(Luotuo)项目是由[冷子昂](https://blairleng.github.io/) @ 商汤科技, 陈启源 @ 华中师范大学 以及 李鲁鲁 @ 商汤科技 发起的中文大语言模型开源项目，包含了一系列语言模型。 | [github](https://github.com/LC1332/Luotuo-Chinese-LLM#contributors) |

| 数据                                                         | 详情                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [Guanaco](https://huggingface.co/datasets/JosephusCheung/GuanacoDataset) | Guanaco是JosephusCheung制作的一套指令调优的数据集，在骆驼0.3以上版本的模型中我们使用了这个数据。 |
| [CNewSum](https://dqwang122.github.io/projects/CNewSum/)     | CNewSum是字节与UCSB发布的中文摘要数据集，我们在驼铃-C模型中使用了这个数据集 |
| [Coco-CN](https://github.com/li-xirong/coco-cn)              | 这是中国人民大学的li-xirong等翻译的部分Coco数据集，**骆驼团队正在准备用GPT翻译完整的Coco,如果你也准备翻译，可以联系我们，避免重复花钱** |
| [CoQA](https://stanfordnlp.github.io/coqa/)                  | 基于一段文字，然后问答，是个很重要的任务。陈丹琦大佬参与做的CoQA数据集，**骆驼团队正在准备用GPT增广和翻译完整的CoQA,如果你也准备翻译，可以联系我们，避免重复花钱** |