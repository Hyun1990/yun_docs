Evaluation
==========

TP, FP, TN, FN
---------------

  .. image:: ./images_eval/tp.png
        :align: center
        :width: 500


假设上图是一个二分类的模型训练结果及Ground True显示图，共1000张图片：

绿色椭圆形
:math:`(TP+FN)` 表示Ground Truth为马，300张

黄色椭圆形
:math:`(TP+FP)` 表示模型识别出的马，400张

两个椭圆形交叉部分
:math:`TP` 表示模型识别出的马且Ground Truth也为马，250张

椭圆形外区域
:math:`TN` 表示未识别出马且Ground True也不为马，550张


**Presion(准确率)** :
:math:`\frac{TP}{TP + FP} = 250/400 = 62.5\%`

**Recall(查全率)** :
:math:`\frac{TP}{TP+FP} = 250/300 = 83.3\%`

**漏检率** = 1 - 查全率

**误检率** = 1 - 准确率


参考文档
--------
    .. [1] `目标检测、图像分类中的模型效果指标precision与recall的本质探索 <https://www.pianshen.com/article/8141305867/>`_
