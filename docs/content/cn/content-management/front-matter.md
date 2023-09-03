---
title: 头信息
description: Hugo支持使用yaml、toml、或者json 格式在你的内容文件中添加头信息
categories: [content management]
keywords: ["头信息", "yaml", "toml", "json", "元数据", "内容类型"]
menu:
  docs:
    parent: content-management
    weight: 60
toc: true
weight: 60
aliases: [/content/front-matter/]
---

**头信息 Front matter** 支持你在一个特定类型[content type]的内容中添加元数据 ---比如：内嵌在一个内容文件中---这是Hugo众多强大的功能特性中的一个。

{{< youtube Yh2xKRJGff4 >}}

## 头信息格式 Front matter formats

Hugo支持4种格式的头信息，每一种都需要用他们特定的方式来定义：

TOML
: 开始和闭合都是用 `+++` 符号。

YAML
: 开始和闭合都是用  `---` 符号。

JSON
: 一个简单的以  '`{`' 和 '`}`' 包围定义的JSON 对象，放在独立的一行中。

ORG
: a group of Org mode keywords in the format '`#+KEY: VALUE`'. Any line that does not start with `#+` ends the front matter section.
  Keyword values can be either strings (`#+KEY: VALUE`) or a whitespace separated list of strings (`#+KEY[]: VALUE_1 VALUE_2`).

### Example

{{< code-toggle >}}
title = "spf13-vim 3.0 release and new website"
description = "spf13-vim is a cross platform distribution of vim plugins and resources for Vim."
tags = [ ".vimrc", "plugins", "spf13-vim", "vim" ]
date = "2012-04-06"
categories = [
  "Development",
  "VIM"
]
slug = "spf13-vim-3-0-release-and-new-website"
{{< /code-toggle >}}

## 头信息中的变量 Front matter variables

### 预定义好的变量 Predefined

Hugo可以识别一些预定义的变量。请参阅[页面变量 Page Variables][pagevars]以更多了解如何在模板中调用这些预定义变量。

aliases
: 将在输出目录结构中创建的一个或多个别名（例如，重命名内容的之前使用的旧的发布路径）的数组。有关详细信息请参见[Aliases][alias]。


audio
: 指向与当前页面相关的音频文件的路径数组；由`opengraph`[内部模板internal template](/templates/internal) 用于填充`og:audio`。

