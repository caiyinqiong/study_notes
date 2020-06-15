- [x] 陈丹琦的毕设

- [ ] Neural speed reading via Skim-RNN.

- [ ] Learning to skim text.

- [ ] Learning to compose neural networks for question answering.

- [ ] Evaluation metrics for machine reading comprehension: Prerequisite skills and readability.

- [ ] DrQA*:Weaver: Deep co-encoding of questions and documents for machine reading.

  

- [ ] Neural approaches to conversational AI.（对话系统综述）

- [ ] Search-based neural structured learning for sequential question answering. （把复杂问题分解成一系列简单的问题）

- [ ] The web as a knowledge-base for answering complex questions. （把复杂问题分解成一系列简单的问题）

- [ ] Complex sequential question answering: Towards learning to converse over linked question answer pairs with a knowledge graph.（基于知识图谱的问答）



- [ ] MCTest:A Challenge Dataset for the Open-Domain Machine Comprehension of Text.（数据集）
- [ ] Hierarchical question answering for long documents（2016，show联合优化检索和阅读模块的优越性）
- [ ] Improving information extraction by acquiring external evidence with reinforcement learning.（2016，找到合适的passage来抽取答案）
- [ ] Attentive memory networks: E￿cient machine reading for conversational search.（2017，先选择最可能的段落，再进行RC）
- [ ] Globally normalized reader.（2017EMNLP，把抽取式QA视为搜索问题，迭代地搜索相关句子、答案起止位置）
- [ ] Learning recurrent span representations for extractive question answering.（2016，学习phrase的向量表示）



多跳推理：

Gated-attention Readers for Text Comprehension（2017ACL）

Reasonet: Learning to Stop Reading in Machine Comprehension. （2017，KDD）

Towards Human-level Machine Reading Comprehension: Reasoning and Inference with Multiple Strategies.（2017）

Neural models for reasoning over multiple mentions using coreference.（2018，NAACL）

Coarse-grain ﬁne-grain coattention network for multi-evidence question answering.（2019，ICLR）





# 2017及以前

- [x] （Chen，2017）Reading wikipedia to answer open-domain questions.（DrQA）

- [x] Question Answering from Unstructured Text by Retrieval and Comprehension（2017，使用pipeline模式，引入ranker模块选择topM相关文档，拼接后作为reader的输入，在reader阶段使用pointer和vocabulary的混合模型。其中ranker模块是针对WikiMovie数据集提出了3种方法，neural的方法是用LSTM+attention）
- [x] （Clark，2017）Simple and Effective Multi-Paragraph Reading Comprehension.（针对有多个段落进行RC的场景）
- [x] （Choi，2017）Coarse-to-ﬁne question answering for long documents.（2017ACL，先使用词袋表示/CNN选择topk个句子或者根据句子得分生成文档摘要，再基于文档摘要用RNN进行答案生成）

# AAAI2018

- [x] （Wang，2018）Rˆ3: Reinforced reader ranker for open-domain question answering. （使用强化学习联合训练ranker和reader，ranker计算每个文档的相关性，根据相关性采样一个passage输入到reader，reader生成答案时仅考虑一个文档）

# ACL2018

- [x] （Lin，2018）Denoising distantly supervised open-domain question answering（利用retrieval得到的所有段落。通过reranker计算每个段落含有答案的概率，将该概率乘到答案的概率上，联合训练text reranker + reader）
- [ ] Training a Ranking Function for Open-Domain Question Answering.（NAACL）
- [ ] Contextualized word representations for reading comprehension.（NAACL，编码文档时最小化question的影响）
- [ ] The web as a knowledge-base for answering complex questions.（NAACL，多跳推理）
- [x] Efﬁcient and robust question answering from minimal context over documents.（提出了sentence-level的reranker，先动态选择出所需的句子，再用现有的QA模型生成答案）
- [x] Joint Training of Candidate Extraction and Answer Selection for Reading Comprehension（基于强化学习联合训练reader和answer reranker，主要是提出了一个融合所有候选答案信息的answer reranker模块）
- [ ] Multi-passage machine reading comprehension with cross-passage answer veriﬁcation.

# ICLR2018

- [x] （Wang，2017b）Evidence Aggregation for Answer Re-Ranking in Open-Domain Question Answering.（对答案进行reranker）

# EMNLP2018

- [x] Phrase indexed question answering: A new challenge for scalable document comprehension.（学习独立的document phrase表示和question表示）
- [x] （Lee，2018）Ranking paragraphs for improving answer recall in open-domain question answering.（对top ranked documents进行段落排序，选择topM个段落进行RC。以便经过retrieval之后可以接触更多的段落，提高answer的召回率）
- [x] （Kratzwald and Feuerriegel，2018）Adaptive Document Retrieval for Deep Question Answering.（探究了retriever和reader之间的关系，对于不同的question，取不同top数目的document用于RC）

