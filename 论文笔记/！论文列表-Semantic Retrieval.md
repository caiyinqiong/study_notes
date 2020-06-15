#### Ad-hoc Retrieval

- [ ] Semantic hashing.（2009，稀疏表示）
- [x] 【DESM】A Dual Embedding Space Model for Document Ranking（基于embedding的向量表示的相似度与BM25的相似度进行加权）
- [ ] Improving document ranking with dual word embeddings.（WWW，2016）
- [x] 【SNRM】From Neural Re-Ranking to Neural Ranking: Learning a Sparse Representation for Inverted Indexing（CIKM2018）
- [x] 【Poly-encoders】Poly-encoders: Architectures and Pre-training Strategies for Fast and Accurate Multi-sentence Scoring（ICLR2020，基于BERT，对question产生多个全局特征表示）
- [x] Incorporating query term independence assumption for efficient retrieval and ranking using deep neural networks（2019，引入query term 独立性假设）
- [x] 【Doc2Query】Document Expansion by ery Prediction.（2019，使用seq2seq生成多个query，扩充原始文档）
- [x] 【DocTTTTTQuery】From doc2query to docTTTTTquery.（2019，doc2query的进一步工作，使用T5生成query）
- [x] 【ColBERT】ColBERT: Eicient and Eective Passage Search via Contextualized Late Interaction over BERT（SIGIR2020，使用两个BERT得到query和passage的表示，推迟词级别的交互到最后，使用max-sum的方法导致可以过滤掉大部分文档，只留小部分进行精确计算）
- [ ] ！！【DeepCT】Context-Aware Document Term Weighting for Ad-Hoc Search（WWW2020）
- [x] 【PreTTR】Efficient Document Re-Ranking for Transformers by Precomputing Term Representations（SIGIR2020，和DeFormer类似的结构，再多一个压缩层和解压层减少存储空间）

#### QA

- [x] 【PIQA】Phrase indexed question answering: A new challenge for scalable document comprehension.（EMNLP2018，以phrase为单位编码，基于BiLSTM学习向量表示）
- [x] 【DenSPI】Real-Time Open-Domain Question Answering with Dense-Sparse Phrase Index（ACL2019，以phrase为单位编码，基于BERT学习dense vector + 基于2-gram的TFIDF的sparse vector）
- [x] 【ORQA】Latent Retrieval for Weakly Supervised Open Domain Question Answering（ACL2019，以passage为单位编码，用ICT任务预训练retriever）
- [x] 【MUPPET】Multi-Hop Paragraph Retrieval for Open-Domain Question Answering（ACL2919，以passage为单位进行编码，但保留每个sentence的向量表示。基于BiGRU学习向量表示。仍然需要BM25缩小检索范围）
- [x] 【SPARC】Contextualized Sparse Representations for Real-Time Open-Domain Question Answering（ACL2020，在DenSPI的基础上增加可学习的稀疏表示）
- [x] 【DC-BERT】DC-BERT: Decoupling Question and Document for Efficient Contextual Encoding（2020，用于openQA的rerank阶段。两个独立的BERT分别编码retrieved document和question，上层再用Transformer进行交互。**另外提出了两个针对语义检索的指标**）
- [x] 【DeFormer】DeFormer: Decomposing Pre-trained Transformers for Faster Question Answering（ACL2020，解耦低层的BERT，分别学习question和document的表示，并使用两个辅助loss）
- [x] REALM: Retrieval-Augmented Language Model Pre-Training（2020，一种检索增强的预训练方法）
- [x] Dense Passage Retrieval for Open-Domain Question Answering（2020，使用BERT分别建模query和passage，CLS处的输出作为表达，用于openQA的检索模块）

#### 电商、推荐

