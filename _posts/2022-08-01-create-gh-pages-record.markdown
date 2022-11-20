---
layout: post
title:  "创建gh-pages站点"
date:   2022-08-01 13:30:00 +0000
categories: 技术文章
tags : application
---

大致是按照[github blog 文档](https://docs.github.com/cn/pages/setting-up-a-github-pages-site-with-jekyll/creating-a-github-pages-site-with-jekyll)操作的

###遇到的主要问题
- [x] 在进行本地服务器初始化的时候`bundle install` 等待太久,我替换了Gemfile 中的source 解决了

  ```yaml
  # source "https://rubygems.org"
  source 'http://rubygems.org'
  ```

- [x] 在`bundle install` 后启动server 仍然报依赖缺失 
  > bundler: failed to load command: jekyll, 
  > require': cannot load such file -- webrick


  ```
  ~ bundle add webrick# (缺失的包)
  ~ bundle install
  ~ bundle exec jekyll server # 启动页面服务 Build the site and make it available on a local server
  ~ jekyll server # 也是启动页面服务
  ```

- [x] 在提交文件遇到的一些问题

  本机之前没有安装过git ，我下载了一个最新版的git之后，直接新建了本地仓库，然后配置了用户名，邮箱等，最后连接了github 

  由于之前在github 页面创建过一个仓库，提交了一些文件，后来按照说明来在本地创建仓库，新的仓库代码无法合并到远程github 上，
  - 首先是pull 被拒绝
      [解决方案](https://komodor.com/learn/how-to-fix-fatal-refusing-to-merge-unrelated-histories-error/)

  ```
   git pull origin master --allow-unrelated-histories 
  ```

  - 然后在pull 的过程中访问远程 失败 出现这个错误

    > OpenSSL SSL_read: Connection was reset, errno 10054
    
    我使用设置不验证跳过这个错误。
    
    `git config --global http.sslVerify "false"`

- [x] post 中的文章老是没法正常展示在页面中，经过搜索检查原因总结如下
   [stackoverflow 回答](https://stackoverflow.com/questions/30625044/jekyll-post-not-generated)
  > The post is not placed in the _posts directory.
  > The post has incorrect title. Posts should be named YEAR-MONTH-DAY-title.MARKUP (Note the MARKUP - extension, which is usually .md or .markdown)
  > The post's date is in the future. You can make the post visible by setting future: true in _config.yml (documentation)
  > The post has published: false in its front matter. Set it to true.
  > The title contains a : character. Replace it with &#58. Works in jekyll 3.8.3 (and probably in other 'recent' releases).

- [x] 尝试使用gitment 给个人博客添加品论区，毕竟无法互动的博客没有灵魂
  > [参考教程](https://imsun.net/posts/gitment-introduction/)
  > [项目链接](https://github.com/imsun/gitment)  
  > 不过我配置完以后，初始化评论登陆后报错，暂时没有成功[object ProgressEvent]

- [X] 个人博客里有许多模板语法，这些语法是 Liquid.js 语法，尝试修改的话可以查看[API](https://liquidjs.com/api/classes/liquid_.liquid.html)

- [X] 在博客中添加数学表达式，参考了[stackoverflow](https://stackoverflow.com/questions/34347818/using-mathjax-on-a-github-page)的回答
  ```
  // append script import inside the <head/> of your _layouts/default.html file`
  <script type="text/javascript" id="MathJax-script" async
  src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js">
</script>
<!-- use dollar signs (i.e. $1 + 2$) to escape math sequence  -->
<script>
  MathJax = {
    tex: {
      inlineMath: [['$', '$']]
    }
  };
</script>



<div id="container"></div>
<link rel="stylesheet" href="https://imsun.github.io/gitment/style/default.css">
<script src="https://imsun.github.io/gitment/dist/gitment.browser.js"></script>
<link rel="stylesheet" href="https://billts.site/extra_css/gitment.css">
<script src="https://billts.site/js/gitment.js"></script>
<script>
var gitment = new Gitment({
//   id: '<%= page.title %>', // 可选。默认为 location.href
  owner: 'eterk',
  repo: 'eterk.github.io',
  oauth: {
    client_id: 'f35aa6f240b2b42f8293',
    client_secret: 'af3c17445a1a1caa52703a3633338d507e513595',
  },
})
gitment.render('container')
</script>



- [x] 最近系统更新 `jekyll server` 命令突然运行报错 `unexpected ucrtbase.dll`
 这个提示其实时启动ruby 时产生的,不知道微软的破系统更新老是有新bug 产生,要么等系统升级解决bug,要么把ruby 降级.  

 后来决定把ruby 降级(3.1.2->3.0.4),在网上看到 ruby 3.0.4 版本没有这个问题,于是先卸载再重新安装jekyll 后解决.
 安装jekyll 过程中又遇到一些问题

 因为我在windwos 环境上,所以 提示我安装一个[mysy2](https://www.msys2.org/),后来`gem install jekyll` 还是报错

 > ERROR:  Error installing jekyll:
   ERROR: Failed to build gem native extension.

 [网上冲浪](https://stackoverflow.com/questions/51699761/error-installing-jekyll-error-failed-to-build-gem-native-extension))后发现是我图下载快了一个不带DEVKIT 的ruby 导致的,于是卸载原来安装的 重新下载安装后解决了
- [x] 添加目录
由于 jekyll 使用的是 `kmardown` 不支持 [toc] 目录语法,
所以使用[liquid 解决方案](https://github.com/allejo/jekyll-toc)

```text
Download the toc.html file from the latest release or the master branch

Toss that file in your _includes folder.

Use it in your template layout where you have {{ content }} which is the HTML rendered from the markdown source with this liquid tag:

{% include toc.html html=content %}
```