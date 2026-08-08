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
%% 导入数据集 %%
from sklearn.datasets improt load_diabetes 
%% 导入训练测试工具 %%
from sklearn.model_selection import train_test_split
%% 函数完成解包赋值，对X赋为特征矩阵features、y赋为目标变量target %%
X, y=load_diabetes(return_X_y=True)
```
#### 查看原始数据集
```
diabetes_raw=load_diavetes(as_frame=True %% X为DataFrame格式 %%, scaled=False %% 不进行标准化处理 %%) 
diabetes_raw.frame.head(10) %% 查看原始数据集前10行 %%
```
#### 模型调用
#### 模型评估
```
%% 导入MSE（jun fang） %%
```
#### 绘制结果图 Plotting
### 回归 Regression 
#### 线性 Linear: y=a+bx
```
from sklearn.linear_model import LinearRegression
redressor=LinearRegression().fit(X_train, y_train)
```
#### logistic: y=1/(1+e^-(a+bx))


### 模型 web应用
### 分类classification
### 聚类clustering
### NLP
### 时间序列timeseries
### 强化学习reinforcement
