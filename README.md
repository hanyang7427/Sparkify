# 为Sparkify预测用户流失
Udacity 项目：为Sparkify预测用户流失

### 项目动机
Sparkify是一家数字音乐服务的公司，大量用户每天使用该服务听自己喜欢的歌曲，他们可能是带广告的免费用户，或是使用尊贵会员的用户，会员听歌免费但是需要按月支付一定费用，用户可以升级降级，注销其账号。如果用户在平台体验不好，或需求没有得到满足，他们可能会流失(churn)，要在用户流失之前留住他们。
数据集有mini和large，次项目用的是mini数据集，large数据集地址：s3n://udacity-dsnd/sparkify/sparkify_event_data.json

### 使用环境：
- python 3.6.8
- spark v2.4.5

### 主要使用的库：
- 绘图：matplotlib, seaborn
- 数据处理：pandas, pyspark

### 目录结构：
- Sparkify-zh.ipynb 主要数据分析代码文件
- Sparkify-zh.html  Sparkify-zh.ipynb的html版本

### 使用方法
```
git clone git@github.com:hanyang7427/Sparkify.git
jupyter notebook Sparkify-zh.ipynb	
```

### 步骤
我使用了mini数据集，详细步骤在博客：https://www.jianshu.com/p/984b1181c6d9

#### 数据清洗
数据的缺失值比率，如下
![alt text](https://github.com/hanyang7427/Sparkify/blob/master/img/null_ratio.jpg)


当用户在没有听任何歌曲的状态时，artist和length和song是null，由于后期null的数量作为了特征，这不分没有处理。另外一方面，用户如果是游客，不会存在流失问题，所以将游客用户删除。

#### 数据探索
数据中有225个用户的数据，其中有23.11%的用户流失。
![alt text](https://github.com/hanyang7427/Sparkify/blob/master/img/churn_ratio.jpg)

标签是偏态的。

![alt text](https://github.com/hanyang7427/Sparkify/blob/master/img/churn_dist.jpg)

#### 特征提取
提取了9个特征，如下

![alt text](https://github.com/hanyang7427/Sparkify/blob/master/img/feature_head.jpg)

每个特征的分布，如下

![alt text](https://github.com/hanyang7427/Sparkify/blob/master/img/feature_dist.jpg)

#### 特征预处理
特征预处理使用了pipeline，对于分类变量使用了stringIndexer创建了分类索引，然后使用VercorAssember将特征向量化，最后使用StandardScaler将特征进行缩放。

#### 模型和评估指标
我使用了F1 score评估模型，评估了逻辑回归，GBT，SVM，随机森林。最后选择了在验证集的得分最高的随机森林进行调优，最终的随机森林在测试集上的F1得分为0.784，特征重要性显示用户的注册时长对用户是否会流失预测有很重要。如下
![alt text](https://github.com/hanyang7427/Sparkify/blob/master/img/feature_importance.jpg)


#### 结论
这个notebook在128M的数据上训练了用于预测用户是否会churn的模型，评估了四个模型：逻辑回归，GBT，支持向量机，随机森林，通过对比F1 socre选择了随机森林并在其上调整参数，使用最优模型在测试集上得到的F1 score 是0.784，但是测试集只有15个样本，所以在测试集上的到的得分有较大偏差。最后列出随机森林给出的对于用户是否会churn的最重要特征是用户注册了多长时间(hour_since_reg)。此notebook使用的是数据的子集，这样才能在单台机器上运行，如果用完整数据(12G)，对于现有机器性能已经是大数据，需要在集群环境运行。数据有只225个用户样本，所以模型实际意义并不大，如果有更多数据，可以将本notebook拓展到spark环境，将对模型表现有很大程度的提升。


