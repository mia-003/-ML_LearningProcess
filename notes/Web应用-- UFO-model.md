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
浏览器向服务器发出请求的的两种方法“GET”、“POST”，分别为“获取内容”“提交数据处理”
```
%% 创建flask应用 %%
from flask import Flask, render_template, request

# 创建储存html的文件夹templates
app = Flask(__name__, template_folder="templates")

%% 当用户打开"/"，执行“打开首页”函数，使用render_template对象在templates文件夹下找到html文件并返回 %%
@app.route("/", methods=["GET"]
def open_homepage():
	return render_template("index.html")
%% 当用户打开"/predict"，执行“返回结果”函数，使用request对象储存用户输入的x，调用模型，返回y %%	
# request对象用于保存浏览器返回给服务器的数据
@app.route("/predict", methods=["POST"])
def return_result():
	features = [
		float(x) # 把用户提交的str转为float
		for x in request.form.values()]
	final_features = [np.array(features)]
	pred_result = ufos_model.predict(final_features)
	return render_template(
		"index.html",
		prediction_text=f"Likely country:{pred_result[0]}")

```

调用模型代码（已训练）：
```
import pandas as pd
import numpy as np
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import accuracy_score, classification_report
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import LabelEncoder
ufos = pd.read_csv("/Users/zhouxiangyue/Documents/ML/ML_DataSet/web_app_practice/ufo_sightings/ufos.csv")
# ufos.head(10)
# 2. 只保留本项目需要的四列，并改成更清楚的列名
ufos = ufos[
    ["duration (seconds)", "country", "latitude", "longitude"]
].copy()
ufos.columns = ["Seconds", "Country", "Latitude", "Longitude"]


# 3. 确保三个特征都是数字；无法转换的内容会变成缺失值 NaN
numeric_columns = ["Seconds", "Latitude", "Longitude"]
ufos[numeric_columns] = ufos[numeric_columns].apply(
    pd.to_numeric,
    errors="coerce",
)

# 删除四个字段中存在缺失值的行，并只保留持续 1～60 秒的记录
ufos.dropna(inplace=True)
ufos = ufos[ufos["Seconds"].between(1, 60)].copy()

# 4. 把国家名称编码成模型可以处理的整数类别
label_encoder = LabelEncoder()
ufos["Country"] = label_encoder.fit_transform(ufos["Country"])

country_code_map = {
    country: int(code)
    for code, country in enumerate(label_encoder.classes_)
}

# 5. X 是输入特征，y 是模型要预测的国家类别
X = ufos[["Seconds", "Latitude", "Longitude"]]
y = ufos["Country"]

X_train, X_test, y_train, y_test = train_test_split(
    X,
    y,
    test_size=0.2,
    random_state=0,
    stratify=y,
)

# 6. 训练逻辑回归分类模型
ufos_model = LogisticRegression(max_iter=1000)
ufos_model.fit(X_train, y_train)


# 7. 使用从未参与训练的测试集评估模型
y_pred = ufos_model.predict(X_test)

print(f"清洗后的数据量：{len(ufos):,} 行")
print(f"训练集：{len(X_train):,} 行")
print(f"测试集：{len(X_test):,} 行")
print(f"国家编码：{country_code_map}")
print(f"准确率：{accuracy_score(y_test, y_pred):.2%}")
print("\n分类报告：")
print(
    classification_report(
        y_test,
        y_pred,
        labels=range(len(label_encoder.classes_)),
        target_names=label_encoder.classes_,
        zero_division=0,
    )
)```