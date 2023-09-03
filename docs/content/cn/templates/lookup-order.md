---
title: 模板渲染的优先级规则
description: Hugo使用以下规则为给定页面选择模板，从最具体的开始。
categories: [fundamentals,templates]
keywords: [templates]
menu:
  docs:
    parent: templates
    weight: 30
weight: 30
toc: true
---

## 渲染时优先级规则 Lookup rules

Hugo在为给定页面选择模板时会考虑以下参数。模板按特定性排序。这应该感觉很自然，具体请查看下表中不同参数变化的具体示例。

种类 Kind
: 不同的 `种类Kind` (主页是其中之一)。 下方示例表格中每一种分类都有展示。这也决定了页面是否是一个 **独立页面** (比如一个独立的页面，我们一般会去 `_default/single.html` 这个路径下寻找模板来渲染) 还是一个 **列表页** (子段落列表、主页、分类列表、分类条目，我们在这个路径下去 `_default/list.html` 寻找列表页的模板)。

布局 Layout
: 可以在 页面头信息中设置

输出形式 Output Format
: 查看 [自定义输出形式 Custom Output Formats](/templates/output-formats)。每种输出形式都有一个 `name`字段 (比如 `rss`, `amp`, `html`) 和一个后缀 `suffix` (比如 `xml`, `html`)。我们倾向于两者都进行匹配 (比如 `index.amp.html`, 而不是去匹配一个不太具体的模板。

请注意，如果输出格式的“媒体类型”定义了多个后缀，则只考虑第一个后缀。

语言 Language
: 我们将在模板名称中考虑一个语言标记。如果网站语言是`fr`，`index.fr.amp.html`将比 `index.amp.html` 的优先级更高，但`index.amp.html`将先于`index.fr.html`选择。


类型 Type
: 如果在页面头信息中已经设置过了`type`的值则使用头信息中设置的，否则为根节的名称（例如"blog"）。它总是有一个值，所以如果不设置，则该值为“page”。

部分 Section
: 与 `section`、 `taxonomy`和 `term`类型相关。

{{% note %}}
模板可以位于项目或主题的布局文件夹中，Hugo将选择最匹配的那一个模板。Hugo在下面列出的列表中依次交错查找，直到找到项目或主题中最匹配的那一个模板。
{{% /note %}}

## 主页 Home page

{{< datatable-filtered "output" "layouts" "Kind == home" "Example" "OutputFormat" "Suffix" "Template Lookup Order" >}}

## 独立页面 Single pages

{{< datatable-filtered "output" "layouts" "Kind == page" "Example" "OutputFormat" "Suffix" "Template Lookup Order" >}}

## 子节点页面 Section pages

A section page is a list of pages within a given section.

{{< datatable-filtered "output" "layouts" "Kind == section" "Example" "OutputFormat" "Suffix" "Template Lookup Order" >}}

## 分类页面 Taxonomy pages

A taxonomy page is a list of terms within a given taxonomy. The examples below assume the following site configuration:

{{< code-toggle file=hugo copy=false >}}
[taxonomies]
category = 'categories'
{{< /code-toggle >}}

{{< datatable-filtered "output" "layouts" "Kind == taxonomy" "Example" "OutputFormat" "Suffix" "Template Lookup Order" >}}

## 条目页面 Term pages

A term page is a list of pages associated with a given term. The examples below assume the following site configuration:

{{< code-toggle file=hugo copy=false >}}
[taxonomies]
category = 'categories'
{{< /code-toggle >}}

{{< datatable-filtered "output" "layouts" "Kind == term" "Example" "OutputFormat" "Suffix" "Template Lookup Order" >}}
