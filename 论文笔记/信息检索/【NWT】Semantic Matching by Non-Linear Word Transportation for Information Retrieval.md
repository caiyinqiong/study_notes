> > CIKM2016



### 思想

根据word mover's distance的思想，把检索问题形式化为一个transportation problem。

### 模型

<img src="../../images/image-20200910154351962.png" alt="image-20200910154351962" style="zoom:50%;" />

<img src="../../images/image-20200910154407709.png" alt="image-20200910154407709" style="zoom:50%;" />

<img src="../../images/image-20200910154503074.png" alt="image-20200910154503074" style="zoom:50%;" />

### 实验

数据集

<img src="../../images/image-20200910154545331.png" alt="image-20200910154545331" style="zoom:50%;" />

实验结果

注：LDA-based 和 word embedding based 方法都使用QL进行fullrank，再对top 2000进行rerank。

![image-20200910154631964](../../images/image-20200910154631964.png)













