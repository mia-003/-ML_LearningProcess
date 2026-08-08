---
tags:
  - 学习
  - ML
date: 2026-07-27
---
GitHub项目学习[[# https://github.com/DataTalksClub/machine-learning-zoomcamp]]
### sklearn库 
"Scikit-learn is an open source machine learning library that supports supervised and unsupervised learning. It also provides various tools for model fitting, data preprocessing, model selection and evaluation, and many other utilities."
```
from sklearn.datasets improt load_diabetes %% 导入数据集 %%
from sklearn.model_selection import train_test_split %% 导入训练测试工具 %%
X, y=load_diabetes(return_X_y=True)%% 函数完成解包赋值，对X赋为特征矩阵、y赋为目标变量target %%
```
### 回归regression 
#### linear: y=a+bx
#### polynomial: y=ax+bx+cx^2
#### logistic: y=1/(1+e^-(a+bx))


### 模型 web应用
### 分类classification
### 聚类clustering
### NLP
### 时间序列timeseries
### 强化学习reinforcement
