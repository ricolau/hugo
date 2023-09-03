---
title: 基础使用
description: Hugo 的命令行模式 (CLI) 功能全面且简单易用，哪怕是对没怎么使用过精灵行的用户来讲也是如此。
categories: [getting started]
keywords: [使用,动态加载,命令行,flags]
menu:
  docs:
    parent: getting-started
    weight: 30
weight: 30
aliases: [/overview/usage/,/extras/livereload/,/doc/usage/,/usage/]
toc: true
---

## 安装完毕后测试

在 [安装完成][installing] 之后, 测试一下你的安装是否成功：

```bash
hugo version
```

正常的话你应该看到类似如下的内容：

```text
hugo v0.105.0-0e3b42b4a9bdeb4d866210819fc6ddcf51582ffa+extended linux/amd64 BuildDate=2022-10-28T12:29:05Z VendorInfo=snap:0.105.0
```

## 查看所有命令关键字和参数

如果想看所有的命令和传参方式：

```bash
hugo help
```

如果想查看子命令的帮助，可以使用子命令模式下的 `--help`  参数，比如：

```bash
hugo server --help
```

## 建立你的站点

如果想建立你的站点, 使用 `cd` 命令切换到你的项目目录，并且执行：

```bash
hugo
```

 [`hugo`] 命令会建设你的站点, 生成/发布网页文件到 `public` 目录。 如果想发布/生成你的站点到其他文件夹，使用 [`--destination`] 参数或者设置配置文件中的 [`publishDir`] 这一项的值。

{{% note %}}
每次在发布/生成内容之前，Hugo 不会清空 `public` 文件夹里的内容。已经存在的文件会被覆盖，但是不会删除。
此行为旨在防止无意中删除您可能在生成后独立添加到`public`目录中的文件。

根据您的需要，您可能需要在每次生成之前手动清除`public`目录内的内容。
{{% /note %}}

## 草稿、未来发布、过期内容

Hugo 支在你页面内容的[文件头][front matter]中设置 草稿`draft`, 日期 `date`, 发布日期`publishDate`, 和过期日期 `expiryDate`。默认情况下，在以下情况时 Hugo不会发布/生成这个页面：


-  `draft` 的值是 `true`
-  `date` 的值是未来的某一天
-  `publishDate` 的值是未来的某一天
-  `expiryDate` 的值代表的日期时间已经过了

如果想忽略这些状态特性，仍然让这些页面展示出来，你可以在执行 `hugo` 或者 `hugo server` 命令的时候带以下参数，比如：

```bash
hugo --buildDrafts    # or -D
hugo --buildExpired   # or -E
hugo --buildFuture    # or -F
```

尽管您也可以在站点配置中设置这些值，但除非所有内容作者都知道并理解这些设置，否则可能会导致不必要的结果。

{{% note %}}
如上所述，每次在发布/生成内容之前，Hugo 不会清空 `public` 文件夹里的内容。
根据以上四个条件的设定，在生成之后，您的`public`目录可能包含以前生成的无关文件。
一种常见的做法是在每次生成之前手动清除公共目录的内容，以删除草稿、过期和将来要发布的内容。
{{% /note %}}

## 开发和测试您的站点

在开发布局或者编写内容的时候，如果想还要浏览您的站点，使用 `cd` 命令切换到你的项目根目录并且执行如下命令：

```bash
hugo server
```

 [`hugo server`] 这个命令会在内存中临时构建起来你的站点，并且使用一个最小化的HTTP服务器来让你的页面可以运行起来查看。
 当你执行 `hugo server` 这个命令的时候，它将先是你本地的预览网址，可以在浏览器中打开这个网址：

```text
Web Server is available at http://localhost:1313/ 
```

当服务器运行时，它会监视项目目录中的资源、配置、内容、数据、布局、翻译和静态文件的更改。当检测到更改时，服务器会重建您的网站，并使用[动态载入][LiveReload]刷新您的浏览。

大多数Hugo构建都非常快，除非你直接查看浏览器，否则你可能不会注意到变化。

### 动态载入
当服务器运行时，Hugo将JavaScript注入生成的HTML页面中。[动态载入][LiveReload]脚本通过web socket中创建从浏览器到服务器的连接。您不需要安装任何软件或浏览器插件，也不需要任何配置。

### 自动重定向

编辑内容时，如果希望浏览器自动重定向到刚刚最新修改的页面，请运行：

```bash
hugo server --navigateToChanged
```

## 部署你的站点

{{% note %}}
如上所述，Hugo在构建您的网站之前不会清除`public`目录中的内容。在每次生成之前手动清除公共目录的内容，以删除草稿、过期内容和将来要发布的内容。
{{% /note %}}

如果你准备好了要部署你的站点，请执行：
```bash
hugo
```
这将生成您的网站，并将文件发布到`public`目录。目录结构如下所示：

```text
public/
├── categories/
│   ├── index.html
│   └── index.xml  <-- RSS feed for this section
├── post/
│   ├── my-first-post/
│   │   └── index.html
│   ├── index.html
│   └── index.xml  <-- RSS feed for this section
├── tags/
│   ├── index.html
│   └── index.xml  <-- RSS feed for this section
├── index.html
├── index.xml      <-- RSS feed for the site
└── sitemap.xml
```

在通常的简单的服务器或者主机配置中，你一般会使用 `ftp`、 `rsync`、或者 `scp` 等命令把你的内容发布到虚拟主机/服务器中, 所有你需要发布的文件都在 `public` 目录中，发布这个目录里的内容即可。

很多用户部署自己的站点使用 CI/CD 流程工具， 使用这些工具的一个 推送/push[^1] 命令把内容发布到对应的 GitHub 或者 GitLab 仓库中，会自动触发构建和部署。常见的服务提供商包括 [AWS Amplify], [CloudCannon], [Cloudflare Pages], [GitHub Pages], [GitLab Pages], 和 [Netlify].

想了解更多可以到 [虚拟主机和部署][hosting and deployment] 部分。

[^1]: Git存储库包含整个项目目录，但通常不包括`public`目录，因为站点是在“推送/push” 命令之后构建的。

[`--destination`]: /commands/hugo/#options
[`hugo server`]: /commands/hugo_server/
[`hugo`]: /commands/hugo/
[`publishDir`]: /getting-started/configuration/#publishdir
[AWS Amplify]: https://aws.amazon.com/amplify/
[CloudCannon]: https://cloudcannon.com/
[Cloudflare Pages]: https://pages.cloudflare.com/
[commands]: /commands/
[front matter]: /content-management/front-matter/
[GitHub Pages]: https://pages.github.com/
[GitLab Pages]: https://docs.gitlab.com/ee/user/project/pages/
[hosting and deployment]: /hosting-and-deployment/
[hosting]: /hosting-and-deployment/
[installing]: /installation/
[LiveReload]: https://github.com/livereload/livereload-js
[Netlify]: https://www.netlify.com/
