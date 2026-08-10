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
```
%% 以线性回归为例 %%
from sklearn.linear_model import LinearRegression
%% 训练集输入 %%
regressor=LinearRegression().fit(X_train, y_train)
%% 调用模型输出预测集 %%

```
#### 模型评估
```
%% 导入MSE（均方误差）和R^2系数用于评估模型效果 %%
from sklearn.metrics import mean_squared_error, r2_score
print(mean_squared_error(y_test, y_pred))
print(r2_score(y_test, y_pred))
```
#### 绘制结果图 Plotting
```
import matplotlib.pyplot as plt
%% 创建两张子图在同行，共享x、y轴 %%
fig, ax=plt.subplot(ncols=2, figsize=(10,5), sharex=True, sharey=True)
%% ax[0]为第一张图，散点图展示训练集；ax[1]为第二张图，展示测试集 %%
ax[0].scatter(X_train, y_train, label="Train data points")
ax[1].scatter(X_test, y_test, label="Test data points")
ax[0].plot(
    X_train,
    regressor.predict(X_train),
    linewidth=3,
    color="tab:orange",
    label="Model predictions",)

```
### 回归 Regression 
#### 线性 Linear: y=a+bx
```
from sklearn.linear_model import LinearRegression
redressor=LinearRegression().fit(X_train, y_train)
```
#### Logistic: y=1/(1+e^-(a+bx))
#### 回归常用的评估指标
- MAE 平均绝对误差：mean(｜pred-test｜)
```
from sklearn.metrics import mean_absolute_error
MAE = mean_absolute_error(y_test, y_pred)
```
- MSE 平均均方误差：mean((pred-test)^2)
	越小越好，最佳值为0，对极端值敏感
```
from sklearn.metrics import mean_squared_error
MAE = mean_squared_error(y_test, y_pred)
```
- RMSE 均方根误差：RMSE = √MSE =√mean((pred-test)^2)
- R^2 Coefficient of determination：决定系数
	越接近1越表示“能解释目标变量的变化”
```
from sklearn.metrics import r2_score
r2 = r2_score(y_test, y_pred)
```
### 模型 web应用
### 分类classification
### 聚类clustering
### NLP
### 时间序列timeseries
### 强化学习reinforcement
