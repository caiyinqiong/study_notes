## Sparse retrieval

#### re-weight

1. ~~Context-Aware Sentence/Passage Term Importance Estimation for First Stage Retrieval（Dai et al., 2019, arXiv, DeepCT）~~
2. ~~Context-Aware Term Weighting For First-Stage Passage Retrieval（Dai et al., 2020, SIGIR, DeepCT）~~
4. ~~Context-Aware Document Term Weighting for Ad-Hoc Search（Dai et al., 2020, WWW, HDCT）~~
4. A Few Brief Notes on DeepImpact, COIL, and a Conceptual Framework for Information Retrieval Techniques（Lin et al., 2021, arXiv, uniCOIL）

#### expansion

3. ~~From doc2query to docTTTTTquery（Nogueira et al., 2019, arXiv, DocTTTTTQuery）~~
4. ~~A Unified Pretraining Framework for Passage Ranking and Expansion（Yan et al., 2021, AAAI, UED）~~
3. ~~Generation-augmented Retrieval for Open-domain Question Answering（Mao et al., 2020, ACL, GAR, query expansion）~~

#### re-weight + expansion

1. ~~SparTerm: Learning Term-based Sparse Representation for Fast Text Retrieval（Bai et al., 2020, arXiv, SparTerm）~~
2. ~~SPLADE: Sparse Lexical and Expansion Model for First Stage Ranking（Formal et al., 2021, SIGIR, SPLADE）~~
3. ~~Learning Passage Impacts for Inverted Indexes（Mallia et al., 2021, SIGIR, DeepImapct）~~

#### sparse representation learning

1. ~~UHD-BERT: Bucketed Ultra-High Dimensional Sparse Representations for Full Ranking（Jang et al., 2021, arXiv, UHD-BERT）~~
2. ~~Efﬁcient Passage Retrieval with Hashing for Open-domain Question Answering（Yamada et al., 2021, ACL, BPR）~~



## Dense retrieval

#### Basic（最初被用到不同的IR任务）

单表达：

1. ~~Dense Passage Retrieval for Open-Domain Question Answering（Karpukhin et al., 2020, EMNLP, DPR）**ODQA**~~
2. Learning Dense Representations of Phrases at Scale（Lee et al., 2021, ACL, DensePhrases）**ODQA**
3. ~~RepBERT: Contextualized Text Embeddings for First-Stage Retrieval（Zhan et al., 2020, arXiv, RepBERT）~~**ad-hoc retrieval**
4. CoRT: Complementary Rankings from Transformers（Wrzalik et al., 2019, NAACL, CoRT）
5. ~~Few-Shot Conversational Dense Retrieval（Yu et al., 2021, SIGIR）**dialogue**~~

多表达：

1. ~~ColBERT: Efficient and Effective Passage Search via Contextualized Late Interaction over BERT（Khattab et al., 2020, SIGIR, ColBERT）**ad-hoc retrieval**~~
2. ~~COIL: Revisit Exact Lexical Match in Information Retrieval with Contextualized Inverted List（Gao et al., 2021, NACL, COIL）**ad-hoc retrieval**~~
3. Multi-Hop Paragraph Retrieval for Open-Domain Question Answering（Feldman et al., 2019, ACL, MUPPET） **ODQA**
4. ~~Sparse, Dense, and Attentional Representations for Text Retrieval（Luan et al., 2020, TACL, ME-BERT）**ad-hoc retrieval**~~
5. ~~Improving Document Representations by Generating Pseudo Query Embeddings for Dense Retrieval（Tang et al., 2021, ACL）  **多表达**~~

蒸馏：

1. ~~Distilling Knowledge for Fast Retrieval-based Chat-bots（Tahami et al., 2020, SIGIR）**dialogue**~~
2. ~~Distilling Dense Representations for Ranking using Tightly-Coupled Teachers（Lin et al., 2020, arXiv, TCT-ColBERT）~~**ad-hoc retrieval**
3. ~~Improving Bi-encoder Document Ranking Models with Two Rankers and Multi-teacher Distillation（Choi et al., 2021, SIGIR, TRMD）**ad-hoc retrieval**~~
4. ~~Improving Efficient Neural Ranking Models with Cross-Architecture Knowledge Distillation（Hofstätter et al., 2021, arXiv, Margin-MSE loss）**ad-hoc retrieval**~~
5. Distilling Knowledge from Reader to Retriever for Question Answering（Izacard et al., 2020, arXiv）**ODQA**

