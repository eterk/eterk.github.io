---
layout: page
title: 文章
permalink: /article/
---
大致是按照操作的

[gh-操作] (https://docs.github.com/cn/pages/setting-up-a-github-pages-site-with-jekyll/creating-a-github-pages-site-with-jekyll)

遇到的主要问题是：

1. 在进行本地服务器初始化的时候`bundle install` 等待太久,我替换了Gemfile 中的source 解决了

```yaml
# source "https://rubygems.org"
source 'http://rubygems.org'
```

2. 在`bundle install` 后启动server 仍然报依赖缺失，`bundler: failed to load command: jekyll, `require': cannot load such file -- webrick`

  solution:

        ```
        ~ bundle add webrick# (缺失的包)
        ~ bundle install
        ~ bundle exec jekyll server
        ```
3. 在提交文件遇到的一些问题

本机之前没有安装过git ，我下载了一个最新版的git之后，直接新建了本地仓库，然后配置了用户名，邮箱等，最后连接了github 

由于之前在github 页面创建过一个仓库，提交了一些文件，后来按照说明来在本地创建仓库，新的仓库代码无法合并到远程github 上，
首先是pull 被拒绝
```
https://komodor.com/learn/how-to-fix-fatal-refusing-to-merge-unrelated-histories-error/
   git pull origin master --allow-unrelated-histories #使用这个方法解决
```

在pull 的过程中访问远程 失败 出现这个错误
OpenSSL SSL_read: Connection was reset, errno 10054
我使用设置不验证跳过这个错误。
`git config --global http.sslVerify "false"`
 

