---
title: write blog steps
date: 2016-10-19 16:37:50
---

## 在新的环境使搭建Hexo操作步骤

0. 确保已经安装了 [Hexo](https://hexo.io/zh-cn/)
1. 从 [Github](www.github.com) Clone项目到一个文件夹
2. 进入Clone完成后的文件夹内
3. 执行 `git clone https://github.com/litten/hexo-theme-yilia.git themes/yilia`
4. 根据 `theme_source_config` 里的文件对比修改 `themes/yilia/` 里的文件
5. 之后就可以在根目录的 `source` 文件夹 (注意: 不是`theme/yilia`下的 `source`) 中添加`Markdown`文件了
6. 执行 `hexo` 的相应操作

## yilia主题操作
地址: **[https://github.com/litten/hexo-theme-yilia](https://github.com/litten/hexo-theme-yilia)**

### 使用
#### 安装
`$ git clone https://github.com/litten/hexo-theme-yilia.git themes/yilia`

#### 配置
> 修改 hexo根目录(Clone完成后的文件夹) 下的_config.yml ： theme: yilia

#### 更新
```
cd themes/yilia
git pull
```

### 配置文件说明
```
# Header

menu:
  主页: /
  随笔: /tags/随笔

# SubNav
subnav:
  github: "#"
  weibo: "#"
  rss: "#"
  zhihu: "#"
  #douban: "#"
  #mail: "#"
  #facebook: "#"
  #google: "#"
  #twitter: "#"
  #linkedin: "#"

rss: /atom.xml

# 是否需要修改 root 路径
# 如果您的网站存放在子目录中，例如 http://yoursite.com/blog，
# 请将您的 url 设为 http://yoursite.com/blog 并把 root 设为 /blog/。
root: 

# Content
excerpt_link: more
fancybox: true
mathjax: false

# 是否开启动画效果
animate: true

# 是否在新窗口打开链接
open_in_new: false

# Miscellaneous
google_analytics: ''
favicon: /favicon.png

#你的头像url
avatar:

#是否开启分享
share_jia: true
share_addthis: false

#是否开启多说评论，填写你在多说申请的项目名称 duoshuo: duoshuo-key
#若使用disqus，请在博客config文件中填写disqus_shortname，并关闭多说评论
duoshuo: false

# 如不需要，将该项置为false
# 比如
#smart_menu:
#  friends: false

smart_menu:
  innerArchive: '所有文章'
  tagcloud: '标签'
  friends: '友链'
  aboutme: '关于我'

friends:
  友情链接1: http://localhost:4000/
  友情链接2: http://localhost:4000/
  友情链接3: http://localhost:4000/
  友情链接4: http://localhost:4000/
  友情链接5: http://localhost:4000/
  友情链接6: http://localhost:4000/

aboutme: 很惭愧<br><br>只做了一点微小的工作<br>谢谢大家
```