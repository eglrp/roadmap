# Rich feature hierarchies for accurate object detection and semantic segmentation

R-CNN 区域卷积神经网络，将 CNN 应用在 Object Detection 中的一篇开山之作。
Object Detection 跟 Object Classification 的区别在于现实的图片很复杂，可能不仅仅包含一个主题物体，Object Detection 不仅是分析图片中有什么，同样还需要识别它在什么位置。

- Object Classification 只需要输出图片的主体物体的分类。但是 Object Detection 必须能够识别出多个物体，一般这类任务叫**多类物体检测**。
- Object Classification 通常只需要输出将图片识别为某一类的概率，Object Detection 不仅仅要识别出分类的概率，还需要标识出每个物体在图片中的位置，通常我们会用方框来进行标识，称为边界框( bounding box )。

### R-CNN 的主要步骤
- 对于输出的每张 image 都通过一个 Selective Search 算法来选择多个的 proposal region。
- 在预训练的 CNN 中去掉最后的输出层，将每个 proposal region 调整(crop or warp)成为 CNN 网络要求的输入大小输入到 CNN 中进行特征提取。
- 对于 CNN 输出的每一个 proposal region 的特征，通过一个 SVM Classification 判断该 proposal region 是否包含某类物体。（ N 个 SVM 二分类来完成一个 N 分类）
- 同时，使用这些 region feature 训练 BBox Reg 来完成 bunding box 的任务。

### R-CNN 的不足
R-CNN 确实很惊艳，直观上也很好理解，主要的问题是它可能很慢。
对于每一张 image ，我们都需要经过 Selective Serach 来选择上千个 proposal region （大约是2000个左右）。然后在每一个 proposal region 上进行特征提取，导致每个 image 都需要做上千个prediction。