## WSL2安装

```shell
wsl --install

# 国内环境避免网络问题中断
wsl --installl --web-download
```

### Linux发行版选择

```shell
wsl --list --online
```

### 查看Linux已安装列表

```shell
wsl --list -v
```

## 设置默认子系统

```shell
wsl --set-default LinuxName 
```

## WSL2更新

```shell
wsl --update
```

## 卸载与备份

### 卸载

```shell
wsl --unregister LinuxName
```

### 备份

```shell
wsl --export LinuxName LinuxName.tar
```

## 导入到其他盘符

```shell
wsl --import NewLinuxName 需要导入的文件夹路径 压缩包路径 
```

## 命令混用

### 在Linux中使用Windows应用程序

```shell
notepad.exe test.txt # 使用Windows记事本打开test.txt文本文件
```

```shell
explorer.exe . # 使用Windows资源管理器打开当前目录
```

### 在Windows中使用Linux命令

```shell
Get-ChildItem | wsl grep .docker
```

[官方文档](https://learn.microsoft.com/zh-cn/windows/wsl/#wslconf) 