##### ANN等快速搜索算法

- [ ] Ann-benchmarks: A benchmarking tool for approximate nearest neighbor algorithms.（2017）

- [ ] Fast Item Ranking under Neural Network based Measures（WSDM2020，基于图的近似近邻检索）

- [ ] 【ANN，Euclidean distance】Nearest-neighbor Searching and Metric Space Dimensions.

- [ ] 【ANN，Cosine distance】Scaling Up All Pairs Similarity Search



# ANN

**brute-force搜索的方式是在全空间进行搜索，为了加快查找的速度，几乎所有的ANN方法都是通过对全空间分割，将其分割成很多小的子空间，在搜索的时候，通过某种方式，快速锁定在某一（几）子空间，然后在该（几个）子空间里做遍历**。可以看到，正是因为缩减了遍历的空间大小范围，从而使得ANN能够处理大规模数据的索引。

- 基于树的方法

  > - 基于树的方法采用**树**这种数据结构的方法来表达对全空间的划分。
  >
  > - KD树是其下的经典算法。一般而言，在空间维度比较低时，KD树的查找性能还是比较高效的；但当空间维度较高时，该方法会退化为暴力枚举，性能较差，这时一般会采用下面的哈希方法或者矢量量化方法。
  > - Annoy：Annoy是[Erik Bernhardsson](https://github.com/erikbern)写的一个以树为数据结构的近似最近邻搜索库，并用在[Spotify](https://www.spotify.com/)的推荐系统中。

  - [x] 【KD树】**Multidimensional binary search trees used for associative searching**（1975）

  - [ ] An algorithm for ﬁnding best matches in logarithmic expected time.（1977）

  - [ ] An optimal algorithm for approximate nearest neighbor searching ﬁxed dimensions.（1998）

  - [x] 【KD树】Shape indexing using approximate nearest-neighbour search in high-dimensional spaces

  - [x] 【M树】Satisfying General Proximity/Similarity Queries with Metric Trees

  - [x] 【MIPS】Maximum inner-product search using cone trees（KDD2012）

    > In this paper, we consider the problem of eﬃciently ﬁnding the best match for a query from a given set of points with respect to the <u>inner-product similarity</u>.
    
  - [ ] Scalable nearest neighbor algorithms for high dimensional data.（2014）

  - [ ] Rank-based similarity search: Reducing the dimensional dependence.（2014）

- 哈希方法

  > - 就是将连续的实值散列化为0、1的离散值。在散列化的过程中，对散列化函数(也就是哈希函数)有一定的要求。根据学习的策略，可以将哈希方法分为无监督、有监督和半监督三种类型。
  > - 对于小数据集和中规模的数据集(几个million-几十个million)，基于LSH的方法的效果和性能都还不错。这方面有3个开源工具NMSLIB、[LSHash](https://github.com/kayzhu/LSHash)和[FALCONN](https://falconn-lib.org/)。
  - [x] 【LSH】**Approximate Nearest Neighbors: Towards Removing the Curse of Dimensionality**（1998）
  - [x] 【LSH】Similarity Search in High Dimensions via Hashing.（1999）
  - [x] 【LSH】Similarity Estimation Techniques from Rounding Algorithms（2002）
  - [x] 【LSH】LSH forest: Self tuning indexes for similarity search.（2005）
  - [x] 【LSH】Multi-Probe LSH: Efficient Indexing for High-Dimensional Similarity Search（2007）
  - [x] 【LSH】Spherical LSH for Approximate Nearest Neighbor Search on Unit Hypersphere（2007）
  - [x] 【LSH】Practical and Optimal LSH for Angular Distance（2015，FALCONN）
  - [x] 【局部敏感哈希】Near-optimal hashing algorithms for approximate nearest neighbor in high dimensions.（2006）
  - [x] 【局部敏感哈希】Locality-sensitive hashing scheme based on p-stable distributions.（2004）
  - [ ] Asymmetric lsh (alsh) for sublinear time maximum inner product search (mips).（2014）
  - [ ] A new unbiased and efﬁcient class of lsh-based samplers and estimators for partition function computation in log-linear models（2017）
  - [ ] 【距离敏感哈希】Distance-sensitive hashing（2018）

- 矢量量化方法

  > - 矢量量化方法，即vector quantization。在矢量量化编码中，关键是码本的建立和码字搜索算法。比如常见的聚类算法，就是一种矢量量化方法。而在相似搜索中，向量量化方法又以PQ方法最为典型。
  > - 对于大规模数据集(几百个million以上)，基于矢量量化的方法是一个明智的选择，可以用用Faiss开源工具。
  
  - [x] 【PQ】**Product quantization for nearest neighbor search**.（2010）
  
    > 把原来的向量空间分解为若干个低维向量空间的笛卡尔积，并对分解得到的低维向量空间分别做量化（quantization）。这样每个向量就能由多个低维空间的量化code组合表示。
  
  - [ ] 【IVFPQ】
  
  - [ ] 【OPQ】Optimized product quantization（2013）
  
  - [ ] Billion-scale similarity search with GPUs.（2017，faiss）
  
  - [ ] Quantization based fast inner product search（2016）
- 基于图的方法

  > 基于图的ANN方法，在规模不是特别大但对召回要求非常高的检索场景下，是非常适用的。

  - [ ] 【HNSW】**Efficient and robust approximate nearest neighbor search using hierarchical navigable small world graphs**（2016）
  - [ ] SPTAG: A library for fast approximate nearest neighbor search, 2018.
  - [ ] Optimization of indexing based on k-nearest neighbor graph for proximity search in high-dimensional data.