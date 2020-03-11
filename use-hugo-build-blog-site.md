
# 如何使用 Hugo 搭建个人博客
弃坑 Hexo，投入 Hugo 的怀抱
> Hugo 其实是一个命令行工具，帮助我们生成静态网页。

## 一、安装
- masOS
    - 使用命令 `brew install hugo`
- Windows
    1. [下载 realse zip 包](https://github.com/gohugoio/hugo/releases)
    2. 解压，将 hugo.exe 文件放入**某盘:\software\Hugo\bin**
    3. 配置环境变量 PATH，添加 hugo.exe 存放路径

- 验证安装：
```shell
$ hugo version
Hugo Static Site Generator v0.67.0-7F1DA3EF windows/amd64 BuildDate: 2020-03-09T20:35:55Z
```

## 二、使用
参考[官方文档](https://gohugo.io/getting-started/quick-start/#step-2-create-a-new-site)
### Ⅰ 创建一个全新的 Hugo 网站，存放于指定的文件夹中
```
$ hugo new site amanda.github.io-creator
```

### Ⅱ 添加一个主题
1. 下载[主题](https://themes.gohugo.io/)，这里使用官方文档主题 Ananke theme
```
$ cd amanda.github.io-creator
$ git init
$ git submodule add https://github.com/budparr/gohugo-theme-ananke.git themes/ananke
$ rm -rf themes\ananke\.git
```

2. 向配置文件（config.toml）中追加主题配置
```shell
$ echo 'theme = "ananke"' >> config.toml
```

**注意！请注意！对于Windows用户来说，此处为后续埋下了一个坑，改用下述语句或手动删除单引号对**

```shell
$ echo theme = "ananke" >> config.toml
```

3. 更换主题：“下载主题” -> “修改主题配置”

### Ⅲ 开始写博客啦
1. 创建一篇新的博客，`hugo new posts/第一篇.md`
2. 打开 posts/第一篇.md，想写啥写啥

**每篇博客，都包含如下开头：**
```
---
title: "My First Post"
date: 2020-03-10T21:02:01+08:00
draft: true
---
```
**其中，draft 意为 “草稿”，默认值为true，代表此篇内容编译时将被忽略；改为 false，该篇博客才能被编译使用。**

## Ⅳ 启动 Hugo 服务
1. 启动命令，`hugo server -D`
2. 访问博客，默认网址 http://localhost:1313/
![](/images/ananke-style-view.png)

## Ⅴ 编译静态页面
1. 编译命令，`hugo -D`
2. 编译结果：默认生成 ./pulic 目录
3. 手动指定编译生成的目录
    - 编译命令中添加选项 [-d/--destination] 指定发布文件夹
    - 配置文件中通过 publishdir 指定

## Ⅵ 编译发布到 GitHub
1. 将 public 目录作为一个单独的 Git 仓库，上传到 GitHub
2. 设置 GitHub Pages，预览