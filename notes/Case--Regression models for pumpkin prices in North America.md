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
pumpkins = pumpkins.drop(columns=["Type","Variety",""])
```