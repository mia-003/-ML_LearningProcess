## 创建Web app文件结构
目前结构：

<img src="../assets/%E6%88%AA%E5%B1%8F2026-08-21%2003.39.56.png" alt="截屏2026-08-21 03.39.56" width="275">

创建web-app文件夹：

<img src="../assets/%E6%88%AA%E5%B1%8F2026-08-21%2003.43.02.png" alt="截屏2026-08-21 03.43.02" width="289">

创建文件：requirements.txt，写入需要的库
<img src="../assets/%E6%88%AA%E5%B1%8F2026-08-21%2003.46.41.png" alt="截屏2026-08-21 03.46.41" width="421">

从vscode终端进入web-app并安装依赖
```
cd /Users/zhouxiangyue/Documents/ML/web-app
pwd
pip install -r requirements.txt
```

在static/css下创建style.css文件、在templates下创建index.html文件、在web-app下创建app.py文件
<img src="../assets/%E6%88%AA%E5%B1%8F2026-08-21%2004.04.31.png" alt="截屏2026-08-21 04.04.31" width="343">
在vscode终端运行
```
python app.py
```
输出网址：http://127.0.0.1:5000
<img src="../assets/%E6%88%AA%E5%B1%8F2026-08-21%2004.07.00.png" alt="截屏2026-08-21 04.07.00" width="409">