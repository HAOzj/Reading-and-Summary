# 背景
在推荐系统中，我们往往用隐式的反馈（比如点击，喜欢）等行为来作为模型的训练数据来学习物品的效用。
但是有研究表明，物品效用和隐式反馈之间存在gap。用隐式反馈数据来训练会使得模型倾向于当前的系统。
这种倾向性叫做selection bias。Selection bias的一个重要因素就是物品展示的位置。
一般来说，位置越靠前，引起用户注意的概率越大，点击概率也相应变大，这个称为位置偏置。
理解和解决位置偏置问题，有助于提高推荐的效果。  

目前业界有以下几种考虑位置偏置的方式：
- 位置域方法 
- Shallow Tower
- PAL

## 位置域方法
训练时将位置作为特征域，infer时用默认位置来计算预估分数。

这个方法
- 优点是和slot embedding+nn的范式匹配；
- 缺点是线上infer的分数和展示时的分数不一致,因为infer时位置域用的是的fault，而展示时对应的是真实的位置。
使用不同的默认位置可能会导致不同的推荐结果。

## Shallow Tower
Youtube在《Recommending What Video to Watch Next》中提出的，
把position bias等selection bias单独用一个浅的塔来学习propensity score，
和user utilities在logits层次上结合



## PAL  
类似于shallow tower，华为在2019年的[论文](https://www.researchgate.net/publication/335771749_PAL_a_position-bias_aware_learning_framework_for_CTR_prediction_in_live_recommender_systems)，用于解决推荐和广告中的位置偏置问题。
训练时在预估分数之外独立学习位置被看到的分数，用被看到的分数乘以预估分数作为最终输出，infer时只预估分数

模型基于这样的假设：物品被看到和看到后被点击的概率是独立的。所以在训练时可以用不同位置被看到的概率来矫正看到后被点击的概率。
但这个假设过于强，比如在搜索推荐中，部分用户的认知是越靠前越相关。