---
title: Linux
description: Install Hugo on Linux.
categories: [installation]
menu:
  docs:
    parent: installation
    weight: 30
toc: true
weight: 30
---
{{% readfile file="/installation/common/01-editions.md" %}}

{{% readfile file="/installation/common/02-prerequisites.md" %}}

{{% readfile file="/installation/common/03-prebuilt-binaries.md" %}}

## Package managers 安装包管理器

### Snap


[Snap]是一个免费的Linux开源软件包管理器。快照包适用于[大多数发行版][most distributions]，安装简单，并且会自动更新。


Hugo快照包是[严格限制][strictly confined]的。严格限制的快照在完全隔离的情况下运行，最高可达被认为始终安全的最低访问级别。您创建和构建的网站必须位于主目录中或可移动媒体上。
这将安装Hugo的扩展版：

```sh
sudo snap install hugo
```

要启用或撤消对可移动介质（比如U盘）的访问，请执行以下操作：

```sh
sudo snap connect hugo:removable-media
sudo snap disconnect hugo:removable-media
```

要启用或撤消对SSH密钥的访问，请执行以下操作：

```sh
sudo snap connect hugo:ssh-keys
sudo snap disconnect hugo:ssh-keys
```

[most distributions]: https://snapcraft.io/docs/installing-snapd
[strictly confined]: https://snapcraft.io/docs/snap-confinement
[Snap]: https://snapcraft.io/

{{% readfile file="/installation/common/homebrew.md" %}}

## Repository packages

大多数Linux发行版都为通常安装的应用程序维护一个存储库。请注意，这些存储库可能不包含[最新版本][latest release]。

[latest release]: https://github.com/gohugoio/hugo/releases/latest

### Arch Linux
Linux的[Arch Linux]发行版的衍生产品包括[EeavourOS]、[Garuda Linux]、[Manjaro]等。这将安装Hugo的扩展版：

```sh
sudo pacman -S hugo
```

[Arch Linux]: https://archlinux.org/
[EndeavourOS]: https://endeavouros.com/
[Manjaro]: https://manjaro.org/
[Garuda Linux]: https://garudalinux.org/

### Debian
Linux的[Debian]发行版的衍生产品包括[elementary OS]、[KDE neon]、[Linux Lite]、[Linux Mint]、[MX Linux]、[Pop!_OS]、[Ubuntu]、[Zorin OS]等。这将安装Hugo的扩展版：

```sh
sudo apt install hugo
```

您也可以从[最新版本][latest release]页面下载Debian软件包。


[Debian]: https://www.debian.org/
[elementary OS]: https://elementary.io/
[KDE neon]: https://neon.kde.org/
[Linux Lite]: https://www.linuxliteos.com/
[Linux Mint]: https://linuxmint.com/
[MX Linux]: https://mxlinux.org/
[Pop!_OS]: https://pop.system76.com/
[Ubuntu]: https://ubuntu.com/
[Zorin OS]: https://zorin.com/os/

### Fedora

Linux的[Fedora]发行版的衍生产品包括[CentOS]、[Red Hat Enterprise Linux]等。这将安装Hugo的扩展版：



```sh
sudo dnf install hugo
```

[CentOS]: https://www.centos.org/
[Fedora]: https://getfedora.org/
[Red Hat Enterprise Linux]: https://www.redhat.com/

### openSUSE


Linux的[openSUSE]发行版的衍生产品包括[GeckoLinux]、[Linux Karmada]和其他。这将安装Hugo的扩展版：


```sh
sudo zypper install hugo
```

[GeckoLinux]: https://geckolinux.github.io/
[Linux Karmada]: https://linuxkamarada.com/
[openSUSE]: https://www.opensuse.org/

### Solus

Linux的[Solus]发行版在其软件包存储库中包含Hugo。这将安装Hugo的_standard_ 标准版本：


```sh
sudo eopkg install hugo
```

[Solus]: https://getsol.us/

{{% readfile file="/installation/common/04-docker.md" %}}

{{% readfile file="/installation/common/05-build-from-source.md" %}}

## Comparison 对比

||安装包安装|软件管理器安装|Repository packages|Docker安装|源代码安装
:--|:--:|:--:|:--:|:--:|:--:
易安装?|:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:
易升级?|:heavy_check_mark:|:heavy_check_mark:|varies|:heavy_check_mark:|:heavy_check_mark:
易降级?|:heavy_check_mark:|:heavy_check_mark: [^1]|varies|:heavy_check_mark:|:heavy_check_mark:
自动升级?|:x:|varies[^2]|:x:|:x: [^3]|:x:
最新版本支持?|:heavy_check_mark:|:heavy_check_mark:|varies|:heavy_check_mark:|:heavy_check_mark:

[^1]: Easy if a previous version is still installed.
[^2]: Snap packages are automatically updated. Homebrew requires advanced configuration.
[^3]: Possible but requires advanced configuration.
