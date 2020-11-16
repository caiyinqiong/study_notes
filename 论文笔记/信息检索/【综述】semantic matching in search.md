> > 2014，李航

## 摘要

- 现代搜索引擎中term matching函数仍然是主要机制，因此term mismatch 给检索带来了挑战
- 本综述主要是给一个系统的详细的介绍：对 query-document matching（尤其是web search） 问题新发展的机器学习方法。
- 本综述包括：基础性问题、最新的解决方案（form aspect、phrase aspect、word sense aspect、topic aspect、structure aspect）



## 引言

- 相关性是搜索中最重要的方面，也是本文的关注点。

- 现代搜索还主要依赖term-based method，但是这种方法容易遭受mismatch的缺点。

  这种缺点主要来自于：1） term匹配度高的不一定相关，相关的不一定term匹配度高。2）由于自然语言的特性，人们通常容易用不同的term表示相同的概念。

  而且这种term-mismatch问题尤其容易出现在长尾上。

- term-based method的缺陷本质上在于缺少对query和document的分析和理解。但semantic matching通过对query和document进行分析得到他们的表示，再基于这些表示进行matching。我们期待通过semantic matching可以克服mismatch的挑战。

  - form aspect
  - phrase aspect
  - word sense aspect
  - topic aspect
  - structure aspect
  - semantic class、named entity

  本文中所使用的semantic matching的定义澄清。

  本survey的内容及边界：其他有一些解决mismatch问题的方法（query扩展、伪相关反馈、LSI），但本文更关注利用大量数据和机器学习方法来解决该问题。

- 区分web search中的 matching 和 ranking：matching score + page重要性/多样性/新鲜性等 = ranking score

- 其他任务中的semantic matching：跨语言检索、广告、对话等

- 按下面章节的分类方式概述了目前提出的一些方法

- 章节介绍、本survey的关注点、与其他综述的区别和联系、预备知识



## Semantic matching in search（问题形式化）

- 数学的视角
  - 监督学习
  - 无监督学习
- 系统的视角（针对web search）



## Matching by query reformulation（form aspect）



## matching with term dependency model（phrase aspect）

- term dependency

  使用term dependency model的假设：1）如果q和d中有连续的词匹配，则相关性更强；2）q中的词序是自由的，但也不是完全自由的，如果d和q中匹配ordered term，则相关性更强。

  有两类利用term dependency的方法：1）结合所有n-gram的匹配值；2）把n-gram的匹配值作为特征，然后利用L2R方法去训练ranking模型。

- 模型

  - Markov random ﬁelds模型

    > A markov random ﬁeld model for term dependencies. 2006.

    根据不同的独立性假设建图，定义团上的函数，学习每个团的权重。

    后续工作：融合query 扩展（图中包含扩展后的query term）、、、、、

  - extended IR 模型

    > Relevance ranking using kernels. 2010
    >
    > Dependence language model for information retrieval. 2004

    在原有的模型（BM25，language model）中加入term dependency。

  - feature of learning to rank

    > An exploration of proximity measures in information retrieval. 2007
    >
    > How good is a span of terms?: exploiting proximity to improve web retrieval. 2010
    >
    > Extracting search-focused key n-grams for relevance ranking in web search. 2012

    提取和term dependency相关的近似特征作为feature，再使用L2R模型去学习weight。

- 实验结果



## matching with translation model（word sense aspect）

- 统计机器翻译

  > Information retrieval as statistical translation. 1999

  把query视为一种语言，document视为另一种语言，统计机器翻译技术就可以用于解决mismatch问题。

  现有的机器翻译模型分为3类：word-based model、phrase-based model、syntax-based model。word-based 最经常被用于解决term mismatch问题。

- 模型（有监督）

  - Berger and Laﬀerty exploit word-based translation model
  - Gao et al. [68] also propose using phrase-based translation model.

- 实验结果



## matching with topic model（topic aspect）

- topic model
  - 概率：PLSI、LDA
  - 非概率（矩阵分解，基于的限制条件不同）：LSI、NMF、RLSI
- topic model for matching（无监督）
  - 线性结合topic model和term-based model
  - topic matching：得到 q 和 d 的 topic space 的表示后，计算距离
  - smoothing：用topic model作为language model的平滑项
- 实验结果



## matching with latent space model

- latent space model

  - 核心思想是：把query从query space映射到共同的隐空间，把document从document space映射到共同的隐空间，再计算相关性度量。当隐空间的维度小于原空间时，就可以解决term mismatch的问题。
  - 和topic model的区别是，topic model的隐空间的每个维度是一个topic，但latent space model的每一维没有具体的含义。
  - 传统的term-based模型（VSM、BM25、language model）都可以视为latent space model的特例，即隐空间的维度等于原空间。

- 模型（监督）

  - partial least square (PLS) ？？？ ----- 线性

    > Overview and recent advances in partial least squares. 2006

  - regularized mapping to latent space (RMLS)  ----- 线性

    > Learning bilinear model for matching queries and documents. 2013

  - supervised semantic indexing (SSI)  ----- 线性

    > Supervised semantic indexing. 2009
    >
    > Learning to rank with (a lot of) word features. 2010

  - bilingual topic model (BLTM)  ----- 线性

    > Clickthroughbased latent semantic models for web search. 2011

  - DSSM  ----- 非线性

- 实验结果



## learning to matching（泛化到其他matching任务）

- 一般化形式：learn to match
- 协同过滤方法
- paraphrasing 和 textual entailment 方法
- 这些方法可能应用到search任务中



## 总结和开放性问题

- 总结
  - Query-document mismatch 是搜索中的一大挑战。这种mismatch主要来自于自然语言的丰富性和灵活性。
  - 一种解决该挑战的方法是：对query和document进行更深层次的分析和理解，再基于这些query和document表示进行匹配，即语义匹配。这些query和document的表示可以perform在form, phrase, sense, topic, and structure aspects。
  - 最近提出的5类方法：query reformulation、term dependency model、translation model、topic model、latent space model
  - 其他任务场景下的match问题（扩展）
- 5类方法之间的比较
- 其他方法
  - document reformulation
  - query分类、document分类
  - 加入命名实体识别技术
  - 融合人类知识
- 未来的方向
  - topic drift
  - 可扩展性
  - 长尾问题
  - 利用NLP技术
  - 相关性评估















