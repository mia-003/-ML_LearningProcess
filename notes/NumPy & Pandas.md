
| Numpy                                                                                        | Pandas                                                                                                                                                    |
| -------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 相同数据类型，多用于数值计算                                                                               | 可不统一数据类型，多用于查看、预处理                                                                                                                                        |
| Array（多维数组）                                                                                  | DataFrame（二维表格）<br>Series（一维）                                                                                                                             |
| 加载数据集默认格式                                                                                    | 加载数据集需要 `as_frame=True`                                                                                                                                   |
| 查看数据集 行列切片<br>`X[1,3]` 取第2行第4列的某个数<br>`X[:2, 1:4]` 取第1～第2行、第2～第4列<br>`y[:10]/y[0:10]` 查看前10行 | 查看数据集 loc表示实际，iloc表示索引<br>`X.loc[1:3,3:5]`等价于`X.iloc[0:2,2:3]` 取第1～2行、第3～第4列<br>`X.iloc[2]` 取第3行<br>`X.iloc[:2]` 取前3行<br>`X.head(10)/X.iloc[:10]`  查看前10行 |
| 不支持按列名选择                                                                                     | 带索引、列名，支持按列名选择<br>eg. 当特征向量有多维时，`X[["BMI"]]`<br>当特征向量一维时，`X["BMI"]`                                                                                       |
| matplot画图<br>`plt.scatter(X_train.to_numpy()[:,0], y_train.to_numpy()[:,0]`                  | 画图（更适合一维Series）`plt.scatter(X.iloc[:,0],y.iloc[:,0])`                                                                                                     |
==pred_y=model.fit(X_test) 返回Numpy数组==
转换方法：
`X_array = X.to_numpy()`
`X_df = pd.DataFrame(`
    `X_array,`
    `columns=["Weight", "Height", "BMI"])`
    
#### 机器学习流程中：
CSV/数据集
   ↓
Pandas：清洗、选列、处理缺失值
   ↓
sklearn：训练模型
   ↓
NumPy：预测值、系数、评估计算

eg. 分别用DataFrame和NumPy处理训练集和测试集
```
%% 训练集 用numpy %%
from sklearn.datasets import load_linnerud
from sklearn.linear_model import LinearRegression
from sklearn.model_selection import train_test_split
import numpy as np
import pandas as pd
# 解析特征值和目标值
X, y = load_linnerud(return_X_y=True,as_frame=True) %% df格式 %%
print(X.head(10))
print(y.head(10))
X_train, X_test, y_train, y_test = train_test_split(X.iloc[:, 0], y.iloc[:, 0], test_size=0.3, random_state=42)
X_train = X_train.to_numpy().reshape(-1, 1)
y_train = y_train.to_numpy()
# 在查看数据集时可知X和y分别有三个变量，共六个。所以取变量时需要限定。
model = LinearRegression().fit(X_train, y_train)
pred_y_train = model.predict(X_train)

import matplotlib.pyplot as plt
plt.scatter(X_train, y_train, color="black", label="Train")
plt.plot(X_train, pred_y_train, color="pink", linewidth=2)
plt.xlabel("Features")
plt.ylabel("Targets")
plt.title("Linear Regression: Actual vs Predicted")
plt.legend()
plt.show()
```
``

```
%%  %%
plt.scatter(X_test.iloc[test_order, 0], y_test.iloc[test_order, 0], color="blue", label="Test")
plt.plot(X_test.iloc[test_order, 0], pred_y_test.iloc[test_order, 0], color="green", linewidth=2)
plt.xlabel("Features")
plt.ylabel("Targets")
plt.title("Linear Regression: Actual vs Predicted")
plt.legend()
plt.show()
```