---
layout: post
title:  "创建gh-pages 站点"
date:   2022-08-01 13:30:00 +0000
categories: 技术文章
tags : application
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
        ~ bundle exec jekyll server # 启动页面服务 Build the site and make it available on a local server
        ~ jekyll server # 也是启动页面服务
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

4.  Layout 'about' requested in _ does not exist
暂时不太搞懂layout 和页面是怎么关联的


5. post 中的文章老是没法正常展示在页面中，经过搜索检查原因总结如下
   [stackoverflow 回答](https://stackoverflow.com/questions/30625044/jekyll-post-not-generated)
> The post is not placed in the _posts directory.
> The post has incorrect title. Posts should be named YEAR-MONTH-DAY-title.MARKUP (Note the MARKUP - extension, which is usually .md or .markdown)
> The post's date is in the future. You can make the post visible by setting future: true in _config.yml (documentation)
> The post has published: false in its front matter. Set it to true.
> The title contains a : character. Replace it with &#58. Works in jekyll 3.8.3 (and probably in other 'recent' releases).
> 