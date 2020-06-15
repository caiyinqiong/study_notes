> > 2017，抽取式QA

# 背景

两个模块：

（1）document retriever：检索出与question最相关的topK个Wikipedia article

（2）document reader：从检索回的article的每个段落中预测 answer-span



# 模型

<img src="../../images/image-20200604090709940.png" alt="image-20200604090709940" style="zoom:33%;" />

##### Document Retriever（返回前5个document）

TFIDF词向量 + 2-gram + hashing

##### Document reader（对每个段落抽取答案）

Question  Encoding：glove embedding + 多层BiLSTM + self-attention 得到question的向量表示

Paragraph Encoding

输入：gloving embedding（只有question中前1000个最频繁的词向量在训练中微调，其他冻结）

​           与question的 精确匹配特征（包括原始匹配、小写匹配、lemma匹配）

​           paragraph的token 特征（词性、实体、词频）

​           与question的aligned question embedding（相当于一个attention） 

处理：多层双向LSTM   

输出：所有BiLSTM层输出的拼接

##### 预测

<img src="../../images/image-20200604091258214.png" alt="image-20200604091258214" style="zoom:50%;" />

<img src="../../images/image-20200604091325272.png" alt="image-20200604091325272" style="zoom:33%;" />

<img src="../../images/image-20200604091342571.png" alt="image-20200604091342571" style="zoom:33%;" />



# 实验

##### 数据集

SQuAD（训练时使用gold paragraph，在dev上评估时使用open domain）、CuratedTREC、WebQuestions、WikiMovies

##### 构造远程监督数据

<img src="../../images/image-20200604091624248.png" alt="image-20200604091624248" style="zoom:33%;" />

<img src="../../images/image-20200604091551230.png" alt="image-20200604091551230" style="zoom:33%;" />

##### **实验结果**

- retriever的性能

<img src="../../images/image-20200604091651959.png" alt="image-20200604091651959" style="zoom:33%;" />

- reader的性能（train和dev都是非开放域设置）

<img src="../../images/image-20200604091728544.png" alt="image-20200604091728544" style="zoom:33%;" />

<img src="../../images/image-20200604091900347.png" alt="image-20200604091900347" style="zoom:33%;" />

- DrQA在开放域设置下的性能

  <img src="../../images/image-20200604093401703.png" alt="image-20200604093401703" style="zoom:33%;" />



# 结论