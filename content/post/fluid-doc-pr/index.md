---
title: Fluid 文档相关 PR 内容原稿
description: 本篇记录在 Fluid 文档中自己 PR 的内容原稿，持续更新中。
date: 2020-04-22
slug: fluid-doc-pr
image: fluid-doc.jpg
categories:
    - 技术
tags:
    - fluid
---
## 前言

本篇记录在 Fluid 文档中自己 PR 的内容原稿，持续更新中。

## 在 Fluid 使用更多图标的详细过程

### 说明
Fluid 在2020/4/5之前的版本使用的图标库是 FontAwesome，在之后的版本使用 Iconfont。相对而言，Iconfont 更适合我们的使用习惯，但是在 Iconfont 中，没用相关图标库项目共享功能，且主题作者也不能内置全部图标，只内置了部分社交图标，不过据了解，后续会更新更多图标。这篇帖子主要介绍如何引入更多图标，另外内容已经 PR 到 **[Fluid Doc](https://hexo.fluid-dev.com/docs/icon/)** 中。

### 关于 font-class 引用
主题图标使用 font-class 引用，font-class 是 unicode 使用方式的一种变种，比起 unicode 更加便捷和直观。iconfont 支持在线引用和本地引用。以下内容基于 font-class 引用。

### 本地应用
首先需要获取图标的 CSS 文件，将文件放到主题目录 'source/css' 中，在主题配置文件 custom_css 项中添加 CSS 文件路径。

实例

``` 
custom_css: /css/custom.css
```

在相关文件、页面，将 icon- 开头的那行填入 css class 即可。比如： iconfont icon-email 。

### Iconfont 在线引用
Iconfont支持用户创建项目来管理和使用图标库，在 图标管理-我的项目 中即可管理和创建项目。将所需图标添加至购物车，再通过购物车添加至所创建的项目中。在项目中可以下载至本地与生成在线链接，Iconfont 支持在阿里云的 CDN 中生成 CSS 或 JS 文件用来调用。

生成在线链接后，通过自定义 CSS调用，将 icon- 开头的那行填入 css class 即可，例如 iconfont icon-email）。在每次有删减项目中图标库目录时，需要在项目中点击重新生成链接才可生效。

## 相关插件推荐

### 可视化编辑页面与文档

[hexo-admin](https://github.com/jaredly/hexo-admin)

### 加密文章或页面

[hexo-blog-encrypt](https://github.com/MikeCoder/hexo-blog-encrypt)

## 常见功能

### 网站运行时间统计

本段代码来源于[网络](http://www.liangz.org/245.html)。在主题目录 `fluid/layout/_partial/footer.ejs` 文件中对应位置添加相关代码。

```html
<div>
  <span id="timeDate">载入天数...</span>
  <span id="times">载入时分秒...</span>
  <script>
  var now = new Date();
  function createtime(){
      var grt= new Date("02/14/2017 00:00:00");//此处修改你的建站时间或者网站上线时间
      now.setTime(now.getTime()+250);
      days = (now - grt ) / 1000 / 60 / 60 / 24;
      dnum = Math.floor(days);
      hours = (now - grt ) / 1000 / 60 / 60 - (24 * dnum);
      hnum = Math.floor(hours);
      if(String(hnum).length ==1 ){
          hnum = "0" + hnum;
      }
      minutes = (now - grt ) / 1000 /60 - (24 * 60 * dnum) - (60 * hnum);
      mnum = Math.floor(minutes);
      if(String(mnum).length ==1 ){
                mnum = "0" + mnum;
      }
      seconds = (now - grt ) / 1000 - (24 * 60 * 60 * dnum) - (60 * 60 * hnum) - (60 * mnum);
      snum = Math.round(seconds);
      if(String(snum).length ==1 ){
                snum = "0" + snum;
      }
      document.getElementById("timeDate").innerHTML = "本站安全运行&nbsp"+dnum+"&nbsp天";
      document.getElementById("times").innerHTML = hnum + "&nbsp小时&nbsp" + mnum + "&nbsp分&nbsp" + snum + "&nbsp秒";
  }
  setInterval("createtime()",250);
  </script>
</div>
```

### 一言

一言在博客网站中非常常见，正如它所说，用代码表达言语的魅力。在页面中，使用直接添加添加如下代码即可。在主题目录 `fluid/layout/_partial/footer.ejs` 文件中对应位置添加相关代码。以下代码为默认使用示例，如需要定制，请到 [hitokoto developer](https://developer.hitokoto.cn/) 中查看详情。

```html
<p id="hitokoto">:D 获取中...</p>
<script>
  fetch('https://v1.hitokoto.cn')
    .then(response => response.json())
    .then(data => {
      const hitokoto = document.getElementById('hitokoto')
      hitokoto.innerText = data.hitokoto
      })
      .catch(console.error)
</script>
```

### 在新建页面添加评论

目前，主题配置文件只能在文章页面添加评论，如果需要在新建的页面添加评论插件，请在打开评论插件的情况下，在文章页面查看评论插件对应的 HTML 代码，并添加到页面的 Markdown 文件中。

## 使用 jsDelivr 服务

插入来自 Github 仓库的图片，由于网络情况可能会出现加载慢和无法加载的情况，我们可以使用 [jsDelivr](https://www.jsdelivr.com/) 来加速图片文件等媒体文件的加载。

通常情况下，可以新建一个仓库来存放这些文件，目前已知的有图片、视频和引用的相关文件可以使用，Github 仓库最大上传文件为 25M，请注意文件大小。

使用方法（文件的绝对地址）

```
https://cdn.jsdelivr.net/gh/user/repo@version/file
```

相关实例（博客仓库下 master 分支里 favicon 图片文件）

```
https://cdn.jsdelivr.net/gh/fluid-dev/hexo-theme-fluid@master/source/img/favicon.png
```