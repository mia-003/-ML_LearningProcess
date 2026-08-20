文件路径：
/Users/zhouxiangyue/Documents/ML/ML_DataSet/web_app_practice/ufo_sightings/ufos.csv

模型任务：
输入：持续时间 + 纬度 + 经度
输出：国家（预测类别）

### 保存模型 (Pickle)
```
import pickle
# 使用pickle保存模型
model_path=("/Users/zhouxiangyue/Documents/ML/ufo-model.pkl")
pickle.dump(
    ufos_model,
    open(model_path, "wb")
) 
# 把（已学习的模型变量）ufos_model保存到路径下的"ufo-model.pkl"的模型文件。.dump()保存，"wb"以二进制写入
```

### 加载模型 (Pickle)
```
ufos_model = pickle.load(
    open(model_path, "rb")
)
# "rb"以二进制读取
```

### 预测
```
ufos_model.predict([[50, 44, -12]]) # 二维
```

### 制作Flask网页
Flask是Python web框架
- 启动本地网站（首页html）
- 接收网页表单数据（用户输入）
- 调用模型并返回预测结果（返回带有结果的html）
```
%% 创建flask应用 %%
from flask import Flask
app = Flask(__name__)

@app.route("/")
def open_homepage():
return render_template("index.html")
```