cascade
: A map of front matter keys whose values are passed down to the page's descendants unless overwritten by self or a closer ancestor's cascade. See [Front Matter Cascade](#front-matter-cascade) for details.

date
: The datetime assigned to this page. This is usually fetched from the `date` field in front matter, but this behavior is configurable.

description
: 对当前内容的描述。

draft
: 如果这个值设置为，则渲染时对应的文章内容不会被渲染，除非在调用 `hugo`命令的时候传递  `--buildDrafts` 参数。

expiryDate
: 在这个日期后，Hugo不会再渲染对应的内容展示。已过期的内容不会再被渲染，除非在调用 `hugo`命令的时候传递  `--buildExpired` 参数。

headless
: If `true`, sets a leaf bundle to be [headless][headless-bundle].

images
: An array of paths to images related to the page; used by [internal templates](/templates/internal) such as `_internal/twitter_cards.html`.

isCJKLanguage
: If `true`, Hugo will explicitly treat the content as a CJK language; both `.Summary` and `.WordCount` work properly in CJK languages.

keywords
: 当前内容的关键词等元数据。

layout
: The layout Hugo should select from the [lookup order][lookup] when rendering the content. If a `type` is not specified in the front matter, Hugo will look for the layout of the same name in the layout directory that corresponds with a content's section. See [Content Types][content type].

lastmod
: 对应的内容最后被修改/更新的时间（时间格式为 datetime）；

linkTitle
: 用于创建指向内容的链接；如果设置，Hugo默认优先使用 `linkTitle`来创建链接而不是`title`。Hugo还可以[按`linkTitle`][bylinktitle]来排序内容列表。

markup
: **experimental**; specify `"rst"` for reStructuredText (requires`rst2html`) or `"md"` (default) for Markdown.

outputs
: 允许您指定特定用于当前内容的输出格式，更多可以参考 [输出格式output formats][outputs].

publishDate
: 如果这个值设置的是一个未来的时间，Hugo 在渲染内容网页时不会渲染这个网页，除非在执行`hugo`命令时使用 `--buildFuture`  参数。

resources
: Used for configuring page bundle resources. See [Page Resources][page-resources].

series
: An array of series this page belongs to, as a subset of the `series` [taxonomy](/content-management/taxonomies/); used by the `opengraph` [internal template](/templates/internal) to populate `og:see_also`.

slug
: Overrides the last segment of the URL path. Not applicable to section pages. See [URL Management](/content-management/urls/#slug) for details.

summary
: Text used when providing a summary of the article in the `.Summary` page variable; details available in the [content-summaries](/content-management/summaries/) section.

title
: 设置当前内容的标题。

type
: 内容的类型；如果没有在头信息中指定，则该值将自动从目录中派生和继承。

url
: 覆盖完整的URL 路径， 适用于常规页面和section页面。
 见 [URL Management](/content-management/urls/#url) for details.

videos
: An array of paths to videos related to the page; used by the `opengraph` [internal template](/templates/internal) to populate `og:video`.

weight
: 用来 [给列表中的内容排序][ordering]。低weight 值在排序时更靠前。所以，低weight 的内容将率先列出来或者展示出来。
如果需要设置weight，它的值不能是0，0 和没有设置的效果是一样的。

\<taxonomies\>
: Field name of the *plural* form of the index. See `tags` and `categories` in the above front matter examples. *Note that the plural form of user-defined taxonomies cannot be the same as any of the predefined front matter variables.*

{{% note %}}
If neither `slug` nor `url` is present and [permalinks are not configured otherwise in your site configuration file](/content-management/urls/#permalinks), Hugo will use the file name of your content to create the output URL. See [Content Organization](/content-management/organization) for an explanation of paths in Hugo and [URL Management](/content-management/urls/) for ways to customize Hugo's default behaviors.
{{% /note %}}

### User-defined

You can add fields to your front matter arbitrarily to meet your needs. These user-defined key-values are placed into a single `.Params` variable for use in your templates.

The following fields can be accessed via `.Params.include_toc` and `.Params.show_comments`, respectively. The [Variables] section provides more information on using Hugo's page- and site-level variables in your templates.

{{< code-toggle copy=false >}}
include_toc: true
show_comments: false
{{</ code-toggle >}}

## Front matter cascade

Any node or section can pass down to descendants a set of front matter values as long as defined underneath the reserved `cascade` front matter key.

### Target specific pages

The `cascade` block can be a slice with a optional `_target` keyword, allowing for multiple `cascade` values targeting different page sets.

{{< code-toggle copy=false >}}
title ="Blog"
[[cascade]]
background = "yosemite.jpg"
[cascade._target]
path="/blog/**"
lang="en"
kind="page"
[[cascade]]
background = "goldenbridge.jpg"
[cascade._target]
kind="section"
{{</ code-toggle >}}

Keywords available for `_target`:

path
: A [Glob](https://github.com/gobwas/glob) pattern matching the content path below /content. Expects Unix-styled slashes. Note that this is the virtual path, so it starts at the mount root. The matching supports double-asterisks so you can match for patterns like `/blog/*/**` to match anything from the third level and down.

kind
: A Glob pattern matching the Page's Kind(s), e.g. "{home,section}".

lang
: A Glob pattern matching the Page's language, e.g. "{en,sv}".

environment
: A Glob pattern matching the build environment, e.g. "{production,development}"

Any of the above can be omitted.

### Example

In `content/blog/_index.md`

{{< code-toggle copy=false >}}
title: Blog
cascade:
  banner: images/typewriter.jpg
{{</ code-toggle >}}

With the above example the Blog section page and its descendants will return `images/typewriter.jpg` when `.Params.banner` is invoked unless:

- Said descendant has its own `banner` value set
- Or a closer ancestor node has its own `cascade.banner` value set.

## Order content through front matter

You can assign content-specific `weight` in the front matter of your content. These values are especially useful for [ordering][ordering] in list views. You can use `weight` for ordering of content and the convention of [`<TAXONOMY>_weight`][taxweight] for ordering content within a taxonomy. See [Ordering and Grouping Hugo Lists][lists] to see how `weight` can be used to organize your content in list views.

## Override global markdown configuration

It's possible to set some options for Markdown rendering in a content's front matter as an override to the [rendering options set in your project configuration][config].

## Front matter format specs

- [TOML Spec][toml]
- [YAML Spec][yaml]
- [JSON Spec][json]

[variables]: /variables/
[aliases]: /content-management/urls/#aliases
[archetype]: /content-management/archetypes/
[bylinktitle]: /templates/lists/#by-link-title
[config]: /getting-started/configuration/
[content type]: /content-management/types/
[contentorg]: /content-management/organization/
[headless-bundle]: /content-management/page-bundles/#headless-bundle
[json]: https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-404.pdf
[lists]: /templates/lists/#order-content
[lookup]: /templates/lookup-order/
[ordering]: /templates/lists/
[outputs]: /templates/output-formats/
[page-resources]: /content-management/page-resources/
[pagevars]: /variables/page/
[section]: /content-management/sections/
[taxweight]: /content-management/taxonomies/
[toml]: https://toml.io/
[urls]: /content-management/urls/
[variables]: /variables/
[yaml]: https://yaml.org/spec/
