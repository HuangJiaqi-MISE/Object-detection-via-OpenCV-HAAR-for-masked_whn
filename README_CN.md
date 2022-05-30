# 基于OpenCV-Haar训练口罩检测Masked-Face-Dataset数据集

以下内容是利用opencv自带的训练器opencv_traincascade.exe与opencv_createsamples.exe，来对口罩数据集进行训练。

##环境准备

操作系统： Windows

* opencv opencv最好是较低版本的，如：OpenCV3.4.1，因为后面所需的一些文件（opencv_createsamples.exe和opencv_traincascade.exe）高版本的opencv是不自带的


##数据集准备

训练集应包括以下内容：
1. 正样本图像：这些图像只包含你试图检测的物体，如：口罩。
2. 负样本图像：这些图像可以包含任何东西，除了你要检测的对象之外。

数据集需要需要分为下列两部分:
nag & pos，其对应程序根目录中的负样本图像存储文件夹与正样本图像文件夹。

* opencv_createsamples.exe： 用于创建多个测试样本。根据调用的参数列表，会执行四种功能：从一张图像中通过扭曲形变创建多个训练样本；从一张图像中通过扭曲形变创建多个测试样本；通过描述文件的图片列表清单中创建训练样本；显示VEC文件中的样本图片。

* opencv_traincascade.exe：用于Haar模型的训练。

通用参数：

-data <cascade_dir_name>
目录名，如不存在训练程序会创建它，用于存放训练好的分类器。

-vec <vec_file_name>
包含正样本的vec文件名（由 opencv_createsamples 程序生成）。

-bg <background_file_name>
背景描述文件，也就是包含负样本文件名的那个描述文件。

-numPos <number_of_positive_samples>
每级分类器训练时所用的正样本数目。其指设置为正样本数量的85%（这是一个保守值）。具体的也要根据级联器的层数来决定的。因为每个stages都是会增加图片数量来进行分类。

-numNeg <number_of_negative_samples>
每级分类器训练时所用的负样本数目，可以大于 -bg 指定的图片数目。

-numStages <number_of_stages>
训练的分类器的级数。

-precalcValBufSize <precalculated_vals_buffer_size_in_Mb>
缓存大小，用于存储预先计算的特征值(feature values)，单位为MB。

-precalcIdxBufSize <precalculated_idxs_buffer_size_in_Mb>
缓存大小，用于存储预先计算的特征索引(feature indices)，单位为MB。内存越大，训练时间越短。

-baseFormatSave
这个参数仅在使用Haar特征时有效。如果指定这个参数，那么级联分类器将以老的格式存储。
级联参数：

-stageType <BOOST(default)>
级别（stage）参数。目前只支持将BOOST分类器作为级别的类型。

-featureType<{HAAR(default), LBP}>
特征的类型： HAAR - 类Haar特征； LBP - 局部纹理模式特征。

-w
-h
训练样本的尺寸（单位为像素）。必须跟训练样本创建（使用 opencv_createsamples 程序创建）时的尺寸保持一致。
Boosted分类器参数：

-bt <{DAB, RAB, LB, GAB(default)}>
Boosted分类器的类型： DAB - Discrete AdaBoost, RAB - Real AdaBoost, LB - LogitBoost, GAB - Gentle AdaBoost。

-minHitRate <min_hit_rate>
分类器的每一级希望得到的最小检测率。总的检测率大约为 min_hit_rate^number_of_stages。总检测率即为整个级联器的检测召回率，

-maxFalseAlarmRate <max_false_alarm_rate>
分类器的每一级希望得到的最大误检率。总的误检率大约为 max_false_alarm_rate^number_of_stages. 为整个级联器的误检率

-weightTrimRate <weight_trim_rate>
Specifies whether trimming should be used and its weight. 一个还不错的数值是0.95。

-maxDepth <max_depth_of_weak_tree>
弱分类器树最大的深度。一个还不错的数值是1，是二叉树（stumps）。

-maxWeakCount <max_weak_tree_count>
每一级中的弱分类器的最大数目。The boosted classifier (stage) will have so many weak trees (<=maxWeakCount), as needed to achieve the given -maxFalseAlarmRate.
类Haar特征参数：


-mode <BASIC (default) | CORE | ALL>
选择训练过程中使用的Haar特征的类型。 BASIC 只使用右上特征， ALL 使用所有右上特征和45度旋转特征。





```bash
Author：Huang Jiaqi
Created: 2022-05-12
Last updated: 2022-05-16
Function：Target detection task for masks using the OpenCV-Haar model for the Masked-Face-Dataset dataset.
```