# CIKM2018

- [x] Retrieve-and-read: Multi-task learning of information retrieval and reading comprehension.（结合多任务学习进行document rerank和answer抽取，要求训练数据中含有有标签的相关段落，不适合直接用于开放域问答）



# 2019、2020

- [x] End-to-end open-domain question answering with bertserini（2020，检索部分用BM25，索引的单位有article、passage、sentence三种设置；reader部分用bert。最后的score是两部分分值的加权。）
- [ ] Passage Re-ranking with BERT
- [ ] Investigating the Successes and Failures of BERT for Passage Re-Ranking,
- [ ] TANDA: Transfer and Adapt Pre-Trained Transformer Models for Answer Sentence Selection
- [ ] SpanBERT: Improving Pre-training by Representing and Predicting Spans
- [x] DC-BERT: Decoupling Question and Document for Efficient Contextual Encoding
- [x] REALM: Retrieval-Augmented Language Model Pre-Training
- [x] Dense Passage Retrieval for Open-Domain Question Answering（2020）
- [ ] Knowledge Guided Text Retrieval and Reading for Open Domain Question Answering（2020）



# WDSM2019

- [x] Learning to Transform, Combine, and Reason in Open-Domain Question Answering（两层次Transformer架构，结合所有候选段落信息）

# SIGIR2019

- [ ] Answering Complex Questions by Joining Multi-Document Evidence with Quasi Knowledge Graphs
- [ ] Asking Clarifying Questions in Open-Domain Information-Seeking Conversations
- [x] Document Gated Reader for Open Domain Question Answering（计算每个文档与query的相似度时考虑其他文档的信息；将文档的相似度与answer span的概率相乘，在全局进行normalize；用bootstrapping数据生成机制解决远程监督带来的假正例问题）

# ACL2019

- [x] （Lee，2018）Latent Retrieval for Weakly Supervised Open Domain Question Answering（使用基于BERT的检索模型。先使用ICT任务预训练retriever，再联合微调retriever+reader）
- [x] Multi-Hop Paragraph Retrieval for Open-Domain Question Answering（通过前一轮的检索结果重构问题表示，再进行下一轮检索。然后把所有轮的检索结果输入到reader中，reader部分使用S-norm模型，对每个段落单独抽取答案）
- [x] Real-Time Open-Domain Question Answering with Dense-Sparse Phrase Index（放弃pipeline模式，直接对phrase建立索引，dense vector+sparse vector）
- [x] Retrieve, Read, Rerank: Towards End-to-End Multi-Document Reading Comprehension（端到端地训练text reranker+reader+answer reranker。对retrieval得到的top-M documents进行段落排序，使用TFIDF cos选择topK个段落拼接成原始文档的裁剪结果文档，再用这些裁剪后的文档划分以固定长度和步长segment，将每个segment和question输入到BERT中，使用BERT底层的表示进行rerank，再选择topN个segment继续通过所有的BERT层，每个segment预测多个候选答案，先用NMS算法对这些候选答案进行过滤，再根据BERT的输出表示对过滤后的候选答案进行排序。）
- [x] RankQA: Neural Question Answering with Answer Re-Ranking（以DrQA为基础，对最终得到的topk个候选答案，根据Retrieval和reader阶段的特征，对这些候选答案重排序，最终得到top1答案）

# ICLR2019

- [x] （Das，2019）Multi-step Retriever-Reader Interaction for Scalable Open-domain Question Answering（建模retriever和reader之间的关系，用reader结果反馈到段落检索）

# AAAI2019

- [ ] Has-qa: Hierarchical answer spans model for open-domain question answering.（分层次的answer span表示）
- [ ] A deep cascade model for multi-document reading comprehension.





# EMNLP2019

- [x] （外部知识，生成式QA）Incorporating External Knowledge into Machine Reading for Generative Question Answering
- [x] ！！**PullNet: Open Domain Question Answering with Iterative Retrieval on Knowledge Bases and Text**
- [x] ！！**Ranking and Sampling in Open-Domain Question Answering**
- [x] ！！Revealing the Importance of Semantic Retrieval for Machine Reading at Scale

- [x] （开放域QA）Multi-passage BERT: A Globally Normalized BERT Model for Open-domain Question Answering（提出要跨段落进行global normalization，并使用基于BERT的passage reranker）

  

  



# ICLR2020



# ACL2020



# SIGIR2020

