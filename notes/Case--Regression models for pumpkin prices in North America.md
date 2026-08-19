数据集位置：
/Users/zhouxiangyue/Documents/ML/ML_DataSet/regression_practice/pumpkin_market/US-pumpkins.csv

```
%% 加载数据集 %%
import pandas as pd
path = "/Users/zhouxiangyue/Documents/ML/ML_DataSet/regression_practice/pumpkin_market/US-pumpkins.csv"
pumpkins = pd.read_csv(path)
pumpkins.head()
print(pumpkins.shape)
```

```
%% 预处理：删除空列和不需要的列 %%
# dropna()删除空行(axis=0)或空列(axis=1)，how="all"整行/列都为空时，"any"任一格为空时
pumpkins = pumpkins.dropna(axis=1, how="all")
# 列名找不到时忽略，不报错
pumpkins = pumpkins.drop(columns=["Type","Origin District","Unnamed: 25"], errors="ignore")
```

Q：预测某个月卖南瓜的价格
所需列：Date、Low Price、High Price、Package
- Step1：选定所需列
- Step2：处理日期（转日期格式、抽出月份）
```
%% 创建独立变量 %%
month=pd.DatetimeFrameIndex(pumpkins['Date']).month
avg_price=(pumpkins['Low Price']+pumpkins['High Price'])/2
# 也可以在数据集中创建列 pumpkins['month']=month
```
- Step3：统一单位，取平均价格
```
# Package列中有各种包装类型（each/per bin/bushel），需要统一
%% 查看所有值 %%
print(pumpkins['Package'].unique())

%% 只取Bushel单位的行 %%
pumpkins=pumpkins[pumpkins['Package'].str.contains('bushel', case=True)] # 忽略大小写

%% 计算每个bushel的平均价格 %%
pumpkins.loc[pumpkins['Package'].str.contains('1 1/9'), 'Price'] = price/(1 + 1/9)
pumpkins.loc[pumpkins['Package'].str.contains('1/2'), 'Price'] = price/(1/2)
```

- Step4：绘制所有销售价格散点图、平均价格by month柱状图
```
import matplotlib.pyplot as plt

plt.scatter(month, avg_price)
avg_price.groupby(month).mean().plot(kind='bar') # 根据month分组取mean值
plt.show()
# 或者使用Seaborn库，把两个独立变量传给x和y：
import seaborn as sns
sns.catplot(x=month, y=price, kind="bar")
```

查看相关系数。当月份作为变量时，相关系数为-0.14. 
`print(pumpkins['month'].corr(pumpkins['avg_price']))`
当day作为变量时，相关系数为-0.16，略微提升。
```
%% pd.to_datetime()转日期格式；.dt访问日期；dayofyear一年中的第几天 %%
pumpkins["day_of_year"]=pd.to_datetime(pumpkins["Date"]).dt.dayofyear
```

```
%% 在价格散点图上区分不同Variety的颜色，初步观察 %%
ax = None # 画布
colors = ["red", "blue", "green", "yellow"]
%% 遍历每个颜色、每个Variety %%
for i, var in enumerate(pumpkins["Variety"].unique()):
    variety=pumpkins[pumpkins["Variety"]==var]
    ax = variety.plot.scatter(x="day_of_year", y="avg_price", color=colors[i], label=var, ax=ax) # ax=ax：每次遍历都画在同一个轴
```
408
发现不同Variety的价格相差更大，PIE TYPE整体价格比MINIATURE都低。所以假设价格和Variety有相关性。
## 线性回归
```
%% 调用线性模型 %%
X=pumpkins[["day_of_year"]].to_numpy().reshape(-1,1) # 转二维,转二维需要用np的reshape方法,所以要先转np
y=pumpkins["avg_price"] #可以保持一维
X_train, X_test, y_train, y_test = train_test_split(X,y, test_size=0.2, random_state=42)
line_reg_model=LinearRegression()
line_reg_model.fit(X_train, y_train)
pred_y=line_reg_model.predict(X_test)
%% 评估模型 %%
MAE = mean_absolute_error(y_test, pred_y)
MSE=mean_squared_error(y_test, pred_y)
RMSE=np.sqrt(MSE)
R2=r2_score(y_test, pred_y)
print("MAE:", f"{MAE:3f}")
print("MSE:", f"{MSE:3f}")
print("RMSE:", f"{RMSE:3f}")
print("R2:", f"{R2:3f}")
```
结果为：MAE=9.104742； MSE=108.258397； RMSE=10.404730； R2=0.002633
MAE说明平均预测价格和真实价格相差约$9，R2接近0说明模型几乎没有学习到线形特征
## 多项式回归
```
%% 调用多项式模型 %%
from sklearn.preprocessing import PolynomialFeatures
from sklearn.pipeline import make_pipeline
pipeline = make_pipeline(PolynomialFeatures(degree=2), LinearRegression()) # 最高输入二次项。假设2个feature则1,x1,x2,x1^2,x1x2,x2^2。
pipeline.fit(X_train,y_train)
pred_y=pipeline.predict(X_test)
```
结果为：MAE=8.86；R2=0.036
略有提升，但仍不显著。

非数值型变量特征Categorical Features参与多项式回归模型
- One-hot encoding：把category转换为1/0编码
```
X = pd.get_dummies(pumpkins['Variety'])
y = pumpkins['Price']
%% 或者组合多个特征 %%
X = pd.get_dummies(pumpkins['Variety'])
	.join(pd.get_dummies(pumpkins['City Name'])) # 分类变量则get_dummies()
	.join(pumpkins['day_of_year']) # 数值变量则直接join
```
结果为：MAE=1.79；R2=0.93，显著提升。

## 逻辑回归（用于预测分类）
```
%% 选择所需列 %%
pumpkins = pumpkins.loc[:, ["Origin", "Variety", "City Name", "Item Size", "Color", "Package"]]
# 观察每个品种、每个颜色的分布。分组1:Variety；分组2:Color
sns.catplot(
    data=pumpkins,
    y="Variety",
    hue="Color", # 图例
    kind="count",)
```

数据预处理
```
%% 编码有自然顺序的非数值变量 %%
# 查看所有值
print(pumpkins["Item Size"].unique())
# 去除空行。inplace=True表示直接修改数据集
pumpkins.dropna(subset=["Item Size"], inplace=True) 

from sklearn.preprocessing import OrdinalEncoder
# 定义顺序
item_size_categories=[['sml','med','med-lge','lge','xlge','jbo','exjbo']]
# dai chu li
```