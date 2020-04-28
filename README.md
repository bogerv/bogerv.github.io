# 基于Hexo的博客文件 bogerv.github.io

## 1. 安装Hexo

> 如果用到本地搜索也要安装 hexo-generator-search, 并且要安装到项目的根目录

> `npm install hexo-generator-search --save`

## 2. 安装相应主题

```zsh
cd bogerv.github.io/themes
git clone https://github.com/bogerv/hexo-theme-obsidian.git obsidian
```

## 3. 将主题配置文件复制到主题文件夹下

> 注意: 主题配置文件和项目配置文件虽然名称相同, 但是文件内容不同

## 4. 在根目录运行相应的hexo命令即可

```sh
hexo clean
hexo generator
hexo server
hexo deploy
```
