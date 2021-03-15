## 天池人工智能创新赛赛道3-chizhu周星星分享

#### 大家好，我是来自重庆邮电大学的ch12hu团队的chizhu。很荣幸能够拿到第一周的周星星。
----

### 方案分享

本赛题是脱敏的文本匹配题，同2019年的[高校赛](https://github.com/chizhu/BDC2019)。
常用的思路如基于特征的feature+LGB或者基于深度语义模型的Esim和Bert等。本文采用后者。

>解题思路：pretrain+fine-tuning

具体方法：
* model

模型结构采用的是Nezha base 参考的是[nezha](https://github.com/lonePatient/NeZha_Chinese_PyTorch)
* pretraing

预训练参考transformers官方的预训练脚本[How to train a language model from scratch](https://github.com/huggingface/blog/blob/master/notebooks/01_how_to_train.ipynb)

预训练细节：预训练数据直接采用句子对形式训练，进一步地，将text1,text2的位置对调过来，数据增强。随机动态mask。加载原有的nezha base 中文模型，然后接着预训练，并不是从头开始的。
相当于保留ENcoder层的信息，重新学习下embedding层的信息。对比过从头开始的，收敛更快，效果上看，提升一个百分点(89->90)
预训练参数：lr=5e-5,epoch=300 ,loss~=0.3

* fine-tuning

直接采用[这里](https://github.com/lonePatient/NeZha_Chinese_PyTorch/blob/main/model/modeling_nezha.py)的NeZhaForSequenceClassificationz做二分类fientune。以及加入了[对抗训练](https://fyubang.com/2019/10/15/adversarial-train/)，采用fgm。
训练参数：bs=128,maxlen=32,epoch=5 ,5-folds。

#### 关键点
* 如何设计mask策略，如群里大佬提到的n-gram mask之类。
* 数据增强
* 显卡的香气，试错成本。



目前线上0.913的成绩是五折交叉的结果。cv ~=0.976

#### 最后
预祝大家上分愉快！！！




