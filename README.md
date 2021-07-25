# 企业微信推送热搜

>  感谢大佬 [WangNingKai](https://github.com/WangNingkai/ServerChan-Push) 的开源代码，此企业微信接入也是基于大佬的源码修改而成。

<br />

### 示例

clone 此 GitHub 仓库，修改.github/workflows/ 文件夹下一个 main.yml 文件，内容如下：

```yml
name: 'Qywx Push News'

on:
  push:
    branches:
      - master
  schedule:
    - cron: '0 */3 * * *' # 定义 cron 表达式
  watch:
    types: [started] # 定义star是自动发送

env:
  TZ: Asia/Shanghai

jobs:
  Gitfolio-Spider:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v2
      - name: 'Set up Python'
        uses: actions/setup-python@v1
        with:
          python-version: 3.8
      - name: 'Install dependencies'
        run: python -m pip install --upgrade pip
      - name: 'Install requirements'
        run: pip install -r ./requirements.txt
      - name: 'Working'
        env:
          SECRET: ${{ secrets.SECRET }}
          CORPID: ${{ secrets.CORPID }}
          AGENTID: ${{ secrets.AGENTID }}
          THUMBMEDIAID: ${{ secrets.THUMBMEDIAID }}
          CORPSECRET: ${{ secrets.CORPSECRET }}
        timeout-minutes: 350
        run: |
          echo SECRET=$SECRET > .env
          echo CORPID=$CORPID >> .env
          echo AGENTID=$AGENTID >> .env
          echo THUMBMEDIAID=$THUMBMEDIAID >> .env
          echo CORPSECRET=$CORPSECRET >> .env
          python main.py
          rm -f .env

```

**注意**

- cron 是 UTC 时间，使用时请将北京时间转换为 UTC 进行配置。
- 请在项目的 Settings -> Secrets 路径下配置好 CORPID、AGENTID、THUMBMEDIAID、CORPSECRET，不要直接在 .yml 文件中暴露地址跟密钥
- CORPID 等设置均为 `xxxxxx`
- SECRET ServerChan 需要时配置

<br />

> 以下为旧版内容

## Serve酱自动推送热门

> 🔝 通过Serve酱自动发送微博、知乎、v2ex热门内容到微信，可配置 workflow 的触发条件为 schedule，实现周期性定时发送热门内容。

### 示例

clone 此 GitHub 仓库，修改.github/workflows/ 文件夹下一个 main.yml 文件，内容如下：

```yml
name: 'Push News'

on:
  push:
    branches:
      - master
  schedule:
    - cron: '0 */3 * * *' # 定义 cron 表达式
  watch:
    types: [started] # 定义star是自动发送

env:
  TZ: Asia/Shanghai

jobs:
  Gitfolio-Spider:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v2
      - name: 'Set up Python'
        uses: actions/setup-python@v1
        with:
          python-version: 3.8
      - name: 'Install dependencies'
        run: python -m pip install --upgrade pip
      - name: 'Install requirements'
        run: pip install -r ./requirements.txt
      - name: 'Working'
        env:
          SECRET: ${{ secrets.SECRET }}
        timeout-minutes: 350
        run: |
          echo SECRET=$SECRET > .env
          python main.py
          rm -f .env

```

**注意**

- cron 是 UTC 时间，使用时请将北京时间转换为 UTC 进行配置。
- 请在项目的 Settings -> Secrets 路径下配置好SECRET(server酱密钥)，不要直接在 .yml 文件中暴露地址跟密钥
- SECRET 设置为 `xxxxxx`
