# Sparkify
Udacity 项目：为Sparkify预测用户流失
使用环境：
python 3.6.8
spark v2.4.5
主要是用pyspark和sklearn

该项目是为了预测用户流失，提前挽留可能会流失的用户，可以避免损失。
评估了逻辑回归/GBT/SVM/随机森林，最终使用了随机森林分类器，最终的模型在测试集上的f1 score 是0.784，准确率是0.8

- Sparkify-zh.ipynb 包含数据探索，模型训练，模型调优等
- Sparkify-zh.html  Sparkify-zh.ipynb的html版本

博客地址：https://www.jianshu.com/p/984b1181c6d9