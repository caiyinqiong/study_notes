##### ANN等快速搜索算法

- [ ] Ann-benchmarks: A benchmarking tool for approximate nearest neighbor algorithms.（2017）

- [ ] Fast Item Ranking under Neural Network based Measures（WSDM2020，基于图的近似近邻检索）

- [ ] 【ANN，Euclidean distance】Nearest-neighbor Searching and Metric Space Dimensions.

- [ ] 【ANN，Cosine distance】Scaling Up All Pairs Similarity Search



# KNN -> ANN

**brute-force搜索的方式是在全空间进行搜索，为了加快查找的速度，几乎所有的ANN方法都是通过对全空间分割，将其分割成很多小的子空间，在搜索的时候，通过某种方式，快速锁定在某一（几）子空间，然后在该（几个）子空间里做遍历**。可以看到，正是因为缩减了遍历的空间大小范围（dm，m<<N），从而使得ANN能够处理大规模数据的索引。

- 基于树的方法

  > - 基于树的方法采用**树**这种数据结构的方法来表达对全空间的划分。
  >
  > - KD树是其下的经典算法。一般而言，在空间维度比较低时，KD树的查找性能还是比较高效的；但当空间维度较高时，该方法会退化为暴力枚举，性能较差，这时一般会采用下面的哈希方法或者矢量量化方法。
  > - Annoy：Annoy是[Erik Bernhardsson](https://github.com/erikbern)写的一个以树为数据结构的近似最近邻搜索库，并用在[Spotify](https://www.spotify.com/)的推荐系统中。

  - [x] 【KD树】**Multidimensional binary search trees used for associative searching**（1975，在建树过程中，通过交替按照向量的每一个维度对集合中的向量进行划分，构建出一棵每个节点对应一个向量集合的搜索树。然而，KD树只有当向量维度$d$和向量数量满$N$足$2^d << N$时才能实现快速搜索，否则会退化至穷尽搜索的时间复杂度。）

  - [ ] An algorithm for ﬁnding best matches in logarithmic expected time.（1977）

  - [ ] An optimal algorithm for approximate nearest neighbor searching ﬁxed dimensions.（1998）

  - [x] 【KD树】Shape indexing using approximate nearest-neighbour search in high-dimensional spaces

  - [x] 【M树】Satisfying General Proximity/Similarity Queries with Metric Trees

  - [x] 【MIPS】Maximum inner-product search using cone trees（KDD2012）

    > In this paper, we consider the problem of eﬃciently ﬁnding the best match for a query from a given set of points with respect to the <u>inner-product similarity</u>.
    
  - [ ] Scalable nearest neighbor algorithms for high dimensional data.（2014）

  - [ ] Rank-based similarity search: Reducing the dimensional dependence.（2014）

  - [ ] 【KD Ball Tree】

  - [ ] 【Annoy算法】将二叉搜索树的思想应用于ANN问题当中，二叉树中每一个节点都对应着一个向量集合，其中根节点是需要索引的全部向量组成的集合。在建树阶段，从根节点开始自上而下进行递归，对于每个节点，通过k-means算法将当前节点中的向量聚为两个簇，并在节点中记录划分超平面的信息，然后递归划分直至节点中的向量个数小于等于$K$个。在查询阶段，从根节点开始，根据节点中记录的超平面信息，选择左儿子或者右儿子继续向下搜索，直到搜索至叶子节点为止；当查询向量与节点中记录的超平面接近时，也可以同时对左右儿子进行搜索，并通过beam search算法维持搜索空间的规模。

- 哈希方法

  > - 就是将连续的实值散列化为0、1的离散值。在散列化的过程中，对散列化函数(也就是哈希函数)有一定的要求。根据学习的策略，可以将哈希方法分为无监督、有监督和半监督三种类型。
  > - 对于小数据集和中规模的数据集(几个million-几十个million)，基于LSH的方法的效果和性能都还不错。这方面有3个开源工具NMSLIB、[LSHash](https://github.com/kayzhu/LSHash)和[FALCONN](https://falconn-lib.org/)。
  - [x] 【LSH】**Approximate Nearest Neighbors: Towards Removing the Curse of Dimensionality**（1998，核心思想是通过使用适当的散列函数，将空间中距离较近的向量，以较大概率映射为相同的哈希值。上述要求可以形式化地表示为，对于$x,y∈R^d$，LSH算法的散列函数$h$需要满足以下条件：当$Sim(x,y)≤S_0$时，$P(h(x)=h(y))≥P_1$；当$Sim(x,y)≥cS_0$时，$P(h(x)=h(y))≤P_2$。此时，称散列函数h为$(S_0,cS_0,P_1,P_2)$敏感的。在查询阶段，首先通过相同的散列函数将问题向量映射为一个哈希值，然后返回该哈希值对应的向量集合作为查询结果。LSH算法被提出时，仅在向量是只由0和1组成的二值向量，且距离度量函数为汉明距离的场景下适用。此时，通过调整散列表的个数以及每个散列表中键值的个数，可以根据实际需求实现对$(S_0,cS_0,P_1,P_2)$的更改。）
  - [x] 【LSH】Similarity Search in High Dimensions via Hashing.（1999）
  - [x] 【LSH】Similarity Estimation Techniques from Rounding Algorithms（2002）
  - [x] 【LSH】LSH forest: Self tuning indexes for similarity search.（2005）
  - [x] 【LSH】Multi-Probe LSH: Efficient Indexing for High-Dimensional Similarity Search（2007）
  - [x] 【LSH】Spherical LSH for Approximate Nearest Neighbor Search on Unit Hypersphere（2007）
  - [x] 【LSH】Practical and Optimal LSH for Angular Distance（2015，FALCONN）
  - [x] 【LSH】Near-optimal hashing algorithms for approximate nearest neighbor in high dimensions.（2006）
  - [x] 【LSH】Locality-sensitive hashing scheme based on p-stable distributions.（2004，设计了一族将$R^d$空间中的向量映射到整数空间$Z$的散列函数，使得映射之后哈希值相同的向量，在$R^d$空间中的L2距离接近。）
  - [ ] Asymmetric lsh (alsh) for sublinear time maximum inner product search (mips).（2014）
  - [ ] A new unbiased and efﬁcient class of lsh-based samplers and estimators for partition function computation in log-linear models（2017）
  - [ ] 【距离敏感哈希】Distance-sensitive hashing（2018）

- 矢量量化方法

  > - 矢量量化方法，即vector quantization。在矢量量化编码中，关键是码本的建立和码字搜索算法。比如常见的聚类算法，就是一种矢量量化方法。而在相似搜索中，向量量化方法又以PQ方法最为典型。
  > - 对于大规模数据集(几百个million以上)，基于矢量量化的方法是一个明智的选择，可以用用Faiss开源工具。
  
  - [x] 【PQ】**Product quantization for nearest neighbor search**.（2010，通过对向量进行量化操作，减少向量间距离计算的时间开销，从而实现对向量检索的加速。假设向量集合中共有$N$个$d$维向量，对于其中的第$i$个向量$x_i=(x_{i,1},x_{i,2},…,x_{i,d})$，乘积量化方法首先将其等分为$m$份，即$x_i=(μ_1 (x_i ),μ_2 x_i ),…,μ_m (x_i ))$，其中$μ_n (x_i )=(x_{i,(n-1)d/m+1},x_{i,(n-1)d/m+2},…,x_{i,n d/m})$；然后，对每一个$μ_n (x_i )$进行聚类，得到$k$个聚类中心；最后，将向量$x_i$量化表示为$q(x_i )=(q_1 (x_i ),q_2 (x_i ),…,q_m (x_i ))$，其中$q_n (x_i )$是与$μ_n (x_i )$距离最近的一个聚类中心。在完成对向量的量化之后，就可以用向量的量化结果代替原始向量计算向量之间的距离。由于向量每一部分的聚类中心均是已知的，通过预先计算并存表的方式，可以将计算查询向量与全部向量之间距离的时间复杂度，由$O(Nd)$降低至$O(Nm)$。实践中，乘积量化的方法经常结合倒排系统一起使用，进一步提高检索的速度。）
  
    > 把原来的向量空间分解为若干个低维向量空间的笛卡尔积，并对分解得到的低维向量空间分别做量化（quantization）。这样每个向量就能由多个低维空间的量化code组合表示。
  
  - [ ] 【IVFPQ】
  
  - [ ] 【OPQ】Optimized product quantization（2013，通过将向量与正交旋转矩阵相乘，解决当向量在空间中分布不均匀时PQ方法表现欠佳的问题）
  
  - [ ] Billion-scale similarity search with GPUs.（2017，faiss）
  
  - [ ] Quantization based fast inner product search（2016）
- 基于图的方法

  > 基于图的ANN方法，在规模不是特别大但对召回要求非常高的检索场景下，是非常适用的。

  - [ ] 【NSW】**Navigation in a small world**（将向量集合中的每一个向量，视为图中的一个节点。NSW算法要求建好的图中每个点与其距离较近的个点之间存在连边，并且整张图通过被称作“高速公路”的连边保持连通。在满足上述条件的图中进行查询时，可以缩小需要计算距离的向量集合的规模，从而实现对向量的快速检索。在建图阶段，依次将全部节点插入到图中，当插入第个节点时，在前个节点构建好的图中近似找到与当前节点距离最小的个点，然后建立连边。）
  - [ ] 【HNSW】**Efficient and robust approximate nearest neighbor search using hierarchical navigable small world graphs**（2016，利用跳表的思想对NSW算法进行了改进，通过层次化的建图以及自上而下的查询方式，进一步提升了NSW算法在向量检索时的效率和性能。）
  - [ ] SPTAG: A library for fast approximate nearest neighbor search, 2018.
  - [ ] Optimization of indexing based on k-nearest neighbor graph for proximity search in high-dimensional data.