#### hard negative mining

1. ~~Approximate Nearest Neighbor Negative Contrastive Learning for Dense Text Retrieval~~（Xiong et al., 2020, arXiv, ANCE）
2. ~~Learning To Retrieve: How to Train a Dense Retrieval Model Effectively and Efficiently~~（Zhan et al., 2020, arXiv, LTRe）
3. ~~Optimizing Dense Retrieval Model Training with Hard Negatives（Zhan et al., 2021, SIGIR, STAR/ADORE）~~
4. An Optimized Training Approach to Dense Passage Retrieval for Open-Domain Question Answering（Qu et al., 2021, ACL, RocketQA）
5. PAIR: Leveraging Passage-Centric Similarity Relation for Improving Dense Passage Retrieval（Ren et al., 2021, ACL, PAIR）
6. ~~Efficiently Teaching an Effective Dense Retriever with Balanced Topic Aware Sampling（Hofstätter et al., 2021, SIGIR, TAS-Balanced）~~

#### Advanced topic

1. Relevance-guided Supervision for OpenQA with ColBERT（Khattab et al., 2021, TACL, ColBERT-QA）
2. ~~Retrieval-augmented generation for knowledge-intensive NLP tasks（Lewis et al., 2020, NLPS, RAG）**end-to-end learning**~~
3. ~~End-to-End Training of Multi-Document Reader and Retriever for Open-Domain Question Answering（Sachan et al., 2021, arXiv, EMDR^2） **end-to-end learning**~~
4. ~~Joint Learning of Deep Retrieval Model and Product Quantization based Embedding Index（Zhang et al., 2021, SIGIR, Poeem） **joint learning with index**~~
5. ~~Jointly Optimizing Query Encoder and Product Quantization to Improve Retrieval Performance（Zhan et al., 2021, CIKM, JPQ）~~**joint learning with index**
6. ~~An Optimized Training Approach to Dense Passage Retrieval for Open-Domain Question Answering（Qu et al., 2021, ACL, RocketQA）~~**bias**
7. ~~PAIR: Leveraging Passage-Centric Similarity Relation for Improving Dense Passage Retrieval（Ren et al., 2021, ACL, PAIR）~~
8. ~~Learning Robust Dense Retrieval Models from Incomplete Relevance Labels~~（Prakash et al., 2021, SIGIR, RANCE） **bias**



## Hybrid Retrieval

1. ~~Real-Time Open-Domain Question Answering with Dense-Sparse Phrase Index （Seo et al., 2019, ACL, DenSPI）~~

2. ~~Contextualized Sparse Representations for Real-Time Open-Domain Question Answering（Lee et al., 2020, ACL, SPARC）~~

3. Expansion via Prediction of Importance with Contextualization（MacAvaney et al., 2020, SIGIR, EPIC）

4. ~~Sparse, Dense, and Attentional Representations for Text Retrieval（Luan et al., 2020, TACL, ME-Hybrid）  加权求和~~

5. ~~A Few Brief Notes on DeepImpact, COIL, and a Conceptual Framework for Information Retrieval Techniques（Lin et al., 2021, arXiv, uniCOIL）~~ 加权求和

6. CoRT: Complementary Rankings from Transformers（Wrzalik et al., 2019, NAACL, CoRT） 合并两个list

7. ~~Complement Lexical Retrieval Model with Semantic Residual Embeddings（Gao et al., 2020, ECIR, CLEAR）  RM3~~

8. ~~Leveraging Semantic and Lexical Matching to Improve the Recall of Document Retrieval Systems: A Hybrid Approach（Kuzi et al., 2020, arXiv, Hybrid） 残差学习~~

9. Contextualized Offline Relevance Weighting for Efficient and Effective Neural Retrieval（Chen et al., 2021, SIGIR）

    

