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

 

