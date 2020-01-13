> >ACL2019，



## 论文分析什么问题

中文分词对于基于深度学习的中文自然语言处理是否必要的问题。

因为中文处理的第一步总是分词（简单惯性思维），但是它的必要性却很少被探究，作者希望通过此文章唤醒研究者重新思考在当下基于深度学习环境下的中文自然语言处理中的分词的必要性。（对于基本问题的思考）



## 实验设计

对比 基于词的（需要分词）和基于字的（不需要分词）这两个神经模型。

在四个端到端的NLP基准任务上： 语言模型，机器翻译，句子matching/paraphrase， 文本分类。

### 语言模型

基于给出的上下文表达，预测下个词出现的概率
数据集： Chinese Tree-Bank 6.00(CTB6)
0.8 training, 0.1 validation, 0.1 test
分词通过jieba进行[后面又补充了通过斯坦福CWS包和LTP包来做分词，防止被说分词方法过于简单造成的影响，但实验结果和jieba的近似]，LSTM作为基本模型来分别编码基于词的或者基于字的上下文表示。

### 机器翻译

数据集：
训练集：1.25M 句子对， 从LDC 语料（LDC2002E18, LDC2003E07, LDC2003E14, Hansards portion of LDC2004T07, LDC2004T08 and LDC2005T06.）中提取
验证集： NIST 2002
测试集： NIST 2003, 2004, 2005, 2006和 2008
模型：标准的seq2seq + attention
同样对比时只把word-level encoding改成 char-level encoding

### Sentence Matching/Paraphrase

数据集：BQ, LCQMC
分词用Jieba， 模型用the bilateral multi- perspective matching model （BiMPM）

### 文本分类

数据集： ChinaNews, Ifeng, JD_Full, JD_binary, Dianping
模型：双向LSTM



## 观察的现象和结论

### 终极结论：

作者自身在本文最大的Motivation 其实是在抨击分词不如不分，就像我们在IR中遇到的OOV问题，基于字的模型可以很好的解决这类问题。所以基本上作者做的实验结果也都是基于字的模型要好。他的论点主要有如下两点：
1、词的数据稀疏性，因为大部分汉字的词频都很低，所以我们容易遇到数据稀疏性，造成OOV（out-of-vocabulary）问题
2、现在的SOTA分词算法的性能还较差，所以其分词所造成的误差会对下游的NLP任务造成影响

### 语言模型

![image](http://forum.deepaccess.cn/uploads/default/original/1X/490157fe369d1c6fd1d051a451f45baf1c042478.png)
现象：
基于字的模型明显比基于词的模型效果好，且效果最好。
混合模型效果介于两者之间。[组合词和字的向量表示后，通过CNN，使得输入LSTM语言模型的维度相等]
结论：
在语言模型的任务中，分词没有提供任何效果提升，包含词的embedding 会使结果变差。指向最终结论中的第一条char 比 word-based model 更能解决数据稀疏性，OOV的问题。

### Sentence Matching/Paraphrase

![image](http://forum.deepaccess.cn/uploads/default/optimized/1X/65032d4a3f6e9e7e7cd126c58ab2298fd46d79da_2_517x73.png)



### 文本分类

[![image](http://forum.deepaccess.cn/uploads/default/optimized/1X/839dac263dae90009c65060b7c8641b3575931c6_2_517x108.png)](http://forum.deepaccess.cn/uploads/default/original/1X/839dac263dae90009c65060b7c8641b3575931c6.png)


可以看到模型相似，实验现象基本上都是基于字的好，作者的分析说辞也都很统一，基本上都在冲着OOV开火，呵呵