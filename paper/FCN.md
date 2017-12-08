# Fully Convolutional Networks for Semantic Segmentation

### 图像分割

- Semantic Segmentation 只标记语义，也就是说只分割出人这个类来。
- Instance Segmentation 标记实例和语义，不仅要分割出人这个类，而且要分割出是哪个人，也就是具体的实例。

### 网络结构

跟 Object Detection 相比，Semantic Segmentation 需要预测的边框更加精细。FCN（ Fully Convolutional Networks ）对图像进行像素级的预测，来解决语义级别的图像分割问题（ Semantic Segmentation ）。

经典的 CNN 分类网络在卷积层之后使用全连接层得到固定长度的特征向量进行分类，由于全连接层的存在，传统的 CNN 网络的输入图像尺寸一遍都是固定的。与此不同的是，FCN 网络接收任意尺寸的输入图像，采用反卷积层（ transposed convolution ）对最后一个卷积层产生的特征映射（ feature map ）进行上采样，使之恢复到输入图像相同的尺寸，从而产生了一个 piexl-wise 的预测，保留了图片的空间信息，最后在上采样的特征映射（ feature map ）上进行分类。

![net-architecture]()



CNN 的强大是在于他的多层结构可以学习到多个层次的特征。

- 浅层卷积的感知域（ receptive field ）小，可以学习到一些局部区域的特征。
- 深层卷积的感知域大，能够学习到一些更加抽象的特征。
- 高层的抽象特征对物体的大小，位置，方向等敏感度比较低，提高了识别的性能，因此我们通常把 CNN 看做一个特征提取器。

### Fully connected layer with convolutional layer mutual transformation

全连接层跟卷积层之间唯一不同的就是卷积层中神经元只与输入数据中的一个局部连接，并且在卷积中共享参数。

- 对于一个卷积层，都存在一个可以实现其相同的前向传播功能的全连接层。比如全连接层的权值网络是一个巨大的矩阵，除了某些特定的块，其余部分的权值都为零。而且由于卷积参数共性的性质，权值网络中的非零值大部分都是相等的。
- 任何的全连接层也都可以转化为卷积层。将卷积核K的大小设为输入图像的大小，output_channel的大小设为全连接层的神经元的个数，填充参数P为零，步长S随意。这样就可以把全连接层等效的看做一个卷积层。

### Transposed Convolution

卷积操作是在图像上进行一系列的下采样（ downsampling ），得到了图像的高维抽象特征。一般的 CNN 将高维的抽象特征通过全连接网络转化为得分向量来进行分类。

在全卷积网络里我们需要对输入图像进行像素级的分割效果，我们需要对高维特征图（ 论文里称为heatmap ) 进行上采样（ upsampling ）,把图像进行放大到原图像的大小。

上采样（ upsampling ）的过程就可以看做是反卷积（ Transposed Convolution）的过程。

反卷积也是卷积层，其实是卷积操作的前向传播跟反向传播的过程置换，卷积操作的前向传播是反卷积的后向传播，卷积的后向是反卷积的前向。并不是反卷积操作是卷积的逆变换。

关于反卷积更详细的解释，可以看[Transposed Convolution, Fractionally Strided Convolution or Deconvolution](https://buptldy.github.io/2016/10/29/2016-10-29-deconv/)。

反卷积操作也是卷积操作，同样适用于卷积网络的前向传播跟后向传播，因此FCN可以实现一个end-to-end的训练。

FCN全过程的示意图

![net-architecture2]()

### skip architecture

直接对 CNN 的结果进行 dense prediction 得到的分割结果比较粗糙，我们需要加入更多的前层细节信息。作者在论文里给出了示意图。

![fcn-net](https://github.com/quinwu/roadmap/blob/master/paper/FCN/fcn-net.png)

实验上来说，FCN-8s的效果是最好的。为什么不继续下去呢？作者在实验中继续往下发现效果又会变差，所以FCN-8s就是最好的效果了。

给出三种结构的Pytorch网络结构。

- [FCN-32s]()
- [FCN-16s]()
- [FCN-8s]()

### 总结

总的来说，论文的主要思路如下：

- 我们的目的是精确预测每个像素的分割结果。
- 需要经历下采样（ downsampling ）到上采样（ upsampling ）的两个过程。
- 在上采样的时候，分阶段采样比一次采样的效果要好，要跟下采样的对应层的特征进行叠加。

