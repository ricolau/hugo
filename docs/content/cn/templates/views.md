---
title: 内容展示模板
description: Hugo可以根据内容展示模板渲染出内容的具体页面样式，在列表视图和摘要视图中很有用。
categories: [templates]
keywords: [views]
menu:
  docs:
    parent: templates
    weight: 110
weight: 110
toc: true
---

这些备选**内容模板**在[列表模板][lists]中特别有用。

以下是内容渲染模板的常见用例：

* 您希望每种类型的内容都显示在主页上，但只能显示有限的[摘要视图][summaries].
* 您只想在[分类列表页][taxonomylists]上显示内容的项目符号列表。模板视图通过将每种不同类型的内容的呈现委派给内容本身来实现这一点。

## 创建换一个内容模板 Create a content view

要创建新内容模板，请在每个不同的内容类型目录中创建一个具有视图名称的模板。 以下示例包含 `posts` 和 `project`内容类型的“li”视图和“summary”视图。

正如您所看到的，这些位于[单一内容模板 single content view][single] 模板`single.html`旁边。您甚至可以为给定类型提供特定的视图，并继续使用 `_default/single.html`作为主模板。


```txt
  ▾ layouts/
    ▾ posts/
        li.html
        single.html
        summary.html
    ▾ project/
        li.html
        single.html
        summary.html
```
Hugo还支持在没有为该类型提供特定内容视图模板的情况下使用默认内容模板。内容模板也可以在`_default` 目录中定义，其工作方式与列表和单个模板相同，这些模板最终会根据查找顺序渗入`_default`目录。


```txt
▾ layouts/
  ▾ _default/
      li.html
      single.html
      summary.html
```

## 哪个模板将会被渲染？Which template will be rendered?

下面是内容模板的 [查找和渲染顺序lookup order][lookup]：

1. `/layouts/<TYPE>/<VIEW>.html`
2. `/layouts/_default/<VIEW>.html`
3. `/themes/<THEME>/layouts/<TYPE>/<VIEW>.html`
4. `/themes/<THEME>/layouts/_default/<VIEW>.html`

## 示例: 内容模板在一个列表中 content view inside a list

以下示例演示了如何在[列表模板][lists]中使用内容视图。

### `list.html`

在本例中，将`.Render`传递到模板中以调用[Render函数][Render]。
`.Render`是一个特殊函数，它指示内容使用作为第一个参数提供的视图模板进行自身呈现。在这种情况下，模板将呈现如下的`summary.html` 视图：

{{< code file="layouts/_default/list.html" >}}
<main id="main">
  <div>
    <h1 id="title">{{ .Title }}</h1>
    {{ range .Pages }}
      {{ .Render "summary" }}
    {{ end }}
  </div>
</main>
{{< /code >}}

### `summary.html`

Hugo将把整个页面对象传递给下面的`summary.html`视图模板。(见 [页面变量][pagevars] 中的全部内容列表)

{{< code file="layouts/_default/summary.html" >}}
<article class="post">
  <header>
    <h2><a href='{{ .Permalink }}'> {{ .Title }}</a> </h2>
    <div class="post-meta">{{ .Date.Format "Mon, Jan 2, 2006" }} - {{ .FuzzyWordCount }} Words </div>
  </header>
  {{ .Summary }}
  <footer>
  <a href='{{ .Permalink }}'><nobr>Read more →</nobr></a>
  </footer>
</article>
{{< /code >}}

### `li.html`

继续前面的示例，我们可以通过更改对`.Render` 函数的调用中的参数来更改渲染render函数，以使用较小的`li.html`视图（比如 `{{ .Render "li" }}`）。


{{< code file="layouts/_default/li.html" >}}
<li>
  <a href="{{ .Permalink }}">{{ .Title }}</a>
  <div class="meta">{{ .Date.Format "Mon, Jan 2, 2006" }}</div>
</li>
{{< /code >}}

[lists]: /templates/lists/
[lookup]: /templates/lookup-order/
[pagevars]: /variables/page/
[render]: /functions/render/
[single]: /templates/single-page-templates/
[spf]: https://spf13.com
[spfsourceli]: https://github.com/spf13/spf13.com/blob/master/layouts/_default/li.html
[spfsourcesection]: https://github.com/spf13/spf13.com/blob/master/layouts/_default/section.html
[spfsourcesummary]: https://github.com/spf13/spf13.com/blob/master/layouts/_default/summary.html
[summaries]: /content-management/summaries/
[taxonomylists]: /templates/taxonomy-templates/
