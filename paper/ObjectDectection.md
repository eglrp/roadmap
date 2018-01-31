### Deep Learning approach

##### OverFeat 

##### R-CNN 

> Rich feature hierarchies for accurate object detection and semantic segmentation

- Extract possible object using a region proposal method ( Selective Search ).
- Extract feature from each region using a CNN.
- Classify each region with SVMs.

Generate proposals for the training dataset , apply the CNN feature extractino to every single one and then finally train the SVM classifiers.

##### Fast R-CNN

Similar to R-CNN , it used Selective Search  to generate region proposals , but instead of extracting all of them independently and using SVM classifiers , it applied the CNN on the complete image and then used both Region of Interest ( RoI ) Pooling on the feature map with final feed forward network for classification and regression.

The biggest downside was that the model still relied on Selective Search or any other region proposal algorithm , which became the bottleneck when using for inference.

##### YOLO

> You Only Look Onece : Unified , Real-Time Object Detection

YOLO proposed a simple convolutional neural network approach which has both great results and high speed , allowing for the first time real time object detection.

##### Faster R-CNN

Faster R-CNN added what they called a Region Proposal Network ( RPN ) , in an attempt to get rid of the Selective Search algorithm and make the model completely trainable end-to-end . 

##### SSD 

> Single Shot Detector

SSD which take on YOLO by using multiple sized convolutional feature maps achieving better results and speed .

##### R-FCN

> Region-based Fully Convolutional Networks

R-FCN which takes the architecture of Faster R-CNN but with only convolutional networks .