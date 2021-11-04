# Pytorch



## nn.Flatten()

```python
import torch.nn as nn
nn.Flatten()
```

该操作是展平输入矩阵，batch层保留



## torch.matmul()

![img](https://img-blog.csdnimg.cn/20200427102034288.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FzbXg2NjY=,size_16,color_FFFFFF,t_70)

将b的第0维1broadcast成2提出来，后两维做矩阵乘法即可



![在这里插入图片描述](https://img-blog.csdnimg.cn/20200427102557964.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FzbXg2NjY=,size_16,color_FFFFFF,t_70)

首先把a的第0维2作为batch提出来，则a和b都可看作三维。再把a的1 broadcat成5，提取公因式5。（这样说虽然不严谨，但是便于理解。）然后a剩下(3,4)，b剩下(4,2)，做矩阵乘法得到(3,2)

**总之永远最低两维做乘法**





## [ PyTorch 两大转置函数 transpose() 和 permute()](https://blog.csdn.net/xinjieyuan/article/details/105232802)

```python
torch.transpose(input, dim0, dim1, out=None) → Tensor
```

函数返回输入矩阵`input`的转置。交换维度`dim0`和`dim1`



## 无法显示绘图

pycharm里无法显示Matplotlib绘图的情况。这是由于matplotlib.imshow()导致的，在绘图参数等语句和plt.imshow()之后加上plt.show()，即可正常显示Matplotlib绘图。



## Broadcasting

两个 Tensors 只有在下列情况下才能进行 broadcasting 操作：

- 每个 tensor 至少有一维

- 遍历所有的维度，从尾部维度开始，每个对应的维度大小**要么相同，要么其中一个是 1，要么其中一个不存在**。

  

```python
# 按照尾部维度对齐
x=torch.empty(5,1,4,1)
y=torch.empty(  3,1,1)
(x+y).size()
# 结果维度
torch.Size([5, 3, 4, 1])

# 但是：
>>> x=torch.empty(5,2,4,1)
>>> y=torch.empty(  3,1,1)
# x 和 y 不能 broadcasting, 因为尾3维度 2 != 3
```



## 维度合并 (cat), 不改变Size维度

要保证其他维度是相同的

```python
import torch

a = torch.rand(4, 32, 8)
b = torch.rand(5, 32, 8)
print(torch.cat([a, b], dim=0).shape)
```

运行结果：

```python
torch.Size([9, 32, 8]
```



##  合并新增(stack), 增加Size维度

stack需要保证两个Tensor的shape是一致的，这就像是有两类东西，它们的其它属性都是一样的（比如男的一张表，女的一张表）。使用stack时候要指定一个维度位置，在那个位置前会插入一个新的维度，因为是两类东西合并过来所以这个新的维度size是2，通过指定这个维度是0或者1来选择性别是男还是女。

```python
c = torch.rand(4, 3, 32, 32)
d = torch.rand(4, 3, 32, 32)
print(torch.stack([c, d], dim=2).shape)
print(torch.stack([c, d], dim=0).shape)
```

运行结果：

```python
torch.Size([4, 3, 2, 32, 32])
torch.Size([2, 4, 3, 32, 32])
```



## 对Tensor的拆分(split) (chunk)

#### 按照size的长度拆分(split)

对一个Tensor而言，要拆分的那个维度的size就是"这个维度的总长度"了，可以指定拆分后的几个Tensor各取多少长度，或者指定每个Tensor取多少长度。

```python
import torch

a = torch.rand(2, 4, 3, 32, 32)
a1, a2 = a.split(1, dim=0)  # 对0号维度拆分,拆分后每个Tensor取长度1
print(a1.shape, a2.shape)

b = torch.rand(4, 3, 32, 32)
b1, b2 = b.split([2, 1], dim=1)  # 对1号维度拆分,拆分后第一个维度取2,第二个维度取1
print(b1.shape, b2.shape)
```

运行结果：

```python
torch.Size([1, 4, 3, 32, 32]) torch.Size([1, 4, 3, 32, 32])
torch.Size([4, 2, 32, 32]) torch.Size([4, 1, 32, 32])
```



#### 按照份数等量拆分(chunk)

给定在指定的维度上要拆分得的份数，就会按照指定的份数尽量等量地进行拆分。

```python
c = torch.rand(7, 4)
c1, c2, c3, c4 = c.chunk(4, dim=0)
print(c1.shape, c2.shape, c3.shape, c4.shape)
```

运行结果：

```python
torch.Size([2, 4]) torch.Size([2, 4]) torch.Size([2, 4]) torch.Size([1, 4])
```

## 矩阵乘法

使用`Torch.mm`（只能用于2d的Tensor）或者`Torch.matmul`（和符号`@`效果一样）。



![image-20210830132629119](/home/hp/.config/Typora/typora-user-images/image-20210830132629119.png)





























