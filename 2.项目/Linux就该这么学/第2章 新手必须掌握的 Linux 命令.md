Linux 常用命令可以在 [ 站点](https://www.linuxcool.com) 查看。

## 强大好用的 Shell

Shell 是一个命令行解释器，它充当用户与操作系统内核之间的“翻译官”，负责将用户输入的命令翻译给系统执行。

## 执行命令的必备知识

常见的执行 Linux 命令的格式如下：

```Shell
命令名称 [命令参数] [命令对象]
```

- **命令名称**：就是语法中的“动词”，表达的是想要做的事情，例如创建用户、查看文件、重启操作系统。
- **命令参数**：用于对命令进行调整，让被“需改”过的命令能更好的贴合工作需求，达到事半功倍的效果。
- **命令对象**：一般指要处理的文件、目录、用户等资源名称，也就是命令执行后的"承受方"。

> [!注意]
> 命令参数有长格式与短格式两种，命令名称、命令参数与命令对象之间要用空格进行分隔，且**字母严格区分大小写** 。

| 格式种类 |     示例     |
| ---- | :--------: |
| 长格式  | man --help |
| 短格式  |   man -h   |
当然了，如下格式也可以正确使用，但是推荐使用标准格式：

|       命令        |
| :-------------: |
| ls -a --reverse |
|     ls -ar      |
|  ls /home -ar   |
|  ls -ar /home   |

### 额外的 5 个快捷键/组合键小技巧

1. Tab：能够实现对命令、参数或文件的内容补全，只需要敲击两次 Tab 键即可。
2. Ctrl+C：终止当前进程的运行。
3. Ctrl+D：可以删除在终端中输入的内容，也可以退出终端。
4. Ctrl+L：清空当前终端的已有内容，相当于清屏操作。
5. Ctrl+B：后台运行命令。

### 常用系统工作命令


|     命令      |
| :---------: |
|     man     |
|    echo     |
|    date     |
| timedatectl |
|   reboot    |
|  poweroff   |
|    wget     |
|     ps      |
|   pstree    |
|     top     |
|    nice     |
|    pidof    |
|    kill     |
|   killall   |
|  ifconfig   |
|    uname    |
|   uptime    |
|    free     |
|     who     |
|    last     |
|    ping     |
|  tracepath  |
|   netstat   |
|   history   |
|     sos     |
|     pwd     |
|     cd      |
|     ls      |
|    tree     |
|    find     |
|   locate    |
|   whereis   |
|    which    |
|     cat     |
|    more     |
|    head     |
|    tail     |
|     tr      |
|     wc      |
|    stat     |
|    grep     |
|     cut     |
|    diff     |
|    uniq     |
|    sort     |
|    touch    |
|    mkdir    |
|     cp      |
|     mv      |
|     rm      |
|     dd      |
|    file     |
|     tar     |
