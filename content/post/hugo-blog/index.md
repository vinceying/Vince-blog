---
title: 从 Fluid 到 Stack
description: 在十月的某一天我决定将博客从 Fluid 切换到 Stack。
date: 2020-10-06
slug: hugo-blog
image: gryffyn-m-OdoW3-o62EM-unsplash.jpg
categories:
    - 博客
tags:
    - Hugo
    - Stack
---
## 此前的博客

我换主题的几点原因：
- 审美疲劳（人总是会变的）
- 风格改变
- 在 V2EX 遇到了 Stack
- ∞

总的来说主要原因还是想多尝试一下，在使用 Fluid 的过程中我学习到了很多东西，对于 Git 的使用就是在此过程中熟悉起来的。我不会放弃关注和使用 Fluid，依旧使用 Fluid 作为我的备用博客。

## 停更的几个月

在六月底开始，我就很少更新博客了。主要是因为我去做暑假工了，体验了一下实习生的工作，导致没有什么时间关注的博客这一块上面。开学以后，我又遇到了一些十分糟糕的事情，导致我对生活失去了信心。经过反复思考，我认为博客应该作为我生活一个部分，博客应该成为日常。最大的作用就是能够督促我进行学习，five 养成计划。对自我的认知上，我急需提升自己。

## 使用 Hugo 的过程遇到的问题

### HTML 渲染

由于一些原因，在写博客的时候有习惯插入一些 HTML 片段，单纯 Markdown 语法渲染出的内容有些不够用。在 Markdown 文件中插入 HTML 片段后 HTML 片段无法加载，于是查看了源代码，HTML 片段前出现 `<!--raw Html omitted-->`，随即查找了官方文档发现了问题所在，Hugo 在最近的版本中使用了 Glodmark 作为默认库，出于安全考虑，默认状态下不加载 HTML 片段。于是就有了两个方法，一是开启不安全设置，二是换回 Blackfriday 库，我认为前者为最佳方案。

在配置文件 ``config.toml`` 中添加以下配置即可，以上问题算是解决了。
```toml
[markup]
    [markup.goldmark]
      [markup.goldmark.renderer]
         unsafe = true
```

### 使用 FTP-Deploy-Action 推送文件时的问题

我将 Hugo 的 Source 文件存放在 Github 的一个仓库中，使用 Github Actions 进行渲染及推送到服务器上，但流程执行完毕后，服务器仍然没有相关文件，同时在运行的过程中没有进行报错。

```yaml
name: github pages
on:
  push:
    branches:
      - main  # Set a branch to deploy

jobs:
  deploy:
    runs-on: ubuntu-18.04
    steps:
      - uses: actions/checkout@v2
        with:
          submodules: true  # Fetch Hugo themes (true OR recursive)
          fetch-depth: 0    # Fetch all history for .GitInfo and .Lastmod
      
      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: '0.75.1'
          # extended: true
      
      - name: Build
        run: hugo --minify
      
      - name: FTP-Deploy-Action
        uses: SamKirkland/FTP-Deploy-Action@3.1.1
        with:
          ftp-server: ftp-server
          ftp-username: username
          ftp-password: ${{ secrets.FTP_PASSWORD_BLOG }}
          local-dir: ./public
```

反复检查工作流文件后，我发现并没有什么明显的错误，查看 log 后基本上可以把问题定位在 FTP-Deploy-Action 的步骤上。

> If you don't commit your build folder to github you MUST create a .git-ftp-include file with the content !build/ so the folder is always uploaded!

查阅文档后发现，`./public` 中文件是在流程中生成的，并不是在仓库中的文件，需要添加 `.git-ftp-include` 文件后方可推送。思路上，我决定将`./public` 中文件推送到一个新的参考，然后在新的仓库使用 FTP-Deploy-Action。这样做的好处是可以使用 Github Pages 增加一个海外镜像网站（虽然没人看）。




