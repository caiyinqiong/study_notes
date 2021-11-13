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

#### Tailored pretraining

pre-training：

1. ~~Latent Retrieval for Weakly Supervised Open Domain Question Answering（Lee et al., 2019, ACL, ORQA）~~
2. Retrieval-Augmented Language Model Pre-Training（Guu et al., 2020, ICML, REALM）
3. ~~Pre-training Tasks for Embedding-based Large-scale Retrieval（Chang et al., 2020, ICLR, BFS+WLP+MLM）~~
4. ~~Embedding-based Zero-shot Retrieval through Query Generation~~（Liang et al., 2020, arXiv, query generation）
5. ~~Zero-shot Neural Passage Retrieval via Domain-targeted Synthetic Question Generation~~（Ma et al., 2020, arXiv, query generation）    **considers application to domain transfer**
6. ~~Towards Robust Neural Retrieval Models with Synthetic Pre-Training~~（Reddy et al., 2021, arXiv, query generation）
7. Domain-matched Pre-training Tasks for Dense Retrieval（2021） DPR-PAQ
8. ~~【coCondenser】Unsupervised Corpus Aware Language Model Pre-training for Dense Passage Retrieval（2021）~~ 
9. ~~【Condenser】Is Your Language Model Ready for Dense Representation Fine-tuning?~~ 

#### Improved Fine-tune

蒸馏：

1. ~~Distilling Knowledge for Fast Retrieval-based Chat-bots（Tahami et al., 2020, SIGIR）**dialogue**~~
2. ~~Distilling Dense Representations for Ranking using Tightly-Coupled Teachers（Lin et al., 2020, arXiv, TCT-ColBERT）~~**ad-hoc retrieval**
3. ~~Improving Bi-encoder Document Ranking Models with Two Rankers and Multi-teacher Distillation（Choi et al., 2021, SIGIR, TRMD）**ad-hoc retrieval**~~
4. ~~Improving Efficient Neural Ranking Models with Cross-Architecture Knowledge Distillation（Hofstätter et al., 2021, arXiv, Margin-MSE loss）**ad-hoc retrieval**~~
5. Distilling Knowledge from Reader to Retriever for Question Answering（Izacard et al., 2020, arXiv）**ODQA**

hard negative mining

1. ~~Approximate Nearest Neighbor Negative Contrastive Learning for Dense Text Retrieval~~（Xiong et al., 2020, arXiv, ANCE）
2. ~~Learning To Retrieve: How to Train a Dense Retrieval Model Effectively and Efficiently~~（Zhan et al., 2020, arXiv, LTRe）
3. ~~Optimizing Dense Retrieval Model Training with Hard Negatives（Zhan et al., 2021, SIGIR, STAR/ADORE）~~
4. An Optimized Training Approach to Dense Passage Retrieval for Open-Domain Question Answering（Qu et al., 2021, ACL, RocketQA）
5. PAIR: Leveraging Passage-Centric Similarity Relation for Improving Dense Passage Retrieval（Ren et al., 2021, ACL, PAIR）
6. ~~Efficiently Teaching an Effective Dense Retriever with Balanced Topic Aware Sampling（Hofstätter et al., 2021, SIGIR, TAS-Balanced）~~

#### Advanced topic

1. Relevance-guided Supervision for OpenQA with ColBERT（Khattab et al., 2021, TACL, ColBERT-QA）
2. Latent Retrieval for Weakly Supervised Open Domain Question Answering（Lee et al., 2019, ACL, ORQA）
3. Retrieval-Augmented Language Model Pre-Training（Guu et al., 2020, ICML, REALM）
4. ~~Retrieval-augmented generation for knowledge-intensive NLP tasks（Lewis et al., 2020, NLPS, RAG）**end-to-end learning**~~
5. ~~End-to-End Training of Multi-Document Reader and Retriever for Open-Domain Question Answering（Sachan et al., 2021, arXiv, EMDR^2） **end-to-end learning**~~
6. ~~Joint Learning of Deep Retrieval Model and Product Quantization based Embedding Index（Zhang et al., 2021, SIGIR, Poeem） **joint learning with index**~~
7. ~~Jointly Optimizing Query Encoder and Product Quantization to Improve Retrieval Performance（Zhan et al., 2021, CIKM, JPQ）~~**joint learning with index**
8. ~~An Optimized Training Approach to Dense Passage Retrieval for Open-Domain Question Answering（Qu et al., 2021, ACL, RocketQA）~~**bias**
9. ~~PAIR: Leveraging Passage-Centric Similarity Relation for Improving Dense Passage Retrieval（Ren et al., 2021, ACL, PAIR）~~
10. ~~Learning Robust Dense Retrieval Models from Incomplete Relevance Labels~~（Prakash et al., 2021, SIGIR, RANCE） **bias**



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

    





----------------

# Pre-training for IR





# End-to-end Learning







# Multi-modal Learning

















4. Designing Effective Architectures

   4.1 Uniﬁed Sequence Modeling

   - Combining Autoregressive and Autoencoding Modeling.
   - Applying Generalized Encoder-Decoder.

   4.2 Cognitive-Inspired Architectures

   - Maintainable Working Memory
   - Sustainable Long-Term Memory.

   4.3 More Variants of Existing PTMs

5. **Utilizing Multi-Source Data**

   **5.1 Multilingual Pre-Training**

   **5.2 Multimodal Pre-Training**

   **5.3 Knowledge-Enhanced Pre-Training**

6. Improving Computational Efﬁciency
7. Interpretation and Theoretical Analysis

8. Future Directions

   8.1 **Architectures and Pre-Training Methods**

   - **New Architectures.**
   - **New Pre-Training Tasks.**
   - **Beyond Fine-Tuning.**
   - Reliability.

   8.2 Multilingual and Multimodal Pre-Training

   - **More Modalities.**
   - More Insightful Interpretation.
   - More Downstream Applications.
   - Transfer Learning.

   8.3 Computational Efﬁciency

9. 





多模态：

single-stream，double-strem；（使用不同的架构所适用的下游任务不同？？？）

挑战：1）还在起步阶段？？？？预训练任务？？



多语言：

不同语言的输入，到一个模型，都能得到在统一的一个空间中的表示？

可以做跨语言任务？？例如跨语言检索？？