- [x] From Semantic Retrieval to Pairwise Ranking: Applying Deep Learning in E-commerce Search（SIGIR2019，京东语义检索，分别提出了两个双塔模型用于semantic ranking和reranking）
- [x] 【DPSR】Towards Personalized and Semantic Retrieval: An End-to-End Solution for E-commerce Search via Embedding Learning（SIGIR2020，京东的语义检索模型）
- [ ] Learning a Joint Search and Recommendation Model from User-Item Interactions（WSDM2020）
- [ ] Mixed Negative Sampling for Learning Two-tower Neural Networks in Recommendations.（WWW2020）
- [ ] Sampling-bias-corrected neural modeling for large corpus item recommendations.（2019）
- [ ] Deep Neural Networks for YouTube Recommendations.（2016）
- [ ] Multi-Interest Network with Dynamic Routing for Recommendation at Tmall.（CIKM2019）
- [ ] Learning Tree-Based Deep Model for Recommender Systems.（KDD2018）

#### 其他

- [ ] ！！Efficient Interaction-based Neural Ranking with Locality Sensitive Hashing（WWW2019）
- [ ] Multi-Stage Document Ranking with BERT（2019）

- [ ] Contextualized word representations for reading comprehension.（NAACL2018，编码文档时最小化question的影响）
- [ ] DeQA: On-Device Question Answering.（2019）
- [ ] Neural ranking models with multiple document ﬁelds.（2018，WSDM，）
- [ ] Understanding the representational power of neural retrieval models using NLP tasks.（2018SIGIR）
- [ ] Spine: Sparse interpretable neural embeddings.（AAAI2018）
- [ ] Semantic Matching in Search（2014）
- [ ] Semantic Hashing.（2009）
- [ ] An Introduction to Neural Information Retrieval.（2018，NeuIR的综述）
- [ ] ！！Efficient natural language response suggestion for smart reply.（2017）
- [ ] Deep sentence embedding using long short-term memory networks: Analysis and application to information retrieval.（2016）
- [ ] Search personalization with embeddings.（ECIR2017）
- [ ] Learning dense representations for entity retrieval.（CoNLL，2019）
- [ ] Real-time inference in multi-sentence tasks with deep pretrained transformers.（2019）
- [ ] Simple Applications of BERT for Ad Hoc Document Retrieval.（2019）
- [ ] CEDR:Contextualized Embeddings for Document Ranking.（SIGIR2019）
- [ ] Let’s measure run time! Extending the IR replicability infrastructure to include performance aspects.（SIGIR2019）
- [ ] Efficient Query Processing for Scalable Web Search.（2018）

引用SNRM的论文：

- [ ] Learning Term Discrimination（SIGIR2020）
- [ ] SPECTER: Document-level Representation Learning using Citation-informed Transformers（ACL2020）
- [ ] Report on the First HIPstIR Workshop on the Future of Information Retrieval（2019）
- [ ] Efficiency Implications of Term Weighting for Passage Retrieval（SIGIR2020）



#### ANN等快速搜索算法

- [ ] ！！Off the Beaten Path: Let’s Replace Term-Based Retrieval with k-NN Search（2016CIKM）
- [ ] Ann-benchmarks: A benchmarking tool for approximate nearest neighbor algorithms.（2017）
- [ ] Fast Item Ranking under Neural Network based Measures（WSDM2020，基于图的近似近邻检索）
- [ ] Billion-scale similarity search with GPUs.（2017，faiss，hps://github.com/facebookresearch/faiss）



#### 数据集

- [ ] 【TREC CAR】Laura Dietz, Manisha Verma, Filip Radlinski, and Nick Craswell. 2017. TREC Complex Answer Retrieval Overview.. In TREC.
- [ ] 



##### 模型蒸馏

DistilBERT, a distilled version of BERT: smaller, faster, cheaper and lighter.（NIPS2019）

TinyBERT: Distilling BERT for Natural Language Understanding.（2019）

Distilling Task-Specific Knowledge from BERT into Simple Neural Networks.（2019）

##### Quantization 方法

Efficient Weights Quantization of Convolutional Neural Networks Using Kernel Density Estimation based Non-uniform Quantizer.（2019）

Deep Compression: Compressing Deep Neural Network with Pruning, Trained Quantization and Huffman Coding.（ICLR2015）