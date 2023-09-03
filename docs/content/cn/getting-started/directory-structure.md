---
title: 目录结构
description: Hugo的CLI命令行构建了一个项目目录结构，然后将该目录作为输入创建一个完整的网站。
categories: [fundamentals,getting started]
keywords: [source, organization, directories]
menu:
  docs:
    parent: getting-started
    weight: 30
weight: 30
aliases: [/overview/source-directory/]
toc: true
---

## 新网站脚手架 New site scaffolding

{{< youtube sB0HLHjgQ7E >}}

从命令行运行“ `hugo new site example`将创建一个包含以下元素的目录结构：

```txt
example/
├── archetypes/
│   └── default.md
├── assets/
├── content/
├── data/
├── layouts/
├── public/
├── static/
├── themes/
└── hugo.toml
```

## 目录结构释义 Directory structure explained

以下是每个目录的高级概述，其中包含指向Hugo文档中各个部分的链接。

[`archetypes`](/content-management/archetypes/)目录
: 您可以使用 `hugo new content` 命令在Hugo中创建新的内容文件。
默认情况下，Hugo将创建至少具有`date`、`title`（根据文件名推断）和`draft = true`的新内容文件。这为使用多种内容类型的网站节省了时间并提高了一致性。您也可以使用自定义的预配置前置字段创建自己的[原型][archetypes]。

[`assets`]目录
: 存储所有需要由[Hugo Pipes](/hugo-pipes/)处理的文件。只有使用了`.Permalink` 或`.RelPermalink`的文件才会发布到 `public` 目录。


[`config`](/getting-started/configuration/)目录：Hugo支持大量的[配置命令][configuration directives]。
这些配置文件内容都以JSON、YAML或者TOML文件的形式存储在 [配置文件夹configuration directory](/getting-started/configuration/#configuration-directory)。
每个根设置对象都可以作为自己的文件，并由环境进行结构化。
设置最少且不需要管理开发、生产环境的项目可以在其根目录下使用单个`hugo.toml` 文件。

许多网站可能几乎不需要配置，但Hugo提供了大量的[配置指令][configuration directives] ，以方便对Hugo如何构建网站进行更精细的配置和表达。

[`content`]目录
: 您网站的所有内容都将位于此目录中。 Hugo中的每个顶级文件夹都被视为[内容部分][content section]。

 例如，如果您的网站有三个主要部分---`blog`, `articles`, 和 `tutorials`---你将有三个不同的目录在 `content/blog`, `content/articles`, and `content/tutorials`。 Hugo使用子目录来指定默认值[content types].

[`data`](/templates/data-templates/)
目录 : 此目录用于存储配置文件
Hugo在生成您的网站时使用。您可以用YAML、JSON或TOML格式编写这些文件。 除了添加到此文件夹中的文件外，您还可以创建从动态内容中提取的[数据模板][data templates]。


[`layouts`]目录
: 以`.html`文件的形式存储模板，这些文件指定如何将内容展示呈现到静态网站中。
其中包含[列表页模板list pages][lists]、 你的 [主页模板 homepage][homepage]、 [分类模板 taxonomy templates][taxonomy templates]、 [子模板partials][partials]、 [单一页面模板single page templates][singles] 以及更多模板等。


[`static`]目录
: 存储所有静态内容：图像、CSS、JavaScript等。当Hugo构建您的网站时，静态目录中的所有资产都会按原样复制。
使用 `static`文件夹的一个很好的例子是[在谷歌搜索控制台上验证网站所有权][searchconsole]，您希望Hugo在不修改其内容的情况下直接展示完整原样的HTML文件中的内容。


{{% note %}}
从**Hugo 0.31**开始，您可以拥有多个静态目录。
{{% /note %}}

[`resources`]目录
: 缓存一些文件以加快生成速度。模板作者也可以使用它来分发构建的Sass文件，所以你不必安装预处理器。注意：默认情况下不会创建resources目录。


[archetypes]: /content-management/archetypes/
[`assets`]: /hugo-pipes/introduction#asset-directory
[configuration directives]: /getting-started/configuration/#all-configuration-settings
[`content`]: /content-management/organization/
[content section]: /content-management/sections/
[content types]: /content-management/types/
[data templates]: /templates/data-templates/
[homepage]: /templates/homepage/
[`layouts`]: /templates/
[`static`]: /content-management/static-files/
[`resources`]: /getting-started/configuration/#configure-file-caches
[lists]: /templates/lists/
[pagevars]: /variables/page/
[partials]: /templates/partials/
[searchconsole]: https://support.google.com/webmasters/answer/9008080#zippy=%2Chtml-file-upload
[singles]: /templates/single-page-templates/
[taxonomies]: /content-management/taxonomies/
[taxonomy templates]: /templates/taxonomy-templates/
[types]: /content-management/types/
