[官方文档](https://www.nushell.sh/zh-CN/book/)

# 安装

## Arch Linux

可以直接通过包管理器安装(我还没见过Arch Linux包管理器生态不行的时候)。

```Shell
pacman -S nushell
```

## Windows 11

```shell
winget install nushell # 用户范围内安装
```

## 在Kitty中设置默认Shell

按 `Ctrl+Shift+F2` 打开 `kitty.conf` 。转到 shell 变量，取消注释该行并将 . 替换为 Nu 的路径。

## 将Nu设置为登录Shell

```Shell
chsh -s $(which nu)
```

>[!NOTE]
>这在某些情况下可能会出现问题，因为Nu并不兼容POSIX。


