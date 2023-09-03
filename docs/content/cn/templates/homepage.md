---
title: 首页模板
description: 网站主页的格式通常与其他页面不同。出于这个原因，Hugo让你很容易将新网站的主页定义为一个独特的模板。
categories: [templates]
keywords: [homepage]
menu:
  docs:
    parent: templates
    weight: 70
weight: 70
aliases: [/layout/homepage/,/templates/homepage-template/]
toc: true
---

主页也是`页面 Page`，因此可以调用和访问所有可供使用的[页面变量page variables][pagevars]和[站点变量site variables][站点变量sitevars]。

{{% note %}}
主页模板是构建网站所需的唯一模板，因此在引导新网站和模板时非常有用。如果你正在开发一个单页网站，它也是唯一需要的模板。
{{% /note %}}

{{< youtube ut1xtRZ1QOA >}}

## 主页模板查找顺序

查看 [模板查找顺序 Template Lookup](/templates/lookup-order/).

## 给网站首页增加内容和定义头信息

网站首页，和其他的 [列表页 list pages in Hugo][lists]一样， 内容和头信息都来自一个  `_index.md`  文件。这个文件需要在你的`content` 目录的根目录之下。(比如 `content/_index.md`)。然后，您可以像添加任何其他内容文件一样，将正文副本和元数据添加到主页中。

请参阅下面的主页模板或[内容组织 Content Organization][contentorg] 来查看更多信息，以及给比如 `_index.md`  和列表页添加内容、头信息的方式。

## 首页模板示例


下面是一个使用了 [子模板 partial][partials], [基础模板][base] 以及文档内容 `content/_index.md` 定义， 并且引用了 `{{ .Title }}` 和 `{{ .Content }}` [ 页面变量 page variables][pagevars]的示例：

{{< code file="layouts/index.html" >}}
{{ define "main" }}
  <main aria-role="main">
    <header class="homepage-header">
      <h1>{{ .Title }}</h1>
      {{ with .Params.subtitle }}
        <span class="subtitle">{{ . }}</span>
      {{ end }}
    </header>
    <div class="homepage-content">
      <!-- Note that the content for index.html, as a sort of list page, will pull from content/_index.md -->
      {{ .Content }}
    </div>
    <div>
      {{ range first 10 .Site.RegularPages }}
        {{ .Render "summary" }}
      {{ end }}
    </div>
  </main>
{{ end }}
{{< /code >}}

[base]: /templates/base/
[contentorg]: /content-management/organization/
[lists]: /templates/lists/
[lookup]: /templates/lookup-order/
[pagevars]: /variables/page/
[partials]: /templates/partials/
[sitevars]: /variables/site/
