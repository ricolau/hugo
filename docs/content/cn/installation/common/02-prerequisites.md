## 前置依赖 Prerequisites

虽然不是所有情况下都需要，但在使用Hugo 时，[Git]、[Go]和[Dat Sass]是常用的。


Git 主要用来：

- 使用资源仓库如github来构建Hugo 站点
- 使用 [Hugo Modules] 特性
- 用 Git submodule的方式来安装一个主题
- 从一个本地Git 仓库来访问 [commit information] 等提交信息
- 通过虚拟主机来部署你的网站到 [CloudCannon], [Cloudflare Pages], [GitHub Pages], [GitLab Pages], 和 [Netlify]等等。

Go 被需要的原因是：

- 用来编译 Hugo 程序本身
- 使用 Hugo Modules 特性

Dart Sass 被需要主要是因为：当使用Sass语言的最新功能时，Dart-Sass需要将Sass转换为CSS。

有关安装说明，请参阅相关文档：

- [Git][git install]
- [Go][go install]
- [Dart Sass][dart sass install]

[cloudcannon]: https://cloudcannon.com/
[cloudflare pages]: https://pages.cloudflare.com/
[dart sass install]: /hugo-pipes/transpile-sass-to-css/#dart-sass
[dart sass]: https://sass-lang.com/dart-sass
[git install]: https://git-scm.com/book/en/v2/Getting-Started-Installing-Git
[git]: https://git-scm.com/
[github pages]: https://pages.github.com/
[gitlab pages]: https://docs.gitlab.com/ee/user/project/pages/
[go install]: https://go.dev/doc/install
[go]: https://go.dev/
[netlify]: https://www.netlify.com/
