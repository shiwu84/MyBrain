---
title: Win-KeX | Kali Linux Documentation
source: https://www.kali.org/docs/wsl/win-kex/
author:
  - "[[Kali Linux]]"
published:
created: 2025-07-19
description: Windows Subsystem for Linux 2 & Win-KeX
tags:
  - clippings
---
## Win-KeX

目录

Win-KeX 为 Kali Linux 在 Windows 子系统 Linux（WSL 2）中提供 GUI 桌面体验，具有以下功能：

- [窗口模式](https://www.kali.org/docs/wsl/win-kex-win/) ：在专用窗口中启动 Kali Linux 桌面
- [无缝模式](https://www.kali.org/docs/wsl/win-kex-sl/) ：在 Windows 和 Kali 应用及菜单之间共享 Windows 桌面
- [增强会话模式](https://www.kali.org/docs/wsl/win-kex-esm/) ：类似于 Hyper-V，使用 RDP 以获得更丰富的功能体验
- 声音支持
- 共享剪贴板，支持在 Kali Linux 和 Windows 之间进行剪切和粘贴
- Root & unprivileged session support
- Multi-session support: root window & non-privileged window & seamless sessions concurrently
- Fully compatible with WSLg

[![Seamless mode](https://www.kali.org/docs/wsl/win-kex/win-kex-sl.png)](https://www.kali.org/docs/wsl/win-kex/win-kex-sl.png)

本页面详细介绍了如何在 2 分钟内安装 Win-KeX 的步骤。

## 安装

所有安装步骤，直至我们安装 Win-KeX，也在 [@NetworkChuck](https://x.com/NetWorkChuck) 的 5 分钟视频指南中进行了说明： [在 Windows 10 (WSL 2)上安装新的 Kali Linux GUI // 2020.3 版本发布](https://www.youtube.com/watch?v=dgdOILL1184)

<iframe src="https://www.youtube.com/embed/dgdOILL1184?si=wMTJtzEqD-C7BzMr" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

### 前置条件

- [Kali 在 WSL 2](https://www.kali.org/docs/wsl/wsl-preparations/)
- [Windows Terminal](https://www.microsoft.com/en-us/p/windows-terminal/9n0dx20hk701)


### 安装 Win-KeX

在 Kali WSL 中，通过以下方式安装 Win-KeX：
```shell
kali@kali:~$ sudo apt update
kali@kali:~$
kali@kali:~$ sudo apt install -y kali-win-kex
```

## 运行 Win-KeX

Win-KeX 支持以下三种模式。

### 窗口模式
[![Window Mode](https://www.kali.org/docs/wsl/win-kex/win-kex-win.png)](https://www.kali.org/docs/wsl/win-kex/win-kex-win.png)

要在窗口模式下启动支持声音的 Win-KeX，请运行以下命令之一：

- 在 Kali WSL 中： `kex --win -s`
- 在 Windows 的命令提示符下： `wsl -d kali-linux kex --win -s`

请参考 [Win-KeX 窗口模式使用说明文档](https://www.kali.org/docs/wsl/win-kex-win/) 获取更多信息。

### 增强会话模式

[![Enhanced Session Mode](https://www.kali.org/docs/wsl/win-kex/win-kex-esm.png)](https://www.kali.org/docs/wsl/win-kex/win-kex-esm.png)

要在增强会话模式下启动 Win-KeX，并支持声音和 ARM 修复程序，请运行以下命令之一：

- 在 Kali WSL 中： `kex --esm --ip -s`
- 在 Windows 的命令提示符下： `wsl -d kali-linux kex --esm --ip -s`

请参考 [Win-KeX 增强会话模式使用说明](https://www.kali.org/docs/wsl/win-kex-esm/) 获取更多信息。

### 无缝模式

[![Seamless Mode](https://www.kali.org/docs/wsl/win-kex/win-kex-sl.png)](https://www.kali.org/docs/wsl/win-kex/win-kex-sl.png)

要在无缝模式下启动 Win-KeX 并支持声音，运行，运行其中之一：

- 在 Kali WSL 中： `kex --sl -s`
- 在 Windows 的命令提示符下：\` `wsl -d kali-linux kex --sl -s` \`

请参考 [Win-KeX SL 使用文档](https://www.kali.org/docs/wsl/win-kex-sl/) 获取更多信息。

## 可选步骤

### Kali 的默认工具

如果你有空间，为什么不安装“带有一切功能的 Kali”？这将为你提供 Kali 传统的“默认”工具，这也是你所期望的：

```bash
sudo apt install -y kali-linux-large
```

[![](https://www.kali.org/docs/wsl/win-kex/win-kex-large.png)](https://www.kali.org/docs/wsl/win-kex/win-kex-large.png)

### Windows 终端

创建一个 [Windows 终端](https://www.microsoft.com/en-us/p/windows-terminal/9n0dx20hk701) 快捷方式：
[![](https://www.kali.org/docs/wsl/win-kex/win-kex-wt1.png)](https://www.kali.org/docs/wsl/win-kex/win-kex-wt1.png)

在这些选项中选择：

**带声音的基本 Win-KeX 窗口模式** :

```shell
{
      "guid": "{55ca431a-3a87-5fb3-83cd-11ececc031d2}",
      "hidden": false,
      "name": "Win-KeX",
      "commandline": "wsl -d kali-linux kex --wtstart -s",
},
```

**带声音的高级 Win-KeX 窗口模式 - Kali 图标和在 Kali 主目录启动** :

将 `kali-menu.png` 图标复制到你的 Windows 图片目录，并将图标和启动目录添加到你的 WT 配置中：

```shell
{
        "guid": "{55ca431a-3a87-5fb3-83cd-11ececc031d2}",
        "hidden": false,
        "icon": "file:///c:/users/<windows user>/pictures/icons/kali-menu.png",
        "name": "Win-KeX",
        "commandline": "wsl -d kali-linux kex --wtstart -s",
        "startingDirectory" : "//wsl$/kali-linux/home/<kali user>"
},
```

---

**无缝模式下的基本 Win-KeX 带声音** :

```shell
{
      "guid": "{55ca431a-3a87-5fb3-83cd-11ececc031d2}",
      "hidden": false,
      "name": "Win-KeX",
      "commandline": "wsl -d kali-linux kex --sl --wtstart -s",
},
```

---

**无缝模式下的高级 Win-KeX 带声音 - Kali 图标并在 Kali 主目录启动** :

将 `kali-menu.png` 图标复制到你的 Windows 图片目录，并将图标和启动目录添加到你的 WT 配置中：

```shell
{
        "guid": "{55ca431a-3a87-5fb3-83cd-11ececc031d2}",
        "hidden": false,
        "icon": "file:///c:/users/<windows user>/pictures/icons/kali-menu.png",
        "name": "Win-KeX",
        "commandline": "wsl -d kali-linux kex --sl --wtstart -s",
        "startingDirectory" : "//wsl$/kali-linux/home/<kali user>"
},
```

---

**ESM 模式下的基本 Win-KeX 带声音** :

```shell
{
      "guid": "{55ca431a-3a87-5fb3-83cd-11ecedc031d2}",
      "hidden": false,
      "name": "Win-KeX",
      "commandline": "wsl -d kali-linux kex --esm --wtstart -s",
},
```

---

**ESM 模式下的高级 Win-KeX 带声音 - Kali 图标并在 Kali 主目录启动** :

将 `kali-menu.png` 图标复制到你的 Windows 图片目录，并将图标和启动目录添加到你的 WT 配置中：

```shell
{
        "guid": "{55ca431a-3a87-5fb3-83cd-11ecedd031d2}",
        "hidden": false,
        "icon": "file:///c:/users/<windows user>/pictures/icons/kali-menu.png",
        "name": "Win-KeX",
        "commandline": "wsl -d kali-linux kex --esm --wtstart -s",
        "startingDirectory" : "//wsl$/kali-linux/home/<kali user>"
},
```

[![](https://www.kali.org/docs/wsl/win-kex/win-kex-wt1.png)](https://www.kali.org/docs/wsl/win-kex/win-kex-wt1.png)

[![](https://www.kali.org/docs/wsl/win-kex/win-kex-wt2.png)](https://www.kali.org/docs/wsl/win-kex/win-kex-wt2.png)

[![](https://www.kali.org/docs/wsl/win-kex/win-kex-full.png)](https://www.kali.org/docs/wsl/win-kex/win-kex-full.png)

享受 Win-KeX！

## 帮助

更多信息，请通过以下方式寻求帮助：

- `kex --help`
- `man kex`
- [Kali 论坛](https://forums.kali.org/)