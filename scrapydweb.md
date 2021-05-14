# ScrapydWeb项目搭建及使用

## 总体架构

*ScrapydWeb*基于[Scrapyd](https://github.com/scrapy/scrapyd)以及[Scrapyd-client](https://github.com/scrapy/scrapyd-client)实现scrapy项目的管理，并提供web ui方便用户管理。

- Scrapyd：运行scrapy项目，可使用HTTP JSON API控制爬虫

  - 例：启动某一个爬虫

    ```shell
    $ curl http://localhost:6800/schedule.json -d project=myproject -d spider=spider2
    {"status": "ok", "jobid": "26d1b1a6d6f111e0be5c001e648c57f8"}
    ```

- Scrapyd-client：将scrapy项目上传到运行*Scrapyd*的服务器需要先打包成`egg`文件，Scrapy-Client提供了相应的打包工具

整体架构如下：

- web主机：运行*ScrapydWeb*，管理抓取主机，控制爬虫的启动、停止、定时任务等，查看运行的结果。
- 抓取主机：运行爬虫的主机，运行*Scrapyd*，接收web主机的指令，返回任务状态及结果。

  <u>**web主机同时也可以是抓取主机**</u>

## Scrapyd安装和配置

### Scrapyd安装

```shell
pip install scrapyd
# 为了方便在web端查看抓取情况，还需要安装logparser
pip install logparser
```

### Scrapyd配置

在当前用户home目录下创建`.scrapyd.conf`

```
cd ~
vi .scrapyd.conf
```

加入以下配置，使其他主机能访问scrapyd，详细配置见[文档](https://scrapyd.readthedocs.io/en/stable/config.html#config-example)

```
[scrapyd]
bind_address = 0.0.0.0
```

### Scrapyd启动

```
scrapyd
```

若能访问当前主机`6800`端口则启动成功(可通过配置conf文件中的`http_port`设置其他端口号)



## ScrapydWeb安装与配置

### 安装

```shell
pip install scrapydweb
```

### 配置

#### 生成配置文件

```
scrapydweb
```

第一次使用命令，将自动在当前目录下生成默认配置文件`scrapydweb_settings_v10.py`

#### 修改配置文件

需要配置以下内容

| 配置项              | 内容                                               |
| ------------------- | -------------------------------------------------- |
| SCRAPYD_SERVERS     | 运行scrapyd服务的主机及端口                        |
| SCRAPYDWEB_PORT     | 网站的端口号，默认为5000                           |
| ENABLE_AUTH         | 是否需要密码进入网站，默认False                    |
| USERNAME            | 用户名                                             |
| PASSWORD            | 密码                                               |
| SCRAPY_PROJECTS_DIR | scrapy项目目录，将项目放到该目录下才能实现一键部署 |

***`SCRAPY_PROJECTS_DIR`若不存在需要手动建立***

ScrapydWeb默认使用sqlite存储任务信息到本地，可以通过设置环境`DATABASE_URL`存储到远程数据库。

```
export DATABASE_URL=''
# e.g.
# 'mysql://username:password@127.0.0.1:3306'	使用mysql需pip install pymysql
# 'postgres://username:password@127.0.0.1:5432'	使用postgres需pip install psycopg2
# 'sqlite:///C:/Users/username'
# 'sqlite:////home/username'
```

### 启用

```
scrapydweb
```

若`SCRAPYD_SERVERS`中配置的scrapyd主机信息正确，将能访问部署ScrapydWeb主机的5000端口或者配置的端口号。

**无法通过web界面修改配置，故每次修改配置文件后需重启ScarpydWeb生效**

## 访问 web UI
通过浏览器访问并登录 http://127.0.0.1:5000
* Servers 页面自动输出所有 Scrapyd server 的运行状态。
* 通过分组和过滤可以自由选择若干台 Scrapyd server，然后在上方 Tabs 标签页中选择 Scrapyd 提供的任一 [HTTP JSON API](https://scrapyd.readthedocs.io/en/latest/api.html)，实现**一次操作，批量执行**。

![servers](https://raw.githubusercontent.com/my8100/files/master/scrapydweb/screenshots/servers.png)

* 通过集成 [*LogParser*](https://github.com/my8100/logparser)，Jobs 页面自动输出爬虫任务的 pages 和 items 数据。
* *ScrapydWeb* 默认通过定时创建快照将爬虫任务列表信息保存到数据库，即使重启 Scrapyd server 也不会丢失任务信息。([issue 12](https://github.com/scrapy/scrapyd/issues/12))

![jobs](https://raw.githubusercontent.com/my8100/files/master/scrapydweb/screenshots/jobs.png)


## 部署项目
* 通过配置 `SCRAPY_PROJECTS_DIR` 指定 Scrapy 项目开发目录，*ScrapydWeb* 将自动列出该路径下的所有项目，默认选定最新编辑的项目，选择项目后即可**自动打包**和部署指定项目。
* 如果 *ScrapydWeb* 运行在远程服务器上，除了通过当前开发主机上传常规的 egg 文件，也可以将整个项目文件夹添加到 zip/tar/tar.gz 压缩文件后直接上传即可，无需手动打包为 egg 文件。
* **支持一键部署项目到 Scrapyd server 集群。**

![deploy](https://raw.githubusercontent.com/my8100/files/master/scrapydweb/screenshots/deploy.gif)


## 运行爬虫
* 通过下拉框依次选择 project，version 和 spider。
* 支持传入 Scrapy settings 和 spider arguments。
* 支持创建基于 [APScheduler](https://github.com/agronholm/apscheduler) 的定时爬虫任务。(如需同时启动大量爬虫任务，则需调整 Scrapyd 配置文件的 [max-proc](https://scrapyd.readthedocs.io/en/stable/config.html#max-proc) 参数)
* **支持在 Scrapyd server 集群上一键启动分布式爬虫。**

![run](https://raw.githubusercontent.com/my8100/files/master/scrapydweb/screenshots/run.gif)


## 日志分析和可视化
* **如果在同一台主机运行 Scrapyd 和 *ScrapydWeb*，建议设置 `SCRAPYD_LOGS_DIR` 和 `ENABLE_LOGPARSER`**，则启动 *ScrapydWeb* 时将自动运行 [*LogParser*](https://github.com/my8100/logparser)，该子进程通过定时增量式解析指定目录下的 Scrapy 日志文件以加快 Stats 页面的生成，避免因请求原始日志文件而占用大量内存和网络资源。
* **同理，如果需要管理 Scrapyd server 集群，建议在其余主机单独安装和启动 *LogParser***。
* 如果安装的 Scrapy 版本不大于 1.5.1，*LogParser* 将能够自动通过 Scrapy 内建的 [Telnet Console](https://scrapy.readthedocs.io/en/latest/topics/telnetconsole.html) 读取 Crawler.stats 和 Crawler.engine 数据，以便掌握 Scrapy 内部运行状态。

![stats](https://raw.githubusercontent.com/my8100/files/master/scrapydweb/screenshots/stats.gif)


## 定时爬虫任务
* 支持查看爬虫任务的参数信息，追溯历史记录
* 支持**暂停，恢复，触发，停止，编辑和删除**任务等操作

![tasks](https://raw.githubusercontent.com/my8100/files/master/scrapydweb/screenshots/tasks.gif)


## 邮件通知
通过轮询子进程在后台定时模拟访问 Stats 页面，*ScrapydWeb* **将在满足特定触发器时根据设定自动停止爬虫任务并发送通知邮件**，邮件正文包含当前爬虫任务的统计信息。

1. 添加邮箱帐号：
```python
SMTP_SERVER = 'smtp.qq.com'
SMTP_PORT = 465
SMTP_OVER_SSL = True
SMTP_CONNECTION_TIMEOUT = 10

EMAIL_USERNAME = ''  # defaults to FROM_ADDR
EMAIL_PASSWORD = 'password'
FROM_ADDR = 'username@qq.com'
TO_ADDRS = [FROM_ADDR]
```

2. 设置邮件工作时间和基本触发器，以下示例代表：每隔1小时或当某一任务完成时，并且当前时间是工作日的9点，12点和17点，*ScrapydWeb* 将会发送通知邮件。
```python
EMAIL_WORKING_DAYS = [1, 2, 3, 4, 5]
EMAIL_WORKING_HOURS = [9, 12, 17]
ON_JOB_RUNNING_INTERVAL = 3600
ON_JOB_FINISHED = True
```

3. 除了基本触发器，*ScrapydWeb* **还提供了多种触发器用于处理不同类型的 log**，包括 'CRITICAL', 'ERROR', 'WARNING', 'REDIRECT', 'RETRY' 和 'IGNORE'等。
```python
LOG_CRITICAL_THRESHOLD = 3
LOG_CRITICAL_TRIGGER_STOP = True
LOG_CRITICAL_TRIGGER_FORCESTOP = False
# ...
LOG_IGNORE_TRIGGER_FORCESTOP = False
```
以上示例代表：当日志中出现3条或以上的 critical 级别的 log 时，*ScrapydWeb* **将自动停止当前任务**，如果当前时间在邮件工作时间内，则同时发送通知邮件。

## 注意事项

### 不能使用`__file__`

打包后使用`__file__`会存在[问题](https://github.com/scrapy/scrapyd-client/issues/46)，可以替换为[pkgutil.get_data](http://docs.python.org/library/pkgutil.html#pkgutil.get_data)

```python
open(os.path.join(os.path.dirname(os.path.dirname(__file__)), 'urls/url.lst'), 'r', encoding='utf8')

# 可替换为
pkgutil.get_data("pachong", 'urls/url.lst').decode("utf-8")
```

### 不能使用静态文件(临时方案)

打包时默认只打包源文件，txt等文件将无法部署到爬虫服务器上。