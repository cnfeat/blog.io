---
layout: post
title: Ubuntu 16.04 怎么将桌面左侧的启动器移动到屏幕底部?
date: 2016-10-10
categories: blog
tags: [ubuntu]
description:  Ubuntu 16.04 怎么将桌面左侧的启动器移动到屏幕底部?

---

>与其他 Linux 发行版不同，Ubuntu 多年来一直使用 Unity 做桌面环境，该环境的最突出特点就是桌面左侧有一个启动器栏（Launcher）。
>从 16.04 版本开始，Ubuntu 提供了一个命令行选项，可以将 Launcher 启动器移动到屏幕的底部。

### 启动器移动到屏幕底部：

按下 Ctrl + Alt + t 键盘组合键调出终端，在终端中输入以下命令：

`gsettings set com.canonical.Unity.Launcher launcher-position Bottom`

按下回车。命令中最后的 Bottom 就是底部的意思。就会发现 Launcher 启动器，已经移动到了屏幕的底部。除了位置发生变化，Launcher 其他的功能和设置没有发生任何改变。

### 启动器移回屏幕左侧：

再次调出终端，输入以下命令：

`gsettings set com.canonical.Unity.Launcher launcher-position Left`

**注意无论是上一步骤中的 Bottom，还是本步骤中的 Left，首字母都要大写。**

会发现 Launcher 启动器又重新回到了屏幕左侧。

如果将上述命令的最后一个参数更改成 Right，则 Launcher 不会发生任何位置移动。

看来上述命令只能将 Launcher 的位置在屏幕左侧和底部之间切换。
