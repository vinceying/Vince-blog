---
title: 最简单的内网穿透，Zerotier 试用
description: 最简单内网穿透，Zerotier 流水账试用过程。
date: 2020-12-18
slug: zerotier
image: https://cdn.vince.pub/blog-file/photo/zerotier.png
categories:
    - 技术
tags:
    - 内网穿透
    - zerotier
---

## 前言

这里不得不吐槽一下学校，把一些仪器和教务管理系统设置在内网，必需通过校园网的环境下才能访问，然后无线网覆盖范围和信号状态及其虚拟专用网客户端长期不更新。之前有提到过，在宿舍购置了一台路由器，运行的是 OpenWrt。在宿舍中的路由器是可以访问校园网的，决定使用内网穿透解决这个问题。另外，深有体会，越好的大学信息服务越好,没有高考的同学加油。

{{<link "ruijie-router">}}

## 开始使用

Zerotier 是一个开源软件，同时也被称谓傻瓜式的内网穿透软件。免费版即可支持50个设备，已经足够我的使用了。简单的说，注册完创建了 Network ID 后输入设备的客户端中，配置完路由规则后，即可开始使用了。

### 通过 SSH 安装 Zentier

OpenWRT 使用 Opkg 来进行包管理，通过 Opkg 命令直接安装 Zentier 即可。

```shell
opkg install zentier
```

配置文件的路径是 `/etc/config/zerotier`，使用熟悉的编辑器对其进行配置即可，这里以 Vim 为例。部分路由器固件(例如 LuCI）内置了 Zentier,可以可视化操作。

```shell
vim /etc/config/zerotier
```

以我的配置为例，其中由于我的路由器内置了 Zerotier，设置 option nat 项允许 Zerotier 拨入客户端访问路由器 LAN 资源。之后，放行出入站数据。在 OpenWrt 的 `网络-防火墙` 中将 `出站数据` 和 `入站数据` .


```bash
config zerotier 'sample_config'
        option config_path '/etc/zerotier'
        list join '<your Network ID>'
        option enabled '1'
        option nat '1'
```

### 配置使用

在 Zerotier 网页配置 `Managed Routes` 中添加添加路由设置。以外网访问路由器管理页面（192.168.2.1）为例。其中我的 IP 段为
10.10.10.1~10.10.10.225 。


```s
10.10.10.0/24  (LAN)  # 系统默认
192.168.2.0/24  Via  10.10.10.171  # 简要的意思访问路由器管理页面时使用路由器进行
```

之后通过其它设备安装 Zerotier 客户端，填入 `Network ID` 后，在网页配置中运行相关设计即可。

## 总结

- 这篇有偷懒成分，流水账试用过程（文章凑数）。
- 不同版本和固件版本使用可能存在差异，以实际为准。
- 受网络环境的局限较大，不同网络环境下情况不同。在我的使用中，访问网页需求基本上可以满足，但是如果使用想通过此方法访问本地 NAS 的数据，我觉得实现起来不太理想。






