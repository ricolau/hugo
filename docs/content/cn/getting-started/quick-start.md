---
title: 快速开始
description: 学会在几分钟内创建一个Hugo网站。
categories: [getting started]
keywords: [quick start,usage]
menu:
  docs:
    parent: getting-started
    weight: 20
weight: 20
toc: true
aliases: [/quickstart/,/overview/quickstart/]
---

在本教程中，您将：

1.创建网站
2.添加内容
3.配置站点
4.发布网站

## 前置依赖条件

在开始本教程之前，您必须：

1. [已经安装了 Hugo][Install Hugo] (扩展版本, v0.112.0 或者更新的版本)
1. [已经安装了 Git][Install Git]

您还必须能够舒适地在命令行中工作。

## 创建网站

### 命令行

{{% note %}}
**如果您是Windows用户：**

- 不要使用CMD命令提示符
- 不使用Windows PowerShell
- 需要从 [PowerShell] 或者一个Linux 类的命令行终端比如 WSL 或者 Git Bash 中执行以下命令：

“PowerShell” 和 “Windows PowerShell” [是不同的程序][are different applications]。

[PowerShell]: https://learn.microsoft.com/en-us/powershell/scripting/install/installing-powershell-on-windows
[are different applications]: https://learn.microsoft.com/en-us/powershell/scripting/whats-new/differences-from-windows-powershell?view=powershell-7.3
{{% /note %}}

运行以下命令创建一个主题为[Ananke]的Hugo 站点。 下一节将对每个命令进行解释。

```text
hugo new site quickstart
cd quickstart
git init
git submodule add https://github.com/theNewDynamic/gohugo-theme-ananke.git themes/ananke
echo "theme = 'ananke'" >> hugo.toml
hugo server
```

在终端中显示的URL处查看您的网站，按 `Ctrl + C` 可以终止Hugo开发环境服务进程。

### 命令的解释
在`quickstart` 目录为您的项目创建[项目目录结构][directory structure]

```text
hugo new site quickstart
```

将当前目录更改为项目的根目录。
```text
cd quickstart
```

将当前目录初始化为一个空的 Git 仓库。
```text
git init
```
将[Ananke]主题克隆到`themes` 目录中，将其作为[Git submodule]添加到您的项目中。

```text
git submodule add https://github.com/theNewDynamic/gohugo-theme-ananke.git themes/ananke
```

在站点配置文件中附加一行，指明网站为当前主题。

```text
echo "theme = 'ananke'" >> hugo.toml
```

启动Hugo的开发服务器来查看网站。

```text
hugo server
```

按 `Ctrl + C` 来停止 Hugo 开发服务器进程。

## Add content

将一个新页面添加到您的网站。

```text
hugo new content posts/my-first-post.md
```

Hugo在`content/posts`目录中创建了该文件。用编辑器打开文件。

```text
---
title: "My First Post"
date: 2022-11-20T09:03:20-08:00
draft: true
---
```

注意在[页面头信息 front matter][front matter]中的`draft`（草稿）项配置的值是`true`。默认情况下在发布网站的时候 Hugo不会发布draft（草稿） 内容。 了解更多关于[草稿、将来发布、和过期内容][draft, future, and expired content].

添加一些 [markdown] 内容到文章中，但是仍然不更改 `draft` 草稿状态。

[markdown]: https://commonmark.org/help/

```text
---
title: "我发布的第一个页面"
date: 2022-11-20T09:03:20-08:00
draft: true
---
## 介绍

这是 **加粗** 文本, 这是一个 *重读* 文本.

访问 [Hugo](https://gohugo.io) 网站!
```

保存文件，然后启动Hugo的开发服务器来查看网站。可以运行以下任一命令以支持草稿内容的预览。

```text
hugo server --buildDrafts
hugo server -D
```

在终端中显示的URL处查看您的网站。在继续添加和更改内容的同时，保持开发服务器的运行。
{{% note %}}
Hugo 的渲染引擎遵循通用的[CommonMark][specification] markdown 语法. CommonMark组织提供了一个有用的[实时测试工具][live testing tool]来支持这些语法的测试实现。

[live testing tool]: https://spec.commonmark.org/dingus/
[specification]: https://spec.commonmark.org/
{{% /note %}}

## 配置网站

用你的编辑器打开在你项目根目录下的 [网站配置文件][site configuration]  (`hugo.toml`)。

```text
baseURL = 'http://example.org/'
languageCode = 'en-us'
title = '我的新 Hugo 站点'
theme = 'ananke'
```

将配置文件中的内容做如下调整:

1. 为你的生产网站设置“基础路径” `baseURL` . 该值必须以协议比如 http:// 开头，并以斜线结尾，如上所示。

2. 更改语言设置，修改为你需要的语言和地区 `languageCode`。

3. 为你的生产网站写好“标题” `title` 。

启动Hugo的开发服务器以查看您的更改，记住启动参数里要包含对草稿内容的支持参数。

```text
hugo server -D
```

{{% note %}}
大多数主题作者都提供配置指南和选项。别忘了访问主题的仓库或文档网站以获取更多关于使用主题的详细信息。

[The New Dynamic]  Ananke主题的作者, 提供 [文档][documentation] 可以帮大家配置和使用。他们同样也提供了 [示例网站][demonstration site].

[demonstration site]: https://gohugo-ananke-theme-demo.netlify.app/
[documentation]: https://github.com/theNewDynamic/gohugo-theme-ananke#readme
[The New Dynamic]: https://www.thenewdynamic.com/
{{% /note %}}

## 发布站点

在这一步你将“发布/生成”_publish_ 你的站点，但并不会“部署” _deploy_ 它到服务器上。

当你“发布/生成” _publish_ 你的站点, Hugo在项目根目录的 `public` 目录中创建整个静态站点。 这包括HTML文件以及图像、CSS文件和JavaScript文件等资源。

当你“发布/生成” _publish_ 你的站点时, 你一般不想包含[草稿、未来发布、过期内容][draft, future, or expired content]等，要实现这一点，只需要一个最简单的命令即可：

```text
hugo
```

如果想要了解如何“部署” _deploy_ 你的站点, 可以查看[主机和部署][hosting and deployment] 部分.

## 寻求更多帮助

Hugo的[论坛][forum] 是一个用户、开发者回答问题、分享知识、提供案例等方面都比较活跃的论坛。快速搜索20000多个主题通常可以回答您的问题。在提出第一个问题之前，请务必阅读有关[请求帮助][requesting help]的内容。

## 其他资源

有关帮助您学习Hugo的其他资源，包括书籍和视频教程，请参阅 [更多学习资源](/getting-started/external-learning-resources/) 页面.

[Ananke]: https://github.com/theNewDynamic/gohugo-theme-ananke
[directory structure]: /getting-started/directory-structure
[draft, future, and expired content]: /getting-started/usage/#draft-future-and-expired-content
[draft, future, or expired content]: /getting-started/usage/#draft-future-and-expired-content
[external learning resources]:/getting-started/external-learning-resources/
[forum]: https://discourse.gohugo.io/
[forum]: https://discourse.gohugo.io/
[front matter]: /content-management/front-matter
[Git submodule]: https://git-scm.com/book/en/v2/Git-Tools-Submodules
[hosting and deployment]: /hosting-and-deployment/
[Install Git]: https://git-scm.com/book/en/v2/Getting-Started-Installing-Git
[Install Hugo]: /installation/
[Requesting Help]: https://discourse.gohugo.io/t/requesting-help/9132
[Requesting Help]: https://discourse.gohugo.io/t/requesting-help/9132
[site configuration]: /getting-started/configuration/
