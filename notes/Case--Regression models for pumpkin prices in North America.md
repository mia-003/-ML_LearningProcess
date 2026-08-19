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

查看相关系数。当月份作为变量时，相关系数为-0.034. 
`print(pumpkins['month'].corr(pumpkins['avg_price']))`
当day作为变量时，相关系数为-0.046
`print(pumpkins['month'].corr(pumpkins['avg_price']))`

找另外一个变量Variety，假设和Variety有相关性。
```
%% 在价格散点图上区分不同Variety的颜色，初步观察 %%
ax = None # 画布
colors = ["red", "blue", "green", "yellow"]
%% 遍历每个颜色、每个Variety %%
for i, var in enumerate(pumpkins["Variety"].unique()):
    variety=pumpkins[pumpkins["Variety"]==var]
    ax = variety.plot.scatter(x="month", y="avg_price", color=colors[i], label=var, ax=ax) # ax=ax：每次遍历都画在同一个轴
```