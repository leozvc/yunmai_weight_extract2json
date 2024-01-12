# [yunmai_weight_extract2json](https://github.com/arthurfsy2/yunmai_weight_extract2json/tree/main)

一个可以输入账号、密码即可获取云麦好轻数据的脚本。

# 步骤

1. clone 本项目到本地，并在当前路径下运行终端
2. 运行以下代码获取体重数据：

   需要在后面加入“你的手机号/密码/自定义的昵称/身高（米）”，如果有多组数据，则以逗号隔开

多个举例： `python getWeightData.py "186XXXXX123/12xxx4/nickname1/1.8,136XXXXX123/12XXX1/nickname2/1.7"`

单个举例：`python getWeightData.py "186XXXXX123/12xxx4/nickname1/1.8"`

如果账号、密码无误的话，即可在当前路径下生成“userinfo*自定义昵称.json”文件，里面包括 userId_real、refreshToken、account_b64、password_RSA，以供 getWeightData.py 使用。当 getWeightData.py 正常运行后，会生成 `weight*自定义昵称.json`，记录了该账号的体重信息。如果输入了多个账号的信息，则批量生成 weight\_自定义昵称.json 文件

# Github Action

如果你想通过 Github Action 来实现定时获取数据，可进行以下步骤

1. fork 本项目到你自己的仓库，然后修改 fork 仓库内的 `.github/workflows/run_data_sync.yml`文件，以下内容改为你自己的 github 信息。

```
env:
  GITHUB_NAME: arthurfsy2 （修改成你的github名称）
  GITHUB_EMAIL: fsyflh@gmail.com （修改为你的github账号邮箱）
```

    默认执行时间是每天10:00（北京时间）会自动执行脚本。

    如需修改时间，可修改以下代码的`cron`

```
on:
  workflow_dispatch:
  schedule:
    - cron: '0 2 * * *'
```

2. 为 GitHub Actions 添加代码提交权限 访问 repo Settings > Actions > General 页面，找到 Workflow permissions 的设置项，将选项配置为 Read and write permissions，支持 CI 将运动数据更新后提交到仓库中。
   **不设置允许的话，会导致 workflows 无法写入文件**
3. 在 repo Settings > Security > Secrets > secrets and variables > Actions > New repository secret > 增加:
   name 填写为：account，Secret 填写为：“你的手机号/密码/自定义的昵称”（不需要双引号），如 `186XXXXX123/12xxx4/nickname1`
   ![img](/img/添加变量.png)

# 使用方法

## 1.CDN

1、成功获取体重的 json 文件,r 如果希望国内网络流畅访问，且对数据更新没那么敏感的话，可考虑通过 CDN 加速一下，

### 推荐 1.jsdelivr

- 格式为：`https://cdn.jsdelivr.net/gh/你的账号名/你的仓库名@分支名称/文件名称`
  如本仓库 json 链接：`https://cdn.jsdelivr.net/gh/arthurfsy2/yunmai_weight_extract2json@master/weight_fsy.json`

### 推荐 2.gitmirror

可直接将 `raw.githubusercontent.com/XXX`换成 `raw.gitmirror.com/XXX`，即可实现免费 CDN

上述 2 种方法获取的 json 文件为直链，可以通过 python 或 javascript 直接 http 的 get 请求，直接获取到数据

## 2、可通过 echarts 表格引入该 json 文件画出图表。

详见：[获取云麦好轻体重数据并在 vuepress 上通过 echarts 折线图展示](https://blog.4a1801.life/%E7%BB%8F%E9%AA%8C%E6%80%BB%E7%BB%93/IT%E6%80%BB%E7%BB%93/%E8%8E%B7%E5%8F%96%E4%BA%91%E9%BA%A6%E5%A5%BD%E8%BD%BB%E6%95%B0%E6%8D%AE%E5%B9%B6%E5%9C%A8vuepress%E4%B8%8A%E5%B1%95%E7%A4%BA.html)
