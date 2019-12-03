> > WSDM2018，相似问题匹配（PI、NLI）、迁移学习，对抗学习

<center>Modelling Domain Relationships for Transfer Learning on Chatbot-based Question Answering Systems</center>

## 背景

有一类QA系统就是先找到相似问题，再把相似问题的答案作为回答。这类相似问题检索的任务可以形式化成PI或NLI任务。。目前的解决PI或NLI问题的方法存在的问题在于：1）依赖于大量的有标签数据；2）在商业应用中不够高效。

本文主要研究PI或NLI任务的迁移学习问题，提出了一个通用的框架，可以有效且高效地应用源域和目标域之间的共同知识，而且还同时学习域之间的关系。



## 迁移学习

经常用的迁移学习方法包括：

1. fully-shared model

   <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191122103304744.png" alt="image-20191122103304744" style="zoom:33%;" />

   通过一个共享的网络，得到句子对的表示Z_c;

   <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191122103430415.png" alt="image-20191122103430415" style="zoom:50%;" />

   这种方法忽略了源域和目标域特定的特征。

2. specify-shared model

   <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191122103323869.png" alt="image-20191122103323869" style="zoom:33%;" />

   包括一个共享的网络得到句子对的表示Z_c，两个域特定的网络得到句子对的表示Z_s和Z_t；

   <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191122103458675.png" alt="image-20191122103458675" style="zoom:50%;" />

   这种方法没有考虑softmax层参数的内在关系，即没有考虑域之间的关系。



## 模型

<img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191122104634930.png" alt="image-20191122104634930" style="zoom:67%;" />

在specify-shared model上进行改进：

1. 考虑域之间的关系

<img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191122103850414.png" alt="image-20191122103850414" style="zoom:43%;" />

<img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191122104008381.png" alt="image-20191122104008381" style="zoom:50%;" />

2. 对抗学习，保证sharedNN中不含特定域的噪声

   <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191122104249060.png" alt="image-20191122104249060" style="zoom:50%;" />

   

   <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191122104451282.png" alt="image-20191122104451282" style="zoom:50%;" />

   <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191122104514959.png" alt="image-20191122104514959" style="zoom:50%;" />

3. base model：基于表达+基于交互

   基于表达：分别通过conv1d+maxpooling得到<img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191122104821019.png" alt="image-20191122104821019" style="zoom:50%;" />

   ​                     <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191122104847242.png" alt="image-20191122104847242" style="zoom:50%;" />

   基于交互的：得到dot-product交互矩阵，再通过两层【conv+maxpooling】，最后flatten得到H_p。

   concat两个部分的结果：<img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191122104923458.png" alt="image-20191122104923458" style="zoom:50%;" />

4. 训练

   <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191122105119475.png" alt="image-20191122105119475" style="zoom:33%;" />

   

## 实验

1. 对PI任务，QQP是源域，自己的数据集（未公开）是目标域
2. 对NLI任务，SNLI是源域，MultiNLI是目标域