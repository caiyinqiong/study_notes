> > 2019

# 背景

一般的开放域QA使用retriever-reranker-reader的方式。以往的reranker一般把question和document拼接起来进行相关度计算，但由于retriever返回的文档一般较多，所以这样处理的效率很低。



# 模型

![image-20200609094443921](../../images/image-20200609094443921.png)

<img src="../../images/image-20200609094502822.png" alt="image-20200609094502822" style="zoom:50%;" />



# 实验

##### 数据集：

SQuAD-open、Natural Question Open

##### 指标：

P@N：answer span出现在topN个文档内的概率

PBT@N：answer span出现在语义检索的topN个文档，但未出现在BM25topN个文档的概率

PTB@N：answer span出现在BM25topN个文档，单位出现在语义检索的topN个文档的概率

EM

##### 实验结果

<img src="../../images/image-20200609095016631.png" alt="image-20200609095016631" style="zoom:33%;" />

<img src="../../images/image-20200609095056045.png" alt="image-20200609095056045" style="zoom:33%;" />

![image-20200609095123367](../../images/image-20200609095123367.png)



# 结论

- 比较简单的模型，但在效率上可以提升不少。