> > 2015，递归神经网络，循环神经网络

<center>When Are Tree Structures Necessary for Deep Learning of Representations?</center>

## 背景

本文在**4个任务上比较了递归模型和循环模型**，用于探究在哪些任务上循环模型试足够的，在哪些任务上需要递归模型提供额外的优势：

1）**句子层次和短语层次的二进制情感分类**（用于理解递归模型在语义组合上的表现）

2）**query 和 phrase answer 的问答匹配**（用于比较循环模型和递归模型的中间表达，并探究在相似度匹配上，句法解析是否是有用的）

3）**语义关系分类**（用于探究句法解析是否可以帮助解决长距离依赖）

4）**篇章解析**（用于探究句法解析可以帮助篇章任务的程度，篇章任务需要更长文本单元上的语义组合）



## 方法

#### 递归神经网络

* 使用句法解析树从下而上地递归生成编码表示。

* 递归模型的一个优势是，可以依赖句法树来捕捉长期依赖（句法上联系紧密的两个词可能在句子中距离较远），但是这种依赖也是缺点，因为句法解析树的生成较慢，其实域相关的，也容易造成错误传递。

* 标准的 Tree model

  <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191027215509296.png" alt="image-20191027215509296" style="zoom:50%;" />

#### 循环神经网络

* 循环神经网络除了词序外，没有考虑其他的语言结构。

* 标准的RNN

  <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191027215416472.png" alt="image-20191027215416472" style="zoom:50%;" />



##实验

#### 实验设置

> 两组对比实验方法：
>
> <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191027220051920.png" alt="image-20191027220051920" style="zoom:50%;" />
>
> 使用统一的AdaGrad优化器和batch_size等超参。
>
> 为了公平起见，大部分模型都只使用一层的结构。

#### 实验

1. Binary sentiment classiﬁcation（监督信号仅出现在最后）

   * 数据集： Internet Movies Database (IMDB) movie review（每个句子对应一个“pos”或者“neg”标签，平均句子长度22.5）

   * 实验方法：word embedding 使用 skip-gram（自己训练的） 进行初始化，并在训练过程中freeze。得到的50维句子表示通过sigmoid分类器。

   * 实验结果：

   > <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191027221911857.png" alt="image-20191027221911857" style="zoom:50%;" />
   >
   > Tree model 没有表现出明显的优势，原因可能在于，监督信号是在整个句子上，没有在局部，不清楚Tree model 是否真正学好了局部表达。

2. Sentiment Classiﬁcation on the Stanford Sentiment Treebank （在词和短语层次上也有监督信号）

   * 数据集：Stanford Sentiment Treebank（一共包括11855个句子，215154个短语，分别在短语层次和句子层次进行评估和比较）

   * 实验方法：RecNN 和 RNN 对应节点或者对应步 的输出 通过softmax分类器。

   * 实验结果：

   > <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191027221008944.png" alt="image-20191027221008944" style="zoom:50%;" />
   >
   > 1) 在短语层次，并没有看到 Tree model 的明显优势（也就是说序列模型可以学好局部，但学不好整个句子的组合）；
   >
   > 2) 对长序列，子句分割分别进行双向建模后再单向进行合并，可以提供对Tree model的简单近似

3. Sentence-Target Matching

   * 数据集：question-answering dataset QANTA（每个answer是一个词或短语），该任务被形式化为多类别的分类问题，从候选的短语池中选择出正确答案。

   * 实验方法：

     > <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191027222817826.png" alt="image-20191027222817826" style="zoom:50%;" />
     >
     > <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191027222909229.png" alt="image-20191027222909229" style="zoom:50%;" />

   * 实验结果：

     > <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191027223024902.png" alt="image-20191027223024902" style="zoom:40%;" />
     >
     > 结果证明，对于对齐target与不同source component的任务，序列模型的component也编码了重要的信息，尽管在序列模型中component仅代表一个文本段，而不是递归模型中的在语言学上有意义的component。

4. Semantic Relation Classiﬁcation

   * 数据集：SemEval-2010 task8（寻找名词对之间的语义关系，是19类的分类任务）

   * 实验方法：

     >  对于递归模型：根据句法树找到两个名词之间的路径，并把最近的一个公共节点的表达喂入softmax分类器。
     >
     >  对于序列模型：找相应的名词的embedding。（论文没具体说）
     >
     >  <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191027224443413.png" alt="image-20191027224443413" style="zoom:33%;" />

   * 实验结果

     > <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191027225304570.png" alt="image-20191027225304570" style="zoom:50%;" />
     >
     > 递归模型比循环模型的结果好很多，可能的原因是很远的两个词在循环模型中不太容易捕捉相互作用特征。

5. Discourse Parsing

   * 数据集：RST-DT corpus（基本篇章单元EDU是short clause，平均长度是7.2。。两个相邻的EDU的表达被送进分类器，如果两者有关，则将其合并成一个新的EDU）

   * 实验结果：

     > <img src="/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191027230028903.png" alt="image-20191027230028903" style="zoom:50%;" />
     >
     > 两种模型没有明显差异。

####实验结果

1. 递归模型在需要建模长距离词的依赖时比较有用（实验4语义关系抽取）。
2. 在长序列且有足够的局部监督信号时，Tree model在短语层次上没有明显优势，但在root层次上可以表现更好。。但当循环模型使用双向时，可以弥补两者之间的gap。。更进一步，通过将长序列划分成子句，对子句分别序列建模后再合并，也可以使得循环模型达到递归模型的性能。（实验2）
3. 在长序列但没有局部监督信号时，Tree model没有表现出明显的优势。（实验1）
4. 在循环结构中无语言学意义的component也可以编码有意义的信息。有研究结果表明，LSTM也可以隐式地发现递归组合结构。（实验3）