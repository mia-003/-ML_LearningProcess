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
Step1：选定所需列
Step2：处理日期（转日期格式、抽出月份）
Step3：统一单位，取平均价格

```
# Package列中有各种包装类型（each/per bin/bushel），
%% 只取Bushel单位的行 %%
pumpkins=pumpkins[pumpkins['Package'].str.contains('bushel', case=True, regex=True)]
```