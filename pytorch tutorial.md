# 入门

### What is PyTorch

1. $\color{red}{Torch.empty ？？？}$

   ![image-20191017214833370](/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191017214833370.png)

2. torch.size 的结果是一个元组

3. $\color{red}{y=x.view(.....)     y到底是否和x共享内存 ？？？？？？}$

   $\color{red}{y=x.reshape(....)   y到底是否和x共享内存 ？？？？？}$

   $\color{red}{reshape的背后可能是view()，也可能是copy()   ？？？？？？}$

4. tensor 转 numpy：np_a = torch_a.numpy()     ## 两者共享内存（cpu），改变一个会使另一个发生变化

   numpy 转 tensor：torch_a = torch.from_numpy(np_a)     ## 两者共享内存（cpu），改变一个会使另一个

   ​                                  发生变化

### Auto Gradiention

1. 计算图是由tensor节点和function节点组成的。

   每个tensor节点有一个grad_fn属性，是由生成该tensor的function决定。用户自己定义的tensor的grad_fn属性是None。

2. detach()可以将一个tensor节点从计算图中剥离出来

3. a.backward()

   如果a是一个scalar，则backward不用传任何参数，否则，需要传一个和a相同size的tensor参数。

### Neural Network

### Training a Classifier

1. 加载模型

   ```python
   net.load_state_dict(torch.load(path))
   ```

2. 使用GPU

   ```python
   model.to(device)  # recursively go over all modules and convert their parameters and 
                     # buffers to CUDA tensors
   data.to(device)
   ```

### Data Parallel

1. 使用多GPU

   ```python
   model = torch.nn.DataParallel(model)
   
   # save
   torch.save(model.module.state_dict(), checkpoint)
   
   # restore
   model.module.load_state_dict(torch.load(path))      
   ```

### DataSet & Dataloader & Transform

1. Dataset

   继承自`torch.utils.data.Dataset`，主要需要重写 `__init__()`、`__len__()`、`__getitem__()` 函数，可以在init()函数中传入transform回调函数。

2. Transform

   主要需要自己写 `__init__()`、`__call__() `函数。

   `torchvision.transforms` 中有一些图像常用的transform类。

   `torchvision.transforms.Compose` 可以把多个transform类进行组合。

3. DataLoader

   `torch.utils.data.DataLoader`

   可以通过`collate_fn`参数指定样本如何被batched。

###TensorBoard

1. `torch.utils.tensorboard` 

   ```PYTHON
   from torch.utils.tensorboard import SummaryWriter
   writer = SummaryWriter('runs/fashion_mnist_experiment_1')   # 指定logdir
   
   在命令行中输入：tensorboard --logdir=runs
   https://localhost:6006
   
   writer.add_image('four_fashion_mnist_images', img)    # 把一张图片加到tensorboard中
   writer.add_graph(net, images)   # 可视化模型
   writer.add_embedding(....)  # 可以把输入数据降维，然后就可以可视化，并以不同的标签显示不同的颜色
   writer.add_scalar(running_loss)  # 记录loss
   
   writer.close()
   ```

### Named Tensor

$\color{red}{refine\_names??}$

```PYTHON
import torch
imgs = torch.randn(1, 2, 2, 3, names=('N', 'C', 'H', 'W'))
# 如果不指定names，则默认各维度的名字是None
print(imgs.names)

# 也可以只指定部分维度的名字
imgs = torch.randn(3, 1, 1, 2, names=('N', None, None, None))

# 重命名
imgs.names = ['batch', 'channel', 'width', 'height']   # 方式1
imgs = imgs.rename(channel='C', width='W', height='H')  # 方式2

# 去掉名字
imgs = imgs.rename(None) 

# 大部分简单的操作是支持传播名字的
print(named_imgs.abs().names)   # 会传播 named_imgs.names

# 只有name能够匹配，或者其中一个为空时才能进行操作
x = torch.randn(3, names=('X',))
y = torch.randn(3)
z = torch.randn(3, names=('Z',))
x + z  # 会报错
print((x + y).names)  # ('X',)

# flatten 三个维度，新的维度重新命名
imgs = imgs.flatten(['C', 'H', 'W'], 'features')
imgs = imgs.unflatten('features', (('C', 2), ('H', 2), ('W', 2)))
```



# 图像



# 文本

### Classifying names with a character-level RNN

###Generating names with a character-level RNN

$\color{red}{**往下的没看**}$

# 强化学习



# 进阶

## Deploying PyTorch Models in Production

## Parallel and Distributed Training

## Quantization

## PyTorch Fundamentals In-Depth



# 额外

### torch.optim.lr_scheduler.ReduceLROnPlateau

![image-20191021151659510](/Users/caiyinqiong/Library/Application Support/typora-user-images/image-20191021151659510.png)































