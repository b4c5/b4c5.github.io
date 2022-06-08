---
title: "如何恢复已有的hugo项目，以及基本的使用方法"
date: 2022-06-08T11:26:22+08:00
draft: false
keywords: hugo
description: 当我们在另外一台电脑上，如何去恢复已有的hugo项目，这里把相关步骤记录下来。
---
新建博客： [使用hugo来建立自己的博客]({{< ref "how-to-install-hugo-and-github-page.md" >}} "abc")

## 安装hugo

`brew install hugo`

## 下载仓库

`git clone --recursive https://github.com/b4c5/b4c5.github.io.git blog`

## 创建新文章

`cd blog`

`hugo new posts/how-to-use-hugo.md`

将在content/posts目录，创建 how-to-use-hugo.md 文件。同时，文件开头 ([Front Matter](https://gohugo.io/content-management/front-matter/#front-matter-formats)) 放入yaml格式的meta信息
```yaml
---
title: "如何使用Hugo"
date: 2022-06-08T11:26:22+08:00
draft: true
---
```
其中，draft 字段表示该文章默认不会发表，当改成 false 后，hugo 渲染时才会生成文章。

## 在本地运行网站
`hugo server --disableFastRender`
--disableFastRender: 关闭快速渲染模式，开启全部渲染（当有本地修改时）。<br />
访问 http://localhost:1313 。该服务将持续运行，所以，你修改本地文件后，页面上将自动反映出最新的修改。

## 生成文章

`hugo -D docs`

`-D`： 将文章内容标记为 draft 文件也生成文章， 忽略 draft 属性。

`docs`： 生成到 docs 目录

## 上传到gitpage

```bash
git add .
git commit -m '新文章：xxx'
git push origin
```

等10分钟后，github上面将生效。



