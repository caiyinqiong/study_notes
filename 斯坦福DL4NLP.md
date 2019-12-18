### lecture08：NMT

1. 传统的机器翻译

   - 基于规则

     > 使用一个双语词典

   - 基于统计

     > 从数据中学习一个概率模型。
     >
     > <img src="images/image-20191216153459968.png" alt="image-20191216153459968" style="zoom:33%;" />
     >
     > 关键是学习translate model，即词-词、短语-短语之间的对齐（alignment）
     >
     > 如何找到agrmax？Use a heuristic search algorithm to search for the best translation, discarding hypotheses that are too low probability。
     >

2. neural mechine translate

   - seq2seq

     > The sequence-to-sequence model is an example of a Conditional Language Model.
     >
     > <img src="images/image-20191216154552105.png" alt="image-20191216154552105" style="zoom:33%;" />
     >
     > decoding：
     >
     > - 贪婪解码：每一步只取最可能的词。
     > - 穷举解码：每一步都保留所有的可能分支。
     > - beam search：每一步只保留最可能的K个分支。（具体方法：对K个分支中的每个分支，先扩展并保留top-K最可能分支，然后再K^K次方个分支中保留top-K分支，并进行下一步解码）
     >
     >  停止解码的条件：
     >
     > <img src="images/image-20191216155505418.png" alt="image-20191216155505418" style="zoom:33%;" />
     >
     > 对短序列进行惩罚：
     >
     > <img src="images/image-20191216155603708.png" alt="image-20191216155603708" style="zoom:33%;" />

   - 一些未被解决的问题

     > 1. OOV的词
     > 2. Domain mismatch between train and test data
     > 3. Maintaining context over longer text
     > 4. Low-resource language pairs
     > 5. using common sense is still hard
     > 6. pick up biases in training data
     > 7. 不可解释

3. Attention 机制

   > <img src="images/image-20191216163051185.png" alt="image-20191216163051185" style="zoom:50%;" />

   > 一些常用的计算attention score的方法：
   >
   > <img src="images/image-20191216163850500.png" alt="image-20191216163850500" style="zoom:50%;" />



### lecture12：subwords

1. Byte Pair Encoding（BPE算法）

   > <img src="images/image-20191217203955956.png" alt="image-20191217203955956" style="zoom:50%;" />

   > <img src="images/image-20191217204030327.png" alt="image-20191217204030327" style="zoom:50%;" />

2. Sub-word models 的两个发展趋势：
   - Same architecture as for word-level model
   - Hybrid architectures（Main model has words; something else for characters）



### Lecture13：contextual representation

1. TagLM （'Pre-EMLo'）：Semi-supervised sequence tagging with bidirectional language models
2. EMLo：以语言模型为任务，学习上下文向量
3. CoVe：以机器学习任务，学习上下文的词向量

> <img src="images/image-20191217214125167.png" alt="image-20191217214125167" style="zoom:50%;" />

4. ULMFit
5. BERT

















