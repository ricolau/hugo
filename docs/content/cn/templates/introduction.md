---
title: 模板
linkTitle: Templating
description: Hugo 使用go语言的 `html/template` 和 `text/template` 库作为模板引擎的基础支撑。
categories: [fundamentals,templates]
keywords: [go]
menu:
  docs:
    parent: templates
    weight: 20
weight: 20
aliases: [/layouts/introduction/,/layout/introduction/, /templates/go-templates/]
toc: true
---

{{% note %}}
以下只是Go Templates的入门教程。要深入了解Go模板，请查看官方的[Go docs](https://golang.org/pkg/text/template/)。
{{% /note %}}

Go模板提供了一种极其简单的模板语言，它坚持这样一种信念，即只有最基本的逻辑才属于模板或展示层。

## 基础语法 Basic syntax

Go模板是添加了[variables][variables]和[functions][functions]标签的HTML文件。Go Template变量和函数可以使用`{{ }}`标签访问。

### 使用一个已经定义好的变量

一个已经定义好的变量应该是在当前作用域内已经存在并生效的（就像下面这段儿[Variables](#variables) 中 `.Title` 的示例一样）变量，或者是一个自定义的变量（比如同样下面段落中的 `$address` 示例）。

```go-html-template
{{ .Title }}
{{ $address }}
```

函数的参数使用空格分隔。一般语法为：

```go-html-template
{{ FUNCTION ARG1 ARG2 .. }}
```

下方的示例是调用`add`这个 funciton 的时候，传入 `1` 和 `2` 这两个参数：

```go-html-template
{{ add 1 2 }}
```

#### 方法和字段通过点 `.` 表示法访问

使用在页面[文件头][front matter]中已经定义的 `bar` 参数：

```go-html-template
{{ .Params.bar }}
```

#### 括号可用于将项目组合在一起

```go-html-template
{{ if or (isset .Params "alt") (isset .Params "caption") }} Caption {{ end }}
```

#### 单个语句可以拆分为多行

```go-html-template
{{ if or
  (isset .Params "alt")
  (isset .Params "caption")
}}
```

#### 原始字符串文字可以包括换行符

```go-html-template
{{ $msg := `Line one.
Line two.` }}
```

## 变量

每个Go 模板都会获得一个数据对象。在Hugo中，每个模板都被传递和定义一个 `Page` 参数。
在下方的示例中，`.Title` 是在[`Page` variable][pagevars] 变量列表中的一个可以访问和使用的变量。
`Page`是一个模板的默认生效范围, 因此在这个范围内访问 `Title` 就可以使用带有 点 `.`  的前缀比如  `.Title` 就可以了.

```go-html-template
<title>{{ .Title }}</title>
```

值也可以存储在自定义变量中，并在以后引用：

{{% note %}}
自定义变量需要加前缀 `$`.
{{% /note %}}

```go-html-template
{{ $address := "123 Main St." }}
{{ $address }}
```

变量可以被 `=` 操作符进行重新赋值。下面的示例会打印“Var is Hugo Home”在 首页 中，并且在其在其他页面中打印 “Var is Hugo Page”：

```go-html-template
{{ $var := "Hugo Page" }}
{{ if .IsHome }}
    {{ $var = "Hugo Home" }}
{{ end }}
Var is {{ $var }}
```

## 函数 Funtions

Go模板只附带了一些基本功能，但也为应用程序提供了一种扩展原始集合的机制。

[Hugo 模板函数 template functions][functions] 提供特定于构建网站的附加功能。函数的调用方法是使用它们的名称，后跟用空格分隔的必需参数。如果不重新编译Hugo，就无法添加模板函数。

### Example 1: 数字加和

```go-html-template
{{ add 1 2 }}
<!-- prints 3 -->
```

### Example 2: 数字比较

```go-html-template
{{ lt 1 2 }}
<!-- prints true (i.e., since 1 is less than 2) -->
```
注意，以上两个示例都是 Go Template 模板默认支持的 [数学计算 math][math] 的函数。

{{% note %}}
在[Go Template documentation](https://golang.org/pkg/text/template/#hdr-Functions) 文档中列举了比Hugo 文档中更多的布尔运算方式可以参考。
{{% /note %}}

## 包含标签 Includes

当包含另一个模板时，您需要将它需要访问的数据传递给它。

{{% note %}}
要传递当前上下文，请记住包含一个尾随的 **.** 。
{{% /note %}}

模板位置始终在 Hugo中的`layouts` 目录中，并以 `layout/` 路径开头。

### 子模板标签 Partial

The [子模板 `partial`][partials] 功能用来引入子模板，使用的方式为 `{{ partial "<PATH>/<PARTIAL>.<EXTENSION>" . }}`。

比如引入一个路径为 `layouts/partials/header.html` 的子模板:

```go-html-template
{{ partial "header.html" . }}
```

### 模板标签 Template

`template` 标签在一些老的历史版本中是用来使用“子模板”的。但是现在它只用来调用 
[内部模板 _internal_ templates][internal templates]. 语法为 `{{ template
"_internal/<TEMPLATE>.<EXTENSION>" . }}`.


{{% note %}}
可以用的 **internal** 模板可以去这里找：
[here](https://github.com/gohugoio/hugo/tree/master/tpl/tplimpl/embedded/templates).
{{% /note %}}

包含 internal 的 `opengraph.html` 模板示例：

```go-html-template
{{ template "_internal/opengraph.html" . }}
```

## 逻辑条件 Logic

Go模板提供了最基本的迭代和条件逻辑。

### 迭代/遍历 Iteration

Go 模板 Templates 重度使用 `range` 来迭代和遍历 _map_,
_array_, 或 _slice_ 。 接下来是几个不同的适合用 `range`的示例：

#### Example 1: 使用上下文 (`.`)

```go-html-template
{{ range $array }}
    {{ . }} <!-- 这个 . 代表了 $array 中的一个元素-->
{{ end }}
```

#### Example 2: 为 array 中的一个元素声明一个变量名

```go-html-template
{{ range $elem_val := $array }}
    {{ $elem_val }}
{{ end }}
```

#### Example 3: 为一个 array 中元素的索引 index、值 value 分别声明变量名


对于数组或切片，第一个声明的变量将映射到每个元素的index。

```go-html-template
{{ range $elem_index, $elem_val := $array }}
   {{ $elem_index }} -- {{ $elem_val }}
{{ end }}
```

#### Example 4: 为map元素的键、值分别声明变量名

对于map类型，第一个声明的变量将映射到每个映射元素的key。

```go-html-template
{{ range $elem_key, $elem_val := $map }}
   {{ $elem_key }} -- {{ $elem_val }}
{{ end }}
```

#### Example 5: 对于 空map、空 array、空slice 的判断和使用：

如果参数传递过来的 map、array、slice 是空的，`else` 中的内容会被执行：

```go-html-template
{{ range $array }}
    {{ . }}
{{ else }}
    <!-- This is only evaluated if $array is empty -->
{{ end }}
```

### 条件判断 Conditionals

Go Templates 中的 `if`, `else`, `with`, `or`, `and` 和 `not` 为Hugo 提供了条件逻辑操作能力。比如 `range`, `if` 和 `with` 等操作符需要使用 `{{ end }}` 来闭合。

Go Templates 对以下值得处理为  **false**:

- `false` (boolean)
- 0 (integer)
- any zero-length array, slice, map, or string

#### Example 1: `with`


`with` 一般用来表示“如果这个变量存在，那么就执行以下逻辑”：

{{% note %}}
`with` 会重新绑定`.`的上下文到自己的作用域内 (就像 `range` 一样)。
{{% /note %}}

如果变量不存在，或者如上所述计算结果为“false”，则跳过块。

```go-html-template
{{ with .Params.title }}
    <h4>{{ . }}</h4>
{{ end }}
```

#### Example 2: `with` .. `else`

下面的代码的逻辑为：如果在页面“头信息”中定义了 "description" 变量的值，就使用它；否则使用[页面变量][pagevars]中的`.Summary` 变量的值。

```go-html-template
{{ with .Param "description" }}
    {{ . }}
{{ else }}
    {{ .Summary }}
{{ end }}
```

查看 [`.Param` function][param].

#### Example 3: `if`

写“with”的另一种（也是更详细的）方式是使用“if”。比如：

下面的代码是使用 `if` 的方式重写了 "Example 1" 的实现：

```go-html-template
{{ if isset .Params "title" }}
    <h4>{{ index .Params "title" }}</h4>
{{ end }}
```

#### Example 4: `if` .. `else`

下面是 "Example 2" 采用 `if` .. `else`进行重写并且使用
[`isset` function][isset] + `.Params` 访问变量的实现。 (与
[`.Param` **function**][param]不同) :

```go-html-template
{{ if (isset .Params "description") }}
    {{ index .Params "description" }}
{{ else }}
    {{ .Summary }}
{{ end }}
```

#### Example 5: `if` .. `else if` .. `else`

与 `with`不同，  `if` 可以包含 `else if` 分支配合：

```go-html-template
{{ if (isset .Params "description") }}
    {{ index .Params "description" }}
{{ else if (isset .Params "summary") }}
    {{ index .Params "summary" }}
{{ else }}
    {{ .Summary }}
{{ end }}
```

#### Example 6: `and` & `or`

```go-html-template
{{ if (and (or (isset .Params "title") (isset .Params "caption")) (isset .Params "attr")) }}
```

## 管道传参 Pipes

Go Templates最强大的组件之一是能够把一个接一个地动作函数进行连续堆叠。这是通过使用管道来完成的。这个概念借用了Unix管道，很简单：每个管道的输出都成为下面管道的输入。

由于Go Templates的语法非常简单，管道对于能够将函数调用链接在一起至关重要。管道的一个限制是，它们只能使用单个值，并且该值将成为下一个管道的最后一个参数。

几个简单的例子应该有助于传达如何使用管道。

### Example 1: `shuffle`

以下两个示例在功能上相同：

```go-html-template
{{ shuffle (seq 1 5) }}
```


```go-html-template
{{ (seq 1 5) | shuffle }}
```

### Example 2: `index`

下面访问名为“disqus_url”的页面参数并转义HTML。此示例还使用[`index`函数]（/functions/index function/），该函数内置于Go Templates中：

```go-html-template
{{ index .Params "disqus_url" | html }}
```

### Example 3: `or` with `isset`

```go-html-template
{{ if or (or (isset .Params "title") (isset .Params "caption")) (isset .Params "attr") }}
Stuff Here
{{ end }}
```

等价于以下代码：

```go-html-template
{{ if isset .Params "caption" | or isset .Params "title" | or isset .Params "attr" }}
Stuff Here
{{ end }}
```

## 上下文访问（即 “点”访问） Context (aka "the dot") {#the-dot}

在Go Templates 中最容易被忽视的概念就是 `{{ . }}`，这个永远表示 **当前上下文**。

- 在模板的顶层，这将是可供其使用的数据集。
-然而，在迭代中，它将具有
循环中的当前项目；即 `{{ . }}` 将不再指代
整个页面可用的数据。

如果在循环结构中你想访问页面级别的变量或者参数
（比如`页面头信息` parameters 中设置的参数） ，你需要按照下方的方法其中之一做：

### 1.定义一个独立于当前上下文的变量

下面展示了如何定义独立于上下文的变量。

{{< code file="tags-range-with-page-variable.html" >}}
{{ $title := .Site.Title }}
<ul>
{{ range .Params.tags }}
    <li>
        <a href="/tags/{{ . | urlize }}">{{ . }}</a>
        - {{ $title }}
    </li>
{{ end }}
</ul>
{{< /code >}}

{{% note %}}
请注意，一旦我们进入循环（比如 `range` 中）， `{{ . }}` 的值就会发生变化。我们在循环外定义了一个变量（`{{ $title }}`），并为其赋值，以便我们也可以从循环内访问该值。

{{% /note %}}

### 2. 使用 `$.` 来访问全局上下文中的变量

`$` 在模板中有特殊意义。 `$` 默认为使用 `.` 时的前置初始符号。 这是 [Go text/template 的文档特性][dotdoc]。这意味着你可以在任何地方访问全局上下文/变量。 下面演示如何在代码块中使用 `$` 来访问全局上下文/变量中的`.Site.Title` 属性。

{{< code file="range-through-tags-w-global.html" >}}
<ul>
{{ range .Params.tags }}
  <li>
    <a href="/tags/{{ . | urlize }}">{{ . }}</a>
            - {{ $.Site.Title }}
  </li>
{{ end }}
</ul>
{{< /code >}}

{{% note "不要给 `.` 做重新赋值" %}}
如果有人恶意地重新定义这个特殊角色，`$` 的内在魔力就会失效；例如 `{{ $ := .Site }}`*不要这么做。*当然，您可以通过在全局上下文中使用`{{ $ := . }}` 将 `$` 重置为默认值来从这种危害中恢复过来。

{{% /note %}}

## 空格 Whitespace

Go 1.6包括通过在相应的 `{{` 或 `}}`分隔符旁边包括连字符（`-`）和空格来修剪Go标记两侧的空白的能力。

例如，下面的Go Template将在其HTML输出中包括换行符和水平选项卡：

```go-html-template
<div>
  {{ .Title }}
</div>
```

将输出:

```html
<div>
  Hello, World!
</div>
```
利用以下示例中的 `-` 将删除`.Title`变量周围的多余空白，并删除换行符：

```go-html-template
<div>
  {{- .Title -}}
</div>
```

将输出:

```html
<div>Hello, World!</div>
```

Go会将以下内容页视为空格：

* <kbd>space</kbd>
* horizontal <kbd>tab</kbd>
* carriage <kbd>return</kbd>
* newline

## 注释
为了保持模板的有序性并在整个团队中共享信息，您可能需要在模板中添加注释。对Hugo来说，有两种方法可以做到这一点。


### Go templates 注释

转到模板支持`{{/*` 和`*/}}` 以打开和关闭注释块。该块中的任何内容都不会被渲染。

例如：

```go-html-template
Bonsoir, {{/* {{ add 0 + 2 }} */}}Eliott.
```

将渲染 `Bonsoir, Eliott.`，将忽略注释木块中的 (`add 0 + 2`) 内容。

### HTML 注释

您可以通过将字符串html代码注释管道连接到`safeHTML`来添加html注释。

例如:

```go-html-template
{{ "<!-- This is an HTML comment -->" | safeHTML }}
```

如果您需要变量来构造这样的HTML注释，可以使用`printf`管道传参给 `safeHTML`.

例如:

```go-html-template
{{ printf "<!-- Our website is named: %s -->" .Site.Title | safeHTML }}
```

#### HTML 注释里包含 Go templates

默认情况下，HTML注释会被剥离，但它们的内容仍会被执行。这意味着，尽管HTML注释永远不会向最终的HTML页面呈现任何内容，但注释中包含的代码可能会使构建过程失败。

{{% note %}}
不要尝试使用HTML注释注释掉Go Template代码。
{{% /note %}}

```go-html-template
<!-- {{ $author := "Emma Goldman" }} was a great woman. -->
{{ $author }}
```

模板引擎将剥离HTML注释中的内容，但将首先评估和执行Go Template中的任何代码（如果存在）。 因此，上面的示例将呈现`Emma Goldman`，因为`$author`变量是在HTML注释中求值的。但是，如果HTML注释中的代码出现错误，那么构建就会失败。

## Hugo 定义的参数使用


Hugo提供了通过[站点配置][config]（比如针对网站全范围的配置复制）或通过每个特定内容的元数据（即[页面头信息][front matter]）将值传递到模板层的选项。

您可以定义任何类型的任何值，并在模板中使用它们，只要这些值受[页面头信息格式][front matter format](/content-management/front-matter#front-matter-formats)支持即可。

## 使用页面级别 (`Page`) 中的变量内容

你可以在每一个独立的内容模板的[头信息][front matter]中定义变量。

Hugo文档中使用了一个例子。大多数页面都受益于提供了目录，但有时目录没有多大意义。我们在页面头信息[front matter]中定义了一个 `notoc` 变量，该变量将阻止目录在其设置为“true”时被渲染。


以下是对应的头信息示例:

{{< code-toggle file="content/example.md" fm=true copy=false >}}
title: Example
notoc: true
{{< /code-toggle >}}

以下是可以在 `toc.html` [子模板][partials]中使用的相应代码：

{{< code file="layouts/partials/toc.html" >}}
{{ if not .Params.notoc }}
<aside>
  <header>
    <a href="#{{ .Title | urlize }}">
    <h3>{{ .Title }}</h3>
    </a>
  </header>
  {{ .TableOfContents }}
</aside>
<a href="#" id="toc-toggle"></a>
{{ end }}
{{< /code >}}

我们希望页面渲染时默认会渲染` {{ .TableOfContents }}` 中的内容，除非被指定了不渲染。而这里指定不渲染的方式就是检查当前页面模板渲染时页面头信息中设置的 `notoc:` 的值确定不是 `true`。

## 使用网站全局配置参数

只要你想，你可以定义任意多的 网站级别的参数，在 [网站配置文件中 ite's configuration file][config]. 这些参数将在全局所有模板中生效。

比如，你可能有如下定义：

{{< code-toggle file="hugo" >}}
params:
  copyrighthtml: "Copyright &#xA9; 2017 John Doe. All Rights Reserved."
  twitteruser: "spf13"
  sidebarrecentlimit: 5
{{< /code >}}

在 footer渲染布局中，你可能定义只有当 `copyrighthtml` 参数已经声明赋值之后，才来渲染你的`<footer>` 布局。
如果这个参数确实有值，则需要通过[`safeHTML`函数][safeHTML]声明该字符串可以安全使用，这样HTML实体就不会再次转义。
这将使您能够在每年1月1日轻松更新您的顶级配置文件，而不是在模板中搜寻。

```go-html-template
{{ if .Site.Params.copyrighthtml }}
    <footer>
        <div class="text-center">{{ .Site.Params.CopyrightHTML | safeHTML }}</div>
    </footer>
{{ end }}
```

一种替代"`if`" 的方法就是使用 [`with`][with]来判断一个参数的值是否存在。如果有一个参数是全局参数，`with`会重新绑定(`.`)所代表的上下文到它自己的代码块范围内：

{{< code file="layouts/partials/twitter.html" >}}
{{ with .Site.Params.twitteruser }}
    <div>
        <a href="https://twitter.com/{{ . }}" rel="author">
        <img src="/images/twitter.png" width="48" height="48" title="Twitter: {{ . }}" alt="Twitter"></a>
    </div>
{{ end }}
{{< /code >}}

最后，您还可以从布局中提取“魔术常量`magic constants`”。
下面是使用[`first`][first] 函数，以及[`.RelPermalink`][relpermalink] 来使用页面变量，和[`.Site.Pages`][sitevars] 来访问网站全局变量的方式。


```go-html-template
<nav>
  <h1>Recent Posts</h1>
  <ul>
  {{- range first .Site.Params.SidebarRecentLimit .Site.Pages -}}
      <li><a href="{{ .RelPermalink }}">{{ .Title }}</a></li>
  {{- end -}}
  </ul>
</nav>
```

## Example: show future events 未来事件

定义如下文档结构和内容在 [页面头信息][front matter]中：

```text
content/
└── events/
    ├── event-1.md
    ├── event-2.md
    └── event-3.md
```

{{< code-toggle file="content/events/event-1.md" copy=false >}}
title = 'Event 1'
date = 2021-12-06T10:37:16-08:00
draft = false
start_date = 2021-12-05T09:00:00-08:00
end_date = 2021-12-05T11:00:00-08:00
{{< /code-toggle >}}

这个 [子模板 partial template][partials] 将渲染未来的事件：

{{< code file="layouts/partials/future-events.html" >}}
<h2>Future Events</h2>
<ul>
  {{ range where site.RegularPages "Type" "events" }}
    {{ if gt (.Params.start_date | time.AsTime) now }}
      {{ $startDate := .Params.start_date | time.Format ":date_medium" }}
      <li>
        <a href="{{ .RelPermalink }}">{{ .LinkTitle }}</a> - {{ $startDate }}
      </li>
    {{ end }}
  {{ end }}
</ul>
{{< /code >}}

如果将[页面头信息][front matter]限制为TOML格式，并省略日期字段周围的引号，则可以在不强制转换的情况下执行日期比较。

{{< code file="layouts/partials/future-events.html" >}}
<h2>Future Events</h2>
<ul>
  {{ range where (where site.RegularPages "Type" "events") "Params.start_date" "gt" now }}
    {{ $startDate := .Params.start_date | time.Format ":date_medium" }}
    <li>
      <a href="{{ .RelPermalink }}">{{ .LinkTitle }}</a> - {{ $startDate }}
    </li>
  {{ end }}
</ul>
{{< /code >}}

[dotdoc]: https://golang.org/pkg/text/template/#hdr-Variables
[config]: /getting-started/configuration
[first]: /functions/first
[front matter]: /content-management/front-matter
[functions]: /functions
[internal templates]: /templates/internal
[isset]: /functions/isset
[math]: /functions/math
[pagevars]: /variables/page
[param]: /functions/param
[partials]: /templates/partials
[relpermalink]: /variables/page#page-variables
[safehtml]: /functions/safehtml
[sitevars]: /variables/site
[variables]: /variables
[with]: /functions/with
