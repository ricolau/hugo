---
title: macOS系统
description: 在macOS上安装Hugo
categories: [installation]
menu:
  docs:
    parent: installation
    weight: 20
toc: true
weight: 20
---
{{% readfile file="/installation/common/01-editions.md" %}}

{{% readfile file="/installation/common/02-prerequisites.md" %}}

{{% readfile file="/installation/common/03-prebuilt-binaries.md" %}}

## Package managers 通过软件管理工具安装

{{% readfile file="/installation/common/homebrew.md" %}}

### MacPorts安装
[MacPorts]是一款适用于macOS的免费开源软件包管理器。这将安装Hugo的扩展版：

```sh
sudo port install hugo
```

[MacPorts]: https://www.macports.org/

{{% readfile file="/installation/common/04-docker.md" %}}

{{% readfile file="/installation/common/05-build-from-source.md" %}}

## Comparison

||安装包安装|软件管理器安装|Docker安装|源代码安装
:--|:--:|:--:|:--:|:--:|:--:
易安装?|:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|
易升级?|:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:
易降级?|:heavy_check_mark:|:heavy_check_mark: [^1]|:heavy_check_mark:|:heavy_check_mark:
自动更新?|:x:|:x: [^2]|:x: [^2]|:x:
最新版本支持?|:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:

[^1]: Easy if a previous version is still installed.
[^2]: Possible but requires advanced configuration.
