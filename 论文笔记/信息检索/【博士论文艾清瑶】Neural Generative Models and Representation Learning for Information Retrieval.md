## 摘要

基于统计概率框架的$\color{red}{生成式检索模型}$有很好的理论基础和经验性能，但是它在IR领域的应用受限于它在<u>建模低维表达时的高复杂度和低扩展性。</u>   ????什么是生成式检索模型？？？

随着深度学习技术的发展，它提供了新的机会来发展**表达学习和生成式模型 for IR**。 不像统计模型，神经模型有更大的灵活性来建模信息和数据之间的关系。



本文主要研究$\color{red}{如何为信息检索任务开发基于神经网络的生成式模型和表达学习框架}$：

- 理论分析：对现有的neural embedding model如何应用到ad-hoc retrieval任务进行了理论分析，并对任务适应性方面存在的问题提出了改进方法。 （`Analysis of the Paragraph Vector Model for Information Retrieval`）​
- 设计实践：设计了一个为个性化产品搜索任务的【embedding-based neural generative model】，同时建模信息的生成和基于分布式信息表示的相关性。
- 通用架构：把本文提出的【neural generative model】泛化到复杂的信息检索应用场景——具有异质信息源的信息检索任务。



## 引言

1. **信息检索任务**的两个基本挑战：
   - 如何表达信息
   - 如何更好地匹配materials和needs来检索信息

2. **词袋表示**可以保持关键信息，并维持简单的数据结构，因此在信息检索系统中非常有效且高效。基于词袋表示的检索模型分两类：
   - 判别式（VSM、L2R）：提取文档和查询的匹配特征，分析他们的相关性。
   - 生成式：建模相关信息的生成过程。

3. 基于词袋表示的生成式模型的缺点：词汇鸿沟。

   为了解决该问题，最直接的方式是使用**潜在语义表达**（topic model），用低维稠密向量代替高维稀疏的正交向量。。

   基于topic model的潜在语义表示计算代价高，泛化能力差（在现有的统计框架下，设计一个可以联合建模来自不同源的异质信息的生成式模型还是困难的）。

   因此，需要探索新的生成式模型和表示学习的范式。

4. Neural Embedding Framework

   > 基于DNN的语言模型：
   >
   > <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191118185622468.png" alt="image-20191118185622468" style="zoom:50%;" />

5. Neural Generative Model for IR

   本文的目标是，结合 generative model 和 neural embedding model 的优点用于IR任务，构建一个**一般化的表达学习框架**，它可以捕捉结构数据与非结构数据之间的复杂关系。

   具体做法是，<u>通过直接建模搜索中的相关信息的生成，来构建基于neural embedding model的分布式表示</u>。

   本文工作的最终成果是，一个neural generative model，它具有灵活的结构，可以被扩展以应用于大部分信息检索任务（e.g. 推荐）。

6. 存在的挑战包括：

   - 如何把 neural embedding model 适应到检索的目的上？

     neural embedding model需要有明确的目标函数，但IR的目标函数（相关性）不清晰。在NLP任务上学习到的word embeeding直接应用到IR任务时，效果不好。

     【本文对PV-DBOW在IR任务上，因适应性引起的问题进行理论分析并提出解决方法。对未来将 neural embedding model 用于IR任务提供了好的指导。】

   - 如何在生成式检索框架下构建神经模型，即如何构建neural generative model？

     在一些复杂的信息检索应用场景（e.g. 产品推荐）下，任务的目标通常伴随着query和document字面之外的一些因素。

     【本文结合neural embedding model 和 generative retrieval framework，直接在信息检索问题上进行优化，同时建模信息的生成和基于分布式信息表示的相关性。】

   - 如何把 neural generative model 泛化到能够捕捉异质数据的复杂关系？

     目前多域/多模态的信息检索还是依赖于启发式规则，可扩展性差。

     【本文用一个一般化的表示学习框架来扩展neural generative model，使其能够捕捉异质信息和多关系数据来进行信息检索。】



## 背景及相关工作

1. 统计生成模型

   - **传统概率模型**

     <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191118222152642.png" alt="image-20191118222152642" style="zoom:40%;" />

     典型方法：BIR（binary independence retrieval model）

     ​                   BM25：<img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191118222336136.png" alt="image-20191118222336136" style="zoom:33%;" />

   - **语言模型方法**

     <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191118222400696.png" alt="image-20191118222400696" style="zoom:70%;" />

     典型方法：Query Likelihood model (QL) with Dirichlet smoothing<img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191118222507138.png" alt="image-20191118222507138" style="zoom:33%;" />

   传统的概率模型和语言模型都是基于词的统计，没有显式建模query和document之间的语义关系。

   - **潜在语义表达**

     典型方法：LSI、PLSI





#### Adapting neural embedding model for ad-hoc retrieval





#### Embedding-based neural generative model for personalized product search





#### Extendable and explainable neural representation learning framework





#### 总结























