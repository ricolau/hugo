---
title: 菜单模板
description: 使用模板中的菜单变量和方法来渲染菜单。
categories: [templates]
keywords: [lists,sections,menus]
menu:
  docs:
    parent: templates
    weight: 140
toc: true
weight: 140
aliases: [/templates/menus/]
---

## 概览 Overview

在[定义菜单条目][defining menu entries]之后，使用[菜单变量和方法][menu variables and methods] 来渲染一个菜单。

三个因素来决定如何渲染一个菜单：

1. 定义菜单中条目的方法在 [automatic], [in front matter],或者 [in site configuration]中。
2. 菜单结构: 平铺或者嵌套
3. 使用 [localize the menu entries] 来定义: 站点配置或翻译表

下面的示例处理每种组合。

## 示例 Example

这个部分模板递归地“遍历”一个菜单结构，呈现一个本地化的、可访问的嵌套列表。

{{< code file="layouts/partials/menu.html" >}}
{{- $page := .page }}
{{- $menuID := .menuID }}

{{- with index site.Menus $menuID }}
  <nav>
    <ul>
      {{- partial "inline/menu/walk.html" (dict "page" $page "menuEntries" .) }}
    </ul>
  </nav>
{{- end }}

{{- define "partials/inline/menu/walk.html" }}
  {{- $page := .page }}
  {{- range .menuEntries }}
    {{- $attrs := dict "href" .URL }}
    {{- if $page.IsMenuCurrent .Menu . }}
      {{- $attrs = merge $attrs (dict "class" "active" "aria-current" "page") }}
    {{- else if $page.HasMenuCurrent .Menu .}}
      {{- $attrs = merge $attrs (dict "class" "ancestor" "aria-current" "true") }}
    {{- end }}
    <li>
      <a
        {{- range $k, $v := $attrs }}
          {{- with $v }}
            {{- printf " %s=%q" $k $v | safeHTMLAttr }}
          {{- end }}
        {{- end -}}
      >{{ or (T .Identifier) .Name | safeHTML }}</a>
      {{- with .Children }}
        <ul>
          {{- partial "inline/menu/walk.html" (dict "page" $page "menuEntries" .) }}
        </ul>
      {{- end }}
    </li>
  {{- end }}
{{- end }}
{{< /code >}}

调用上面的子模板，在上下文中传递菜单ID和当前页面。

{{< code file="layouts/_default/single.html" >}}
{{ partial "menu.html" (dict "menuID" "main" "page" .) }}
{{ partial "menu.html" (dict "menuID" "footer" "page" .) }}
{{< /code >}}

## Page references

无论您如何[定义菜单项][define menu entries]，与页面关联的项都可以访问页面变量和方法。
这个简单的示例在每个条目的`name`旁边呈现一个名为`version`的页面参数。代码防御性地使用 `with`或 `if` 来处理条目，其中（a）条目指向外部资源，或（b）未定义`version`参数。


{{< code file="layouts/_default/single.html" >}}
{{- range site.Menus.main }}
  <a href="{{ .URL }}">
    {{ .Name }}
    {{- with .Page }}
      {{- with .Params.version -}}
        ({{ . }})
      {{- end }}
    {{- end }}
  </a>
{{- end }}
{{< /code >}}

## Menu entry parameters

When you define menu entries [in site configuration] or [in front matter], you can include a `params` key as shown in these examples:

- [Menu entry defined in site configuration]
- [Menu entry defined in front matter]

This simplistic example renders a `class` attribute for each anchor element. Code defensively using `with` or `if` to handle entries where `params.class` is not defined.

{{< code file="layouts/partials/menu.html" >}}
{{- range site.Menus.main }}
  <a {{ with .Params.class -}} class="{{ . }}" {{ end -}} href="{{ .URL }}">
    {{ .Name }}
  </a>
{{- end }}
{{< /code >}}

## Localize

Hugo provides two methods to localize your menu entries. See [multilingual].

[automatic]: /content-management/menus/#define-automatically
[define menu entries]: /content-management/menus/
[defining menu entries]: /content-management/menus/
[in front matter]: /content-management/menus/#define-in-front-matter
[in site configuration]: /content-management/menus/#define-in-site-configuration
[localize the menu entries]: /content-management/multilingual/#menus
[menu entry defined in front matter]: /content-management/menus/#example-front-matter
[menu entry defined in site configuration]: /content-management/menus/#example-site-configuration
[menu variables and methods]: /variables/menus/
[multilingual]: /content-management/multilingual/#menus
