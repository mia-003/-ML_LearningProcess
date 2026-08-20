---
tags:
  - 学习
  - ML
date: 2026-07-27
---
GitHub项目学习[https://github.com/DataTalksClub/machine-learning-zoomcamp](https://github.com/DataTalksClub/machine-learning-zoomcamp)

### NumPy(Array) & Pandas(DataFrame)
- 查看[NumPy & Pandas](notes/NumPy%20%26%20Pandas.md)
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
import pandas as pd

diabetes_raw=load_diavetes(as_frame=True %% X为DataFrame格式 %%, scaled=False %% 不进行标准化处理 %%) 
diabetes_raw.frame.head(10) %% 查看原始数据集前10行,.head()是由pandas的DataFrame提供的方法;否则输出numpy数组形式 %%
```
#### 划分训练集和测试集
```
%% 按比例（0～1的小数）默认打乱则需要写随机种子数，不打乱则shuffle=False %%
X_train, X_test, y_train, y_test = model_selection.train_test_split(X,y,test_size=0.33,random_state=42)

%% 按实际数量 %%
X_train, X_test, y_train, y_test = model_selection.train_test_split(X,y,test_size=50,random_state=42)
```
#### 模型调用
```
%% 以线性回归为例 %%
from sklearn.linear_model import LinearRegression 
%% 训练集输入 %%
model=LinearRegression().fit(X_train, y_train)
%% 调用模型输出预测集 %%
y_pred=model.predict(X_test)

%% 或者 %%
from sklearn import linear_model
model = linear_model.LinearRegression()
model.fit(X_train, y_train)
```
#### 模型评估
```
%% 导入MSE（均方误差）和R^2系数用于评估模型效果 %%
from sklearn.metrics import mean_squared_error, r2_score
print(mean_squared_error(y_test, y_pred))
print(r2_score(y_test, y_pred))

%% 分类用F1和混淆矩阵指标评估模型效果 %%
from sklearn.metrics import f1_score, classification_report
print(classification_report(y_test, predictions))
print('F1-score: ', f1_score(y_test, predictions))
```
#### 绘制结果图 Plotting
```
import numpy
%% 对特征值排序，方便绘图 %%
order = np.argsorx(X_train[:,0])
```
```
import matplotlib.pyplot as plt
%% 创建两张子图在同行，共享x、y轴 %%
fig, ax=plt.subplot(ncols=2, figsize=(10,5), sharex=True, sharey=True)

%% ax[0]为第一张图，散点图展示训练集 %%
ax[0].scatter(X_train[order], y_train, label="Train data points")
%% ax[0]的直线绘制预测线（用于观察拟合情况） %%
ax[0].plot(
    X_train[order], %% 横坐标 %%
    model.predict(X_train), %% 纵坐标 %%
    linewidth=3,
    color="tab:orange",
    label="Model predictions")
    
%% ax[1]为第二张图，展示测试集 %%
ax[1].scatter(X_test, y_test, label="Test data points")
%% ax[1]的直线展示测试集的预测线 %%
ax[1].plot(X_test, y_pred, 
	linewidth=3,
	color="tab:orange",
	label="Model predictions")
plt.show()
```
### 回归 Regression 
* 线性回归 Linear: y=a+bx
* 逻辑回归 Logistic: y=1/(1+e^-(a+bx))
* 多项式回归 Polynomial：多个变量，解释非线性关系
#### Case Study（预测北美南瓜价格）
[Case--Regression models for pumpkin prices in North America](notes/Case--Regression%20models%20for%20pumpkin%20prices%20in%20North%20America.md)
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
[Web应用-- UFO-model](notes/Web%E5%BA%94%E7%94%A8--%20UFO-model.md)
- 使用Pickle保存和加载模型
- 使用Flask
### 分类classification
### 聚类clustering
### NLP
### 时间序列timeseries
### 强化学习reinforcement
