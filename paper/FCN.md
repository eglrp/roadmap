# Fully Convolutional Networks for Semantic Segmentation

### 图像分割

- Semantic Segmentation 只标记语义，也就是说只分割出人这个类来。
- Instance Segmentation 标记实例和语义，不仅要分割出人这个类，而且要分割出是哪个人，也就是具体的实例。

### 网络结构

跟 Object Detection 相比，Semantic Segmentation 需要预测的边框更加精细。FCN（ Fully Convolutional Networks ）对图像进行像素级的预测，来解决语义级别的图像分割问题（ Semantic Segmentation ）。

经典的 CNN 分类网络在卷积层之后使用全连接层得到固定长度的特征向量进行分类，由于全连接层的存在，传统的 CNN 网络的输入图像尺寸一遍都是固定的。与此不同的是，FCN 网络接收任意尺寸的输入图像，采用反卷积层（ transposed convolution ）对最后一个卷积层产生的特征映射（ feature map ）进行上采样，使之恢复到输入图像相同的尺寸，从而产生了一个 piexl-wise 的预测，保留了图片的空间信息，最后再上采样的特征映射（ feature map ）上进行分类。

![net-architecture]()

### Fully connected layer with convolutional layer mutual transformation

全连接层跟卷积层之间唯一不同的就是卷积层中神经元只与输入数据中的一个局部连接，并且在卷积中共享参数。

- 对于一个卷积层，如果权值网络是一个巨大的矩阵，除了某些特定的块，其余部分的权值都为0。
- ​

### Transposed Convolution



![fcn-net](https://github.com/quinwu/roadmap/blob/master/paper/FCN/fcn-net.png)