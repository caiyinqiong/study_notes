> > SIGIR2019，京东

# 背景

在ranking阶段使用深度模型，是为了减少query和item之间的语义鸿沟。

在reranking阶段使用深度模型，是为了更好地利用稀疏特征，e.g.用户点击/购买历史。（因为可以把原始特征转化成embedding）



# 模型

- ranking 阶段：deep semantic retrieval

  <img src="../../images/image-20200610224824606.png" alt="image-20200610224824606" style="zoom:33%;" />

  <img src="../../images/image-20200610224853571.png" alt="image-20200610224853571" style="zoom:33%;" />

  <img src="../../images/image-20200610224914373.png" alt="image-20200610224914373" style="zoom:33%;" />

- reranking阶段：Deep pairwise ranking

  <img src="../../images/image-20200610225040456.png" alt="image-20200610225040456" style="zoom:33%;" />

  <img src="../../images/image-20200610225108667.png" alt="image-20200610225108667" style="zoom:33%;" />



