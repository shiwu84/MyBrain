---
title: WSL 中的高级设置配置
source: https://learn.microsoft.com/zh-cn/windows/wsl/wsl-config
author:
  - "[[mattwojo]]"
published:
created: 2025-07-19
description: 在适用于 Linux 的 Windows 子系统上运行多个 Linux 发行版时用于配置设置的 wsl.conf 和 .wslconfig 文件的指南。
tags:
  - clippings
---
[`wsl.conf`](https://learn.microsoft.com/zh-cn/windows/wsl/#wslconf) 和 [`.wslconfig`](https://learn.microsoft.com/zh-cn/windows/wsl/#wslconfig) 文件用于在 WSL 中配置高级设置，这些设置将在 WSL VM [启动时应用](https://learn.microsoft.com/zh-cn/windows/wsl/#the-8-second-rule-for-configuration-changes) 。 `wsl.conf` 用于根据 WSL 发行版应用设置，`.wslconfig` 用于将全局设置应用于 WSL。 可以阅读有关以下差异的详细信息。

| 方面  | `.wslconfig`                                     | `wsl.conf`                                                   |
| --- | ------------------------------------------------ | ------------------------------------------------------------ |
| 范围  | 适用于所有 WSL 的常规设置                                  | 仅限 WSL 分发的设置                                                 |
| 配置  | WSL 中的功能启用、为 WSL 2 提供支持的虚拟机设置（RAM、要启动的内核、CPU 数等） | WSL 中的分发设置，例如启动选项、DrvFs 自动装载、网络、与 Windows 系统的互作性、系统使用情况和默认用户 |
| 位置  | `%UserProfile%\.wslconfig` ，在 WSL 分发之外           | `/etc/wsl.conf` ，而在 WSL 分发中                                  |

目前，所有 `.wslconfig` 设置仅适用于 WSL 2 分发版。 了解 [如何检查正在运行的 WSL 版本](https://learn.microsoft.com/zh-cn/windows/wsl/install#check-which-version-of-wsl-you-are-running) 。

必须等到运行你的 Linux 发行版的子系统完全停止运行并重启，配置设置更新才会显示。 这通常需要关闭发行版 shell 的所有实例后大约 8 秒。

如果启动分发版（例如 Ubuntu），请修改配置文件，关闭分发版，然后重启它，你可能会假设配置更改已立即生效。 但当前情况并非如此，因为子系统可能仍在运行。 在重新启动之前，必须等待子系统停止，以便有足够的时间让系统识别你的更改。 可以通过使用 PowerShell 和以下命令来检查关闭 Linux 发行版 (shell) 后其是否仍在运行： `wsl --list --running` 。 如果分发版未运行，则会收到响应：“没有正在运行的分发版。”现在可以重启分发版，以查看应用的配置更新。

命令 `wsl --shutdown` 是重新启动 WSL 2 发行版的快速途径，但它会关闭所有正在运行的发行版，因此请谨慎使用。 还可以使用 `wsl --terminate <distroName>` 来终止正在运行的特定发行版。

## wsl.conf

使用 **wsl.conf** 为 WSL 1 或 WSL 2 上运行的每个 Linux 发行版按各个发行版配置 **本地设置** 。

- 作为 unix 文件存储在发行版的 `/etc` 目录中。
- 用于针对每个发行版配置设置。 在此文件中配置的设置将仅应用于包含存储此文件的目录的特定 Linux 发行版。
- 可用于由 WSL 1 或 WSL 2 版本运行的发行版。
- 若要访问已安装发行版的 `/etc` 目录，请使用发行版的命令行通过 `cd /` 访问根目录，然后使用 `ls` 列出文件或者 `explorer.exe .`在Windows文件资源管理器中查看。 该目录路径应类似于： `/etc/wsl.conf` 。

### wsl.conf 的配置设置

wsl.conf 文件会针对每个发行版配置设置。 *（有关 WSL 2 发行版的全局配置，请参阅 [.wslconfig](https://learn.microsoft.com/zh-cn/windows/wsl/#wslconfig) ）。*

wsl.conf 文件支持四个部分： `automount` 、 `network` 、 `interop` 和 `user` 。 *（在.ini 文件约定之后建模，密钥在.gitconfig 文件等部分下声明。）* 有关存储 wsl.conf 文件位置的信息，请参阅 [wsl.conf](https://learn.microsoft.com/zh-cn/windows/wsl/#wslconf) 。

### systemd 支持

许多 Linux 发行版（包括 Ubuntu）默认运行“systemd”，WSL 最近添加了对此系统/服务管理器的支持，因此 WSL 更类似于在裸机上使用你最爱的 Linux 发行版。 需要 WSL 的 0.67.6+ 版本才能启用 systemd。 使用命令 `wsl --version` 检查 WSL 版本。 如果需要更新，可以在 [Microsoft Store 中获取最新版本的 WSL](https://aka.ms/wslstorepage) 。 了解更多信息，请参阅 [博客公告](https://devblogs.microsoft.com/commandline/a-preview-of-wsl-in-the-microsoft-store-is-now-available/) 。

若要启用 systemd，请使用 `wsl.conf` 通过管理员权限在文本编辑器中打开 `sudo` 文件，并将以下行添加到 `/etc/wsl.conf` ：

```bash
[boot]
systemd=true
```

然后，您需要从 PowerShell 使用 `wsl.exe --shutdown` 来关闭您的 WSL 发行版，以便重启 WSL 实例。 发行版重启后，systemd 应该就会运行了。 可以使用 `systemctl list-unit-files --type=service` 命令确认，这将显示您的服务状态。

### 自动装载设置

wsl.conf 节标签： `[automount]`

| 密钥 | 值 | 默认 | 说明 |
| --- | --- | --- | --- |
| `enabled` | 布尔 | `true` | `true` 导致固定驱动器（即 `C:/` 或 `D:/` ）自动装载到 DrvFs 中的 `/mnt` 下。 `false` 表示驱动器不会自动装载，但你仍可以手动或通过 `fstab` 装载驱动器。 |
| `mountFsTab` | 布尔 | `true` | `true` 设置启动 WSL 时要处理的 `/etc/fstab` 。 /etc/fstab 是可在其中声明其他文件系统的文件，类似于 SMB 共享。 因此，在启动时，可以在 WSL 中自动装载这些文件系统。 |
| `root` | 字符串 | `/mnt/` | 设置固定驱动器要自动装载到的目录。 默认情况下，此项设置为 `/mnt/` ，因此 Windows 文件系统 C 驱动器会装载到 `/mnt/c/` 。 如果将 `/mnt/` 更改为 `/windir/` ，则你应会看到固定的 C 驱动器装载到 `/windir/c` 。 |
| `options` | 以逗号分隔的值的列表，例如 uid、gid 等，请参阅下面的自动装载选项 | `""` | 下面列出了自动装载选项值，它们追加到了默认的 DrvFs 装载选项字符串中。 **只能指定特定于 DrvFs 的选项。** |

对于所有自动装载的驱动器，这些自动装载选项会应用为装载选项。 若要仅更改特定驱动器的选项，请改用 `/etc/fstab` 文件。 通常由装载二进制文件分析成标志的选项不受支持。 若要显式指定这些选项，必须在 `/etc/fstab` 中包含要对其执行此操作的每个驱动器。

#### 自动装载选项

为 Windows 驱动器 (DrvFs) 设置不同的装载选项可以控制为 Windows 文件计算文件权限的方式。 可使用以下选项：

| 密钥 | 说明 | 默认 |
| --- | --- | --- |
| `uid` | 用于所有文件的所有者的用户 ID | WSL 发行版的默认用户 ID（首次安装时，此项默认为 1000） |
| `gid` | 用于所有文件的所有者的组 ID | WSL 发行版的默认组 ID（首次安装时，此项默认为 1000） |
| `umask` | 要对所有文件和目录排除的权限的八进制掩码 | `022` |
| `fmask` | 要对所有文件排除的权限的八进制掩码 | `000` |
| `dmask` | 要对所有目录排除的权限的八进制掩码 | `000` |
| `metadata` | 是否将元数据添加到 Windows 文件以支持 Linux 系统权限 | `disabled` |
| `case` | 确定被视为区分大小写的目录以及使用 WSL 创建的新目录是否将设置标志。 有关选项的详细说明，请参阅 [区分大小写](https://learn.microsoft.com/zh-cn/windows/wsl/case-sensitivity) 。 选项包括 `off` 、 `dir` 或 `force` 。 | `off` |

默认情况下，WSL 会将 uid 和 gid 设置为默认用户的值。 例如，在 Ubuntu 中，默认用户为 uid=1000，gid=1000。 如果此值用于指定不同的 gid 或 uid 选项，则默认用户值将被覆盖。 否则，默认值将始终被追加。

上述 umask、fmask 等选项仅在 Windows 驱动器以元数据方式挂载时适用。 默认情况下，未启用元数据。 可 [在此处找到有关此内容的详细信息](https://learn.microsoft.com/zh-cn/windows/wsl/file-permissions) 。

#### 什么是 DrvFs？

DrvFs 是 WSL 的文件系统插件，旨在支持 WSL 和 Windows 文件系统之间的互操作。 DrvFs 支持 WSL 在 /mnt 下装载包含支持的文件系统的驱动器，例如 /mnt/c、/mnt/d 等。有关在装载 Windows 或 Linux 驱动器或目录时指定默认区分大小写行为的详细信息，请参阅 [区分大小写](https://learn.microsoft.com/zh-cn/windows/wsl/case-sensitivity) 页。

### 网络设置

wsl.conf 节标签： `[network]`

| 密钥 | 值 | 默认 | 说明 |
| --- | --- | --- | --- |
| `generateHosts` | 布尔 | `true` | `true` 将 WSL 设置为生成 `/etc/hosts` 。 `hosts` 文件包含主机名与 IP 地址对应的静态映射。 |
| `generateResolvConf` | 布尔 | `true` | `true` 将 WSL 设置为生成 `/etc/resolv.conf` 。 `resolv.conf` 包含一个 DNS 列表，能够将给定的主机名解析为其 IP 地址。 |
| `hostname` | 字符串 | Windows 主机名 | 设置要用于 WSL 发行版的主机名。 |

### 互操作设置

wsl.conf 节标签： `[interop]`

这些选项在 Insider 版本 17713 和更高版本中可用。

| 密钥 | 值 | 默认 | 说明 |
| --- | --- | --- | --- |
| `enabled` | 布尔 | `true` | 设置此键可确定 WSL 是否支持启动 Windows 进程。 |
| `appendWindowsPath` | 布尔 | `true` | 设置此键可确定 WSL 是否会将 Windows 路径元素添加到 $PATH 环境变量。 |

### 用户设置

wsl.conf 节标签： `[user]`

这些选项在版本 18980 及更高版本中可用。

| 密钥 | 值 | 默认 | 说明 |
| --- | --- | --- | --- |
| `default` | 字符串 | 首次运行时创建的初始用户名 | 设置此键指定在首次启动 WSL 会话时以哪个用户身份运行。 |

### 启动设置

启动设置仅在 Windows 11 和 Server 2022 上可用。

wsl.conf 节标签： `[boot]`

| 密钥 | 值 | 默认 | 说明 |
| --- | --- | --- | --- |
| `command` | 字符串 | `""` | 你希望在 WSL 实例启动时运行的命令字符串。 此命令以根用户身份运行。 例如 `service docker start` 。 |
| `protectBinfmt` | 布尔 | `true` | 防止 WSL 在启用 systemd 时生成 systemd 单元。 |

### GPU 设置

wsl.conf 节标签： `[gpu]`

| 密钥 | 值 | 默认 | 说明 |
| --- | --- | --- | --- |
| `enabled` | 布尔 | `true` | 允许 Linux 应用程序通过准虚拟化访问 Windows GPU。 |

### 时间设置

wsl.conf 节标签： `[time]`

| 密钥 | 值 | 默认 | 说明 |
| --- | --- | --- | --- |
| `useWindowsTimezone` | 布尔 | `true` | 设置此密钥将使 WSL 使用 Windows 中设置的时区并与之同步。 |

下面的 `wsl.conf` 示例文件展示了一些可以使用的配置选项。 在此示例中，分发版为 Ubuntu-20.04，文件路径为 `\\wsl.localhost\Ubuntu-20.04\etc\wsl.conf` 。

```bash
# Automatically mount Windows drive when the distribution is launched
[automount]

# Set to true will automount fixed drives (C:/ or D:/) with DrvFs under the root directory set above. Set to false means drives won't be mounted automatically, but need to be mounted manually or with fstab.
enabled = true

# Sets the directory where fixed drives will be automatically mounted. This example changes the mount location, so your C-drive would be /c, rather than the default /mnt/c. 
root = /

# DrvFs-specific options can be specified.  
options = "metadata,uid=1003,gid=1003,umask=077,fmask=11,case=off"

# Sets the \`/etc/fstab\` file to be processed when a WSL distribution is launched.
mountFsTab = true

# Network host settings that enable the DNS server used by WSL 2. This example changes the hostname, sets generateHosts to false, preventing WSL from the default behavior of auto-generating /etc/hosts, and sets generateResolvConf to false, preventing WSL from auto-generating /etc/resolv.conf, so that you can create your own (ie. nameserver 1.1.1.1).
[network]
hostname = DemoHost
generateHosts = false
generateResolvConf = false

# Set whether WSL supports interop processes like launching Windows apps and adding path variables. Setting these to false will block the launch of Windows processes and block adding $PATH environment variables.
[interop]
enabled = false
appendWindowsPath = false

# Set the user when launching a distribution with WSL.
[user]
default = DemoUser

# Set a command to run when a new WSL instance launches. This example starts the Docker container service.
[boot]
command = service docker start
```

## .wslconfig

使用 **.wslconfig** 为 WSL 上运行的所有已安装的发行版配置 **全局设置** 。

- 默认情况下，.wslconfig 文件不存在。 它必须创建并存储在 `%UserProfile%` 目录中才能应用这些设置。
- 用于在所有运行 WSL 2 版本的已安装 Linux 发行版中配置全局设置。
- **只能用于 WSL 2 运行的发行版** 。 作为 WSL 1 运行的发行版不受此配置的影响，因为它们不作为虚拟机运行。
- 要访问您的 `%UserProfile%` 目录，可以在 PowerShell 中使用 `cd ~` 进入您的主目录（通常是您的用户配置文件 `C:\Users\<UserName>` ），或者可以打开 Windows 文件资源管理器并在地址栏中输入 `%UserProfile%` 。 该目录路径应类似于： `C:\Users\<UserName>\.wslconfig` 。

WSL 将检测这些文件是否存在，读取内容，并在每次启动 WSL 时自动应用配置设置。 如果文件缺失或格式错误（标记格式不正确），则 WSL 将继续正常启动，而不应用配置设置。

### .wsl.conf 的配置设置

.wslconfig 文件对所有在 WSL 2 上运行的 Linux 发行版进行全局设置配置。 （有关每个发行版配置的信息，请参阅 wsl.conf）。

有关.wslconfig 文件的存储位置的信息，请参见 [.wslconfig](https://learn.microsoft.com/zh-cn/windows/wsl/#wslconfig) 。

此文件可以包含以下选项，它们会影响为任何 WSL 2 发行版提供支持的 VM：

.wslconfig 节标签： `[wsl2]`

| 密钥                        | 值   | 默认                                           | 说明                                                                                                            |
| ------------------------- | --- | -------------------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| `kernel`                  | 路径  | Microsoft 内置内核提供的收件箱                         | 自定义 Linux 内核的绝对 Windows 路径。                                                                                   |
| `kernelModules`           | 路径  | 自定义 Linux 内核模块 VHD 的绝对 Windows 路径。           |                                                                                                               |
| `memory`                  | 大小  | Windows 上总内存的 50%                            | 要分配给 WSL 2 VM 的内存量。                                                                                           |
| `processors`              | 数字  | Windows 上相同数量的逻辑处理器                          | 要分配给 WSL 2 VM 的逻辑处理器数量。                                                                                       |
| `localhostForwarding`     | 布尔  | `true`                                       | 一个布尔值，用于指定绑定到 WSL 2 VM 中的通配符或 localhost 的端口是否应可通过 `localhost:port` 从主机连接。                                     |
| `kernelCommandLine`       | 字符串 | `""`                                         | 其他内核命令行参数。                                                                                                    |
| `safeMode` \*             | 布尔  | `false`                                      | 在“安全模式”中运行 WSL，这会禁用许多功能，应用于恢复处于错误状态的发行版。 仅适用于 Windows 11 和 WSL 版本 0.66.2+。                                    |
| `swap`                    | 大小  | Windows 上 25% 的内存大小四舍五入到最接近的 GB              | 要为 WSL 2 VM 添加多少交换空间，若不需要交换文件，请使用 `0` 。 交换存储是当内存需求超过硬件设备上的限制时使用的基于磁盘的 RAM。                                    |
| `swapFile`                | 路径  | `%USERPROFILE%\AppData\Local\Temp\swap.vhdx` | 交换虚拟硬盘的绝对 Windows 路径。                                                                                         |
| `pageReporting`           | 布尔  | `true`                                       | 默认的 `true` 设置使 Windows 能够回收分配给 WSL 2 虚拟机的未使用内存。                                                               |
| `guiApplications`         | 布尔  | `true`                                       | 一个布尔值，用于在 WSL 中打开或关闭对 GUI 应用程序 ([WSLg](https://github.com/microsoft/wslg)) 的支持。                               |
| `debugConsole` \*         | 布尔  | `false`                                      | 一个布尔值，用于在 WSL 2 发行版实例启动时打开显示 `dmesg` 内容的输出控制台窗口。 仅适用于 Windows 11。                                             |
| `nestedVirtualization` \* | 布尔  | `true`                                       | 用于打开或关闭嵌套虚拟化的布尔值，使其他嵌套 VM 能够在 WSL 2 中运行。 仅适用于 Windows 11。                                                     |
| `vmIdleTimeout` \*        | 数字  | `60000`                                      | VM 在关闭之前处于空闲状态的毫秒数。 仅适用于 Windows 11。                                                                          |
| `dnsProxy`                | 布尔  | `true`                                       | 仅适用于 networkingMode = NAT。 布尔值，通知 WSL 将 Linux 中的 DNS 服务器配置为主机上的 NAT。 设置为 false 会将 DNS 服务器从 Windows 镜像到 Linux。 |
| `networkingMode` \*\*     | 字符串 | `NAT`                                        | 如果值为 `mirrored` ，则会启用镜像网络模式。 默认或无法识别的字符串会生成 NAT 网络。                                                           |
| `firewall` \*\*           | 布尔  | `true`                                       | 如果设置为 true，则 Windows 防火墙规则以及特定于 Hyper-V 流量的规则可以筛选 WSL 网络流量。                                                   |
| `dnsTunneling` \*\*       | 布尔  | `true`                                       | 更改将 DNS 请求从 WSL 代理到 Windows 的方式                                                                               |
| `autoProxy` \*            | 布尔  | `true`                                       | 强制 WSL 使用 Windows 的 HTTP 代理信息                                                                                 |
| `defaultVhdSize`          | 大小  | `1TB`                                        | 设置存储 Linux 发行版（例如 Ubuntu）文件系统的虚拟硬盘 (VHD) 大小。 可用于限制分发文件系统允许占用的最大大小。                                            |

具有 `path` 值的条目必须是带有转义反斜杠的 Windows 路径，例如： `C:\\Temp\\myCustomKernel`

具有 `size` 值的条目后面必须跟上大小的单位，例如 `8GB` 或 `512MB` 。

值类型后带有 \* 的条目仅在 Windows 11 中可用。

值类型后带有 \*\* 的条目需要 [Windows 11 版本 22H2](https://blogs.windows.com/windows-insider/2023/09/14/releasing-windows-11-build-22621-2359-to-the-release-preview-channel/) 或更高版本。

### 实验性设置

这些设置是试验性功能的选择加入预览，我们的目标是将来将其设为默认设置。

.wslconfig 节标签： `[experimental]`

| 密钥 | 值 | 默认 | 说明 |
| --- | --- | --- | --- |
| `autoMemoryReclaim` | 字符串 | `dropCache` | 检测空闲 CPU 使用率后，自动释放缓存的内存。 设置为 `gradual` 以慢速释放缓存的内存，设置为 `dropCache` 以立即释放缓存的内存。 |
| `sparseVhd` | 布尔 | `false` | 如果设置为 true，则任何新创建的 VHD 将自动设置为稀疏。 |
| `bestEffortDnsParsing` \*\* | 布尔 | `false` | 仅当 `wsl2.dnsTunneling` 设置为 true 时才适用。 如果设置为 true，Windows 将从 DNS 请求中提取问题并尝试解决该问题，从而忽略未知记录。 |
| `dnsTunnelingIpAddress` \*\* | 字符串 | `10.255.255.254` | 仅当 `wsl2.dnsTunneling` 设置为 true 时才适用。 在启用 DNS 隧道时，指定将在 Linux resolv.conf 文件中配置的 nameserver。 |
| `initialAutoProxyTimeout` \* | 字符串 | `1000` | 仅当 `wsl2.autoProxy` 设置为 true 时才适用。 配置启动 WSL 容器时，WSL 等待检索 HTTP 代理信息的时长（以毫秒为单位）。 如果代理设置在此时间之后解决，则必须重新启动 WSL 实例才能使用检索到的代理设置。 |
| `ignoredPorts` \*\* | 字符串 | Null | 仅当 `wsl2.networkingMode` 设置为 `mirrored` 时才适用。 指定 Linux 应用程序可以绑定到哪些端口（即使该端口已在 Windows 中使用）。 通过此设置，应用程序能够仅侦听 Linux 中的流量端口，因此即使该端口在 Windows 上用于其他用途，这些应用程序也不会被阻止。 例如，WSL 将允许绑定到 Linux for Docker Desktop 中的端口 53，因为它只侦听来自 Linux 容器中的请求。 应在逗号分隔的列表中设置格式，例如： `3000,9000,9090` |
| `hostAddressLoopback` \*\* | 布尔 | `false` | 仅当 `wsl2.networkingMode` 设置为 `mirrored` 时才适用。 如果设置为 `True` ，将会允许容器通过分配给主机的 IP 地址连接到主机，或允许主机通过该地址连接到容器。 始终可以使用 `127.0.0.1` 环回地址，此选项还允许使用所有额外分配的本地 IP 地址。 仅支持分配给主机的 IPv4 地址。 |

值类型后带有 \* 的条目仅在 Windows 11 中可用。

值类型后显示 \*\* 的条目需要 [Windows 版本 22H2](https://blogs.windows.com/windows-insider/2023/09/14/releasing-windows-11-build-22621-2359-to-the-release-preview-channel/) 或更高版本。

下面的 `.wslconfig` 示例文件展示了一些可以使用的配置选项。 在此示例中，文件路径为 `C:\Users\<UserName>\.wslconfig` 。

```bash
# Settings apply across all Linux distros running on WSL 2
[wsl2]

# Limits VM memory to use no more than 4 GB, this can be set as whole numbers using GB or MB
memory=4GB 

# Sets the VM to use two virtual processors
processors=2

# Specify a custom Linux kernel to use with your installed distros. The default kernel used can be found at https://github.com/microsoft/WSL2-Linux-Kernel
kernel=C:\\temp\\myCustomKernel

# Specify the modules VHD for the custum Linux kernel to use with your installed distros.
kernelModules=C:\\temp\\modules.vhdx

# Sets additional kernel parameters, in this case enabling older Linux base images such as Centos 6
kernelCommandLine = vsyscall=emulate

# Sets amount of swap storage space to 8GB, default is 25% of available RAM
swap=8GB

# Sets swapfile path location, default is %USERPROFILE%\AppData\Local\Temp\swap.vhdx
swapfile=C:\\temp\\wsl-swap.vhdx

# Disable page reporting so WSL retains all allocated memory claimed from Windows and releases none back when free
pageReporting=false

# Turn on default connection to bind WSL 2 localhost to Windows localhost. Setting is ignored when networkingMode=mirrored
localhostforwarding=true

# Disables nested virtualization
nestedVirtualization=false

# Turns on output console showing contents of dmesg when opening a WSL 2 distro for debugging
debugConsole=true

# Enable experimental features
[experimental]
sparseVhd=true
```
