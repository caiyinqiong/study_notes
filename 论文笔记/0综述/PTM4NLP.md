# Recent Advances in Natural Language Processing via Large Pre-Trained Language Models: A Survey



### 1. Introduction

使用PTMs4NLP的4种范式：1）2）3）4）数据增强



### 2. Paradigm 1: Pre-Train then Fine-Tune

##### 2.1 The Beginnings of the Paradigm Shift

- 2010年，预训练就已经在机器学习尤其是CV领域被研究。
- 2008年，首次提出预训练一个语言模型的想法。
- 2018年，Pre-training and ﬁne-tuning开始流行（ELMo，ULMFiT）
- 2018年末：transformer

##### 2.2 Modern Pre-Trained Language Models

- Autoregressive Language Models（e.g., GPT）: GPT only utilizes the autoregressive **decoder** portion of the Transformer architecture。GPT是微调，GPT-2和GPT-3是prompt或者语言生成的方式用于下游任务。
- Masked Language Models（e.g., BERT）：BERT use the **encoder** portion of the Transformer architecture.
- Encoder-Decoder Language Models（e.g., BART，T5）：sequence corruption + reconstruction

##### 2.3 Pre-Training Corpora

- size，diversity，quality，intended downstream applications

##### 2.4 Fine-Tuning: Applying PLMs to NLP Tasks 【四种微调的方式】

- **Contextual Embeddings**：“freeze” the model and use its output as sophisticated, context-sensitive word embeddings。

  适用场景：

  - insufﬁcient labeled data or compute power, 
  - Highly complex or difﬁcult NLP tasks often make use of the frozen PLM technique to help reduce training complexity.
  - Unsupervised tasks such as word sense disambiguation and word sense induction 

- **Fine-tuning the PLM**：ﬁne-tunes some or all the layers of the PLM and then adds one or two simple output layers。

  适用场景：sequence classiﬁcation tasks；sequence tagging tasks

- **Fine-tuning the PLM in Customized Models**：Some tasks require signiﬁcant additional architecture on top of a language model

  适用场景：structure prediction tasks

- **Efﬁcient Fine-tuning Approaches**

  - ﬁne-tuning a separate, small network that is tightly coupled with the PLM：Adapters；side-tuning；diff-pruning
  - selecting only a small number of the PLM’s weights to ﬁne-tune or keep：BitFit



### 3. Paradigm 2: Prompt-based Learning

使用prompt的优势：

- 高效：may not require updates to the PLM’s parameters, reducing computational requirements as compared to ﬁne-tuning approaches
- 与预训练对齐：Prompts also encourage a better alignment of the new task formulation with the pre-training objective, leading to better use of knowledge captured in pre-training
- few-shot：The closer match also enables a few-shot approach (Liu et al., 2021b), especially for tasks with small training datasets

【分为以下三种方法】

##### 3.1 Learning from Instructions and Demonstrations：in-context learning（e.g., GPT-3）

##### 3.2 Template-based Learning：reformulates NLP tasks into tasks that are closer to language models’ pre-training tasks via template-based prompts. （ﬁlling the slots + project output to labels）

- Template Design：1）cloze-style prompt；2）Preﬁx prompts
- Template Construction：1）Manually-crafted Templates；2）Automatically-generated Discrete Templates；3）Automatically-generated Continuous Templates；4）Multi-Prompt Learning
- Answer Generation：1）a classiﬁcation label；2）text answer
- Task-speciﬁc Tuning：1）a ﬁxed template-style prompt to perform tuning of the PLM；2）joint tuning of the prompt and the PLM
- Applications of Template-based Methods

##### 3.3 Learning from Proxy Tasks

- Question Answering as Proxy Task
- Textual Entailment as Proxy Task



### 4. Paradigm 3: NLP as Text Generation

leveraging generative PLMs to solve various non-generative NLP tasks, these tasks are reformulated as text generation problems so that they can be directly solved with generative PLMs.

这种模式的优势：

- First, in this formulation, a uniﬁed text-totext/seq2seq framework can be used to solve different NLP tasks via encoder-decoder architectures, thus facilitating multi-task learning and transfer learning across tasks of different natures
- Second, the direct generation of labels in output sequences allows the PLMs to exploit the semantics of the labels to improve the performance and data efﬁciency, a beneﬁt that cannot be achieved in discriminative models
- Finally, when adapting to structure prediction problems, PLM-based models can naturally capture the inter-dependencies between prediction steps/tasks in the modeling process to further improve the performance

关键：**the formation of the output sequence** y for an input x is critical for the performance of the PLM-based methods.  【分为以下6类】

##### 4.1 Generating Label-Augmented Texts：the output sequence y copies the input text x and augments it with additional markers that can be decoded into desired label annotations for x for a given NLP task.

适用场景：structure prediction tasks

##### 4.2 Generating Word Indices：directly generate indices of the words of interest in the input text.

##### 4.3 Generating Answers：ﬁne-tune PLMs to generate answers for the QA problems of interest.

适用场景：QA task

##### 4.4 Filling templates：A template deﬁnes the appropriate relationship and order for the spans and labels for generation, forming the output sequence y.

适用场景：extraction tasks

##### 4.5 Generating Structure-Linearized Texts：linearizing the output structure to serve as the output sequence y

适用场景：Structure prediction tasks

##### 4.6 Ranking Input-Output Pairs：rank the candidates in relation to the input query

适用场景：multiple choice-sytle QA, information retrieval, entity retrieval



### 5. Data Generation via PLM

【分为以下两种方法】

##### 5.1 Augmenting NLP Models with Automatically Generated Data：data generated by PLMs can be combined with original training data to improve NLP models where training data is too sparse.

案例：Information Extraction，Question Answering，Sentiment Analysis，Fact Veriﬁcation，Document Classiﬁcation

##### 5.2 Generating Auxiliary Data to Improve Different Aspects of NLP Models：plays a role in machine learning explainability

案例：Explaining Models’ Decisions；Knowledge Extraction；Question Generation；Inference Rule Generation



### 6. Discussion

- Mix of paradigms or PLMs：几种范式的结合；几种预训练模型的结合；**目前预训练模型的预训练过程由fine-tune所主导，可以考虑基于其他几种范式的预训练任务**
- How much unlabeled data is needed：预训练需要的数据量
- How much labeled data is still needed：使用各种范式用于下游任务时需要的数据量
- Can we reduce the amount and cost of computation：计算量大
- Do PLMs excel at semantic understanding or memorization：**即使对于训练数据的内容也记忆得不好**
- Is explicit linguistic information needed：预训练或微调阶段是否还需要**显式插入句法或者语义信息**（目前的几个工作发现如果显式插入的话对于结果还是会有提升的）
- Can we integrate implicit semantic information using QA：使用QA来隐式地引入语义信息
- Do PLMs need meaningful prompts：the results of continuous prompts also show that PLMs do not need meaningful instructions for improving few-shot performance.
- Theoretical and empirical analysis：
- 