---
title: 菜单栏
description:  通过定义条目、本地化每个条目并呈现生成的数据结构来创建菜单。
categories: [content management]
keywords: [菜单]
menu:
  docs:
    parent: content-management
    weight: 190
toc: true
weight: 190
aliases: [/extras/menus/]
---

## Overview

要为网站创建菜单，请执行以下操作：

1. 定义菜单中的每一个条目
2. 指定每一个条目的[位置][Localize]
3. 使用一个[模板][template]渲染菜单

创建多个菜单，可以是一层平铺菜单，也可以是嵌套菜单。例如，为页眉创建一个主菜单，为页脚创建一个单独的菜单。

有三种方法可以定义菜单项：

1. 自动根据文件目录创建
1. 使用头信息配置内容创建
1. 在网站的配置中内容创建

{{% note %}}
尽管在定义菜单时可以组合使用这些方法，但如果在整个网站中使用一种方法，菜单将更容易概念化和维护。
{{% /note %}}

## 自动化定义/生成菜单

若要为网站的每个顶级分区自动定义菜单项，请在网站配置中启用分区页面菜单。

{{< code-toggle file="hugo" copy=false >}}
sectionPagesMenu = "main"
{{< /code-toggle >}}

这将创建一个菜单结构，您可以使用模板中的`site.Menus.main` 访问该结构。有关详细信息，请参见[菜单模板][menu templates]。

## 在每个页面头信息中配置菜单

如果想把 某个页面的链接 加入到 主菜单 中：

{{< code-toggle file="content/about.md" copy=false fm=true >}}
title = 'About'
menu = 'main'
{{< /code-toggle >}}

使用模板中的 `site.Menus.main` 访问条目。有关详细信息，请参见[菜单模板][menu templates]。

如果想把 某个页面的链接 加入到 主菜单、底部菜单中：

{{< code-toggle file="content/contact.md" copy=false fm=true >}}
title = 'Contact'
menu = ['main','footer']
{{< /code-toggle >}}

访问模板中带有`site.Menus.main` 和`site.Menus.footer`的条目。有关详细信息，请参见[菜单模板][menu templates]。


### 头信息中定义菜单时可以使用的属性配置 {#properties-front-matter}

如果想要在 页面头信息中定义菜单条目，可以使用以下 属性：

identifier
:
：（`string` 字符串格式）当两个或多个菜单项具有相同的`name`时，或使用翻译表本地化`name'时，此项为必需项。必须以字母开头，后跟字母、数字或下划线。

name
: (`string` 字符串格式) 呈现菜单项时要显示的文本。

params
: (`map` 字典类型) 菜单项的用户自定义属性。

parent
:（`string` 字符串类型）父菜单项的“`identifier` 标识符”。如果未定义“identifier”，请使用`name`。嵌套菜单中的子项是必需的。

post
: (`string` 字符串类型) 呈现菜单项时要附加的HTML。

pre
: (`string` 字符串类型) 呈现菜单项时 附在前面作为前缀的HTML。

title
: (`string` 字符串类型) 呈现菜单项的HTML`title`属性。


weight
: (`int` 整数 )一个非零整数，指示条目相对于菜单根的位置，或子条目相对于其父条目的位置。较小的整数标识的条目浮到顶部，而较大的整数标识的条目沉到底部。

### 头信息中定义菜单的示例 {#example-front-matter}

此重要菜单项演示了一些可用属性：

{{< code-toggle file="content/products/software.md" copy=false fm=true >}}
title = 'Software'
[menu.main]
parent = 'Products'
weight = 20
pre = '<i class="fa-solid fa-code"></i>'
[menu.main.params]
class = 'center'
{{< /code-toggle >}}

使用模板中的h `site.Menus.main`访问条目。有关详细信息，请参见[菜单模板][menu templates]。

## 在站点配置中定义菜单

定义“主菜单”条目的方式：

{{< code-toggle file="hugo" copy=false >}}
[[menu.main]]
name = 'Home'
pageRef = '/'
weight = 10

[[menu.main]]
name = 'Products'
pageRef = '/products'
weight = 20

[[menu.main]]
name = 'Services'
pageRef = '/services'
weight = 30
{{< /code-toggle >}}

这将创建一个菜单结构，您可以使用模板中的`site.Menus.main` 访问该结构。有关详细信息，请参见[菜单模板][menu templates]。


为网站“底部信息栏”定义菜单项：

{{< code-toggle file="hugo" copy=false >}}
[[menu.footer]]
name = 'Terms'
pageRef = '/terms'
weight = 10

[[menu.footer]]
name = 'Privacy'
pageRef = '/privacy'
weight = 20
{{< /code-toggle >}}

这将创建一个菜单结构，您可以使用模板中的`site.Menus.footer`访问该结构。有关详细信息，请参见[菜单模板][menu templates]。

### 站点配置内容中配置菜单时，可以用的属性 {#properties-site-configuration}

{{% note %}}
在 [头部信息中定义菜单项的属性][properties available to entries defined in front matter] 也可以在“网站配置项”中定义菜单项的时候使用，一样的能力。

[properties available to entries defined in front matter]: /content-management/menus/#properties-front-matter
{{% /note %}}

站点配置中定义的每个菜单项都需要两个或多个属性：

- 为内部链接定义 `name` 和 `pageRef` 两个属性
- 为外部链接定义 `name` 和 `url` 两个属性

pageRef
: (`string` 字符串类型) 目标页面相对于`content`目录的文件路径。不需要包含“多语言代码”和“文件扩展名”在其中。*内部* 链接需要。

Kind|pageRef
:--|:--
home|`/`
page|`/books/book-1`
section|`/books`
taxonomy|`/tags`
term|`/tags/foo`

url
: (`string` 字符串类型)  *外部* 链接必须；

### 示例 在网站配置中定义菜单 {#example-site-configuration}

此嵌套菜单演示了一些可用属性：

{{< code-toggle file="hugo" copy=false >}}
[[menu.main]]
name = 'Products'
pageRef = '/products'
weight = 10

[[menu.main]]
name = 'Hardware'
pageRef = '/products/hardware'
parent = 'Products'
weight = 1

[[menu.main]]
name = 'Software'
pageRef = '/products/software'
parent = 'Products'
weight = 2

[[menu.main]]
name = 'Services'
pageRef = '/services'
weight = 20

[[menu.main]]
name = 'Hugo'
pre = '<i class="fa fa-heart"></i>'
url = 'https://gohugo.io/'
weight = 30
[menu.main.params]
rel = 'external'
{{< /code-toggle >}}

这将创建一个菜单结构，您可以使用模板中的`site.Menus.main`访问该结构。有关详细信息，请参见[菜单模板][menu templates]。



## 多语言站点的本地化菜单支持

Hugo提供了两种本地化语言菜单项的方法。见 [多语言支持][multilingual].

## 渲染

见 [菜单模板][menu templates].

[localize]: /content-management/multilingual/#menus
[menu templates]: /templates/menu-templates/
[multilingual]: /content-management/multilingual/#menus
[template]: /templates/menu-templates/
