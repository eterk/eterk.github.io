## scrapy

生命周期

![1574675459389](C:\Users\Summers\AppData\Roaming\Typora\typora-user-images\1574675459389.png)

``` shell
# 安装scrapy
pip install scrapy
#自动创建 scrapy project
scrapy start {project_name}
# 展示scrapy 项目里的 spider 必须在project 项目里才能用
scrapy list 
#edit scraoy.cfg
```

在项目写好后，可以进行部署，

使用scrapyd（服务端）

```shell
# 下载服务端
install scrapyd
# 下载客户端
install scrapyd-client

# 启动客户端
scrapyd
#到项目目录下，必须要有cfg 文件的目录
cd project_dir
#在 windows 下不能直接使用scrapyd-deploy 命令
scrapyd-deploy 部署名称 -p 项目名称
# 部署名称在scrapy.cfg 文件中修改 [deploy:部署名称]， 修改cfg  ,打开部署端口

# 使用scrapyd 启动服务
curl http://localhost:6800/schedule.json -d project=项目名称 -d spider=爬虫名称

# 使用spiderkeeper 启动服务
spiderkeeper 启动
按提示创建项目，项目名称需要和真实项目名一致，按照提示用 scrapy-deploy 生成egg 文件并上传

scrapyd-deploy --build-egg output.egg
 # 在pycharm 中启动 spider
  main.py
  爬虫入口
  from scrapy.cmdline import execute
  # -a 是args 的意思，arg 必须以键值对的形式出现
  execute(['scrapy', 'crawl', 'spider_name', "-a", "word_len=1"])
```

![1574754767677](C:\Users\Summers\AppData\Roaming\Typora\typora-user-images\1574754767677.png)



### 在 windows 部署scrapyd-deploy 命令

在scrapyd-deploy 所在目录，新建 scrapyd-deploy.bat

```she
@echo off
C:\dev\Anaconda3\python.exe C:\dev\Anaconda3\Scripts\scrapyd-deploy %*
```



### scrapy sheel

```
 
# 增加用户代理
-s USER_AGENT='Mozilla/5.0
```



# xpath

```xml
xpath("string(//p[@class='EXAMPLE'])")
xpath("//p[@class='EXAMPLE']").xpath("string(.)")

```



### set



### bug解析

```python
1. 遇到错误网址后重定向至非domain 域名
	  scrapy.Request(meta={'dont_redirect': True, 'handle_httpstatus_list': [302]})
```