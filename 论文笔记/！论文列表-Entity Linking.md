#### Entity Linking

- [ ] 【rerank】Learning entity representation for entity disambiguation.（ACL2013，）

  > use a dual encoder setup to train a re-ranker, but start with unsupervised training to build representations of contexts and entities using Denoising Autoencoders. They use an alias table for candidate generation, and then train a ranking model using mention speciﬁc batching to obtain hard in-batch negatives.

- [ ] Entity disambiguation with web links.（ACL2015）

- [ ] 【rerank】Modeling mention, context and entity with neural networks for entity disambiguation.（2015）

  > use a dual encoder to score entries from an alias table，Their mention encoder is a considerably more complex combination of mention and context，Their alias table method not only maps mentions to entities, but also uses additional ﬁlters to reduce the set of candidate entities based on words in the mention’s context. They train their representations for each true mentionentity pair against a single random negative entity for the mention.

- [ ] 【fullrank，非独立编码】Joint learning of the embedding of words and entities for named entity disambiguation.（2016，）

  > use an alias table derived from the December 2014 Wikipedia dump, restricted to the ﬁfty most popular entities per mention. They tune their model on the TACKBP 2010 training set. Architecturally, they include features that capture the alias table priors and string similarities, both of which are not feasible in a dual encoder conﬁguration that precludes direct comparison between mention- and entity-side features.

- [ ] 【fullrank，非独立编码】Mention and entity description co-attention for entity disambiguation（AAAI2018）

  > deﬁne a complex model that uses both entity type information and attention between the mention string and the entity description. To augment the small 1500 example training data in TACKBP, they also collected 55k mentions found in Wikipedia that were active in TACKBP 2010 to train this model.

- [ ] 【rerank】Learning text representations for 500k classiﬁcation tasks on named entity disambiguation（CoNLL2018）

  > train 523k mention-speciﬁc deep classiﬁers—effectively treating entity linking as a special form of word sense disambiguation. They do this by pre-training a single LSTM that predicts among 248k mentions, and then the parameters of this model are used to warm start each of the 523k mention-speciﬁc models. In doing so, they learn an effective context encoding, and can then ﬁne-tune each mention model to discriminate among the small set of popular candidate entities for the mention (their alias table uses a cutoff of the thirty most popular entities for each mention

- [x] 【DEER】【fullrank，独立编码】**Learning dense representations for entity retrieval.**（CoNLL2019，full ranking设置）

- [ ] Zero-shot Entity Linking with Dense Entity Retrieval（2019）

- [ ]  （实体链接）Improving Entity Linking through Semantic Reinforced Entity Embeddings（ACL2020，未公开论文）

- [x] Neural Entity Linking: A Survey of Models based on Deep Learning（2020）

  >综述，2015年之后的neural model
  >
  >- 实体链接的基本模块：1）候选生成；2）实体排序；3）实体表达；4）NIL预测
  >- 新的研究方向：1）联合训练实体识别和实体链接；2）使用全局上下文；3）领域独立；4）跨语言

- [ ] Named entity extraction for knowledge graphs: A literature overview.（2020，综述，review both entity recognition and general entity disambiguation/linking methods published between the years 2014-2019）

- [ ] From Zero to Hero: Human-In-The-Loop Entity Linking in Low Resource Domains（ACL2020）

- [ ] Boosting Entity Linking Performance by Leveraging Unlabeled Documents（ACL2019）

- [ ] Zero-Shot Entity Linking by Reading Entity Descriptions（ACL2019）

- [ ] Distant Learning for Entity Linking with Automatic Noise Detection（ACL2019）

- [ ] Revisiting Joint Modeling of Cross-document Entity and Event Coreference Resolution（ACL2019）

- [ ] Low-resource Deep Entity Resolution with Transfer and Active Learning（ACL2019）

- [ ] Learning Dynamic Context Augmentation for Global Entity Linking（EMNLP2019）

- [ ] Fine-Grained Evaluation for Entity Linking（EMNLP2019）

- [ ] ！！ENT Rank: Retrieving Entities for Topical Information Needs through Entity-Neighbor-Text Relations（SIGIR2019）

- [ ] Implicit Entity Recognition, Classification and Linking in Tweets（SIGIR2019）

- [ ] 

