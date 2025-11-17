![](https://youtu.be/Y-b3RvwVq7c?si=egMtGp0yRNwcn_Ck)

Bash是由GNU项目开发的命令行Shell编程语言，是Arch Linux的默认命令行Shell。

# 窗口大小调整时自动换行

在调整终端模拟器大小时，Bash可能不会接收到调整大小的信号。这会导致输入的文本无法正确换行并覆盖提示符。Shell选项 `checkwinsize`  会在每次命令后检查窗口大小，并且在必要的时候更新 `LINES` 和 `COLUMNS` 的值。

```Shell
shopt -s checkwinsize # 添加到~/.bashrc
```

# 即使设置了 `ignoreeof` ，Shell也会退出

在终端模拟器中，可以使用 `Ctrl+D` 删除输入的内容，但是反复按下后会导致 `Shell` 退出。

可以在 `~/.bashrc` 中设置：

```Shell
set -o ignoreeof
```

如果仍然出现反复按下会导致退出，可以在 `~/.bashrc` 中设置 `export IGNOREEOF=100` ，这会允许按下 `Ctrl+D` 更高的值。默认情况下，`IGNOREEOF` 默认应该是10。


