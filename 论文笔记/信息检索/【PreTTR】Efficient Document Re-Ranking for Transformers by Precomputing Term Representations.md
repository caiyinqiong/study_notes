> > SIGIR2020



# 模型

使用和DeFormer一样的结构。（再多一个压缩层和解压层）

<img src="../../images/image-20200614114925747.png" alt="image-20200614114925747" style="zoom:50%;" />

##### 压缩解压层

<img src="../../images/image-20200614115518301.png" alt="image-20200614115518301" style="zoom:50%;" />

<img src="../../images/image-20200614115541719.png" alt="image-20200614115541719" style="zoom:50%;" />



# 实验

##### 数据集

<img src="../../images/image-20200614115125688.png" alt="image-20200614115125688" style="zoom:50%;" />

使用TREC CAR预训练压缩/解压层。

##### 实验结果

- ranking性能

  <img src="../../images/image-20200614115213746.png" alt="image-20200614115213746" style="zoom:50%;" />

- 压缩层

  ![image-20200614115247584](../../images/image-20200614115247584.png)

- rerank的效率

  <img src="../../images/image-20200614115333592.png" alt="image-20200614115333592" style="zoom:50%;" />

- 使用不同的基础模型

  <img src="../../images/image-20200614115410424.png" alt="image-20200614115410424" style="zoom:50%;" />





# 结论