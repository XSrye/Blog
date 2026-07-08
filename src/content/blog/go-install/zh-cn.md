---
title: "Linux 下安装 Golang"
pubDate: 2026-03-15
description: "包管理器安装与官方二进制包安装两种方式，以及目录分级管理方法"
category: 杂文
image: ""
draft: false
slugId: "go-install"
---

# Linux

## 方法一：包管理器安装

笔者的机器环境分为阿里云服务器和WSL两种，都是基于Ubuntu24.04LTS或者26.04LTS的系统，故本篇文章仅使用apt包进行说明，其它诸如pacman，dnf，yum同理操作或搜寻资料即可。Ubuntu系统的apt包内置的有golang-go，但是大多并不是最新版本，截至本篇文章写稿之前，官网的最新版本为1.26.3，使用以下命令可以查看当前系统apt的golang包版本。

```bash
apt show golang-go
```

以笔者自己的设备为例，以下分别为服务器和WSL的版本：

服务器版本（Ubuntu24.04LTS）：

![](/upload/image-mtRv.png)

WSL（Ubuntu26.04LTS）：

![](/upload/image-eczN.png)

可以看到24.04的版本为1.22，26.04为1.26，对于没有使用最新版本的需求，即可直接使用包管理器进行下载安装。

```bash
apt install golang-go
```

## 方法二：官方二进制包安装

### 前置准备

#### 二进制包下载

进入Golang发行官网选择合适的二进制包下载：[All releases - The Go Programming Language](https://golang.google.cn/dl/)，以笔者自己机器为例，下载版本为红框选项。

![](/upload/image-tuJf.png)

### 通用方法

#### 步骤一：安装包解压

将下载好的压缩包放入你的机器系统中，云服务器最好使用ssh客户端进行ssh连接，可以通过客户端内的内置的文件上传将下载好的安装包传入服务器内，或者使用FTP服务以及云服务商提供的文件上传服务。如果是WSL则可以使用mv或cp命令直接操作windows系统内的文件移动或复制到WSL的系统内。当然，除此之外，也可以使用wget拉取国内分发站的最新版。之后切换到文件所在目录执行以下命令：

```bash
tar -C /usr/local -xzf go1.26.3.linux-amd64.tar.gz
```

#### 步骤二：设置环境变量

暂时添加：在终端中执行以下命令

```bash
export PATH=$PATH:/usr/local/go/bin
```

永久添加：将以下命令添加到~/.bash_profile 或者 /etc/profile，并将以下命令添加该文件的末尾，这样就永久生效了，笔者自己添加到/etc/profile的结尾了。

```bash
export PATH=$PATH:/usr/local/go/bin
```

无论暂时添加还是永久添加，之后均需要执行以下命令：

```bash
#根据之前添加的具体文件选择
source ~/.bash_profile 
source /etc/profile
```

#### 步骤三：环境验证

终端内使用以下命令验证是否成功安装：

```bash
go version 
```

成功之后，创建目录并初始化和go文件进行验证。

```bash
mkdir gopr
cd gopr
go mod init myproject //使用Module模式创建，会在当前目录下生成go.mod文件
touch main.go
```

go mod init myproject ，其中myproject是笔者自己定义的，用户可以自行定义，防止后续import出错。

main.go文件内写入以下内容：

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, Go!")
}
```

成功输出即可成功。

### 目录分级方法

#### 步骤一：目录创建

在安装包下载完成导入系统之后，先不要进行解压，首先在自己指定目录下（例如home）创建分级目录：

```bash
go/
|
|--root/
|  |
|  |--go1.26.3/
|  |
|  |--go1.25.10/
|
|--mod/
|  |
|  |--bin/
|  |
|  |--libs/
|
|--cache/
|
|--temp/
|
|--env
```

- `go/root`目录用于存放各个版本 go 语言源文件
    
- `go/mod`对应`GOPATH`
    
- `go/mod/libs`对应`GOMODCACHE`，也就是下载的第三方依赖存放地址
    
- `go/mod/bin`对应`GOBIN`，第三方依赖二进制文件存放地址
    
- `go/cache`，对应`GOCACHE`，存放缓存文件
    
- `go/temp`，对应`GOTMPDIR`，存放临时文件
    
- `go/env`，对应`GOENV`，全局环境变量配置文件
    

该方式更新时不需要覆盖原安装目录，只需要将其存放到`go/root`目录下，然后修改`GOROOT`系统环境变量为该目录下指定版本的文件夹即可。在默认情况下 env 文件是读取的路径`GOROOT/env`，通过设置`GOENV`系统变量将其与`GOROOT`分离开，避免了因版本变更时 go 环境变量配置的变化，下面是`env`文件的初始设置。

之后进行安装包解压，解压到上述目录下的go/root目录下，使用go1.26.3格式命名：

```bash
tar -C /home/go/root -xzf go1.26.3.linux-amd64.tar.gz
```

```bash
GO111MODULE=on
GOCACHE=/home/go/cache
GOMODCACHE=/home/go/mod/libs
GOBIN=/home/go/mod/bin
GOTMPDIR=/home/go/temp
```

#### 步骤二：设置环境变量

将以下内容添加到~/.bashrc中

```bash
# 指向当前使用的 Go 版本目录
export GOROOT=/home/go/root/go1.26.3

# Go 工作区（mod 目录）
export GOPATH=/home/go/mod

# Go 二进制文件 + 第三方工具 加入 PATH
export PATH=$PATH:$GOROOT/bin:$GOPATH/bin

# 指定 env 配置文件位置（与 GOROOT 分离）
export GOENV=/home/go/env
```

之后进行source并验证：

```bash
source ~/.bashrc
go version   # 验证是否生效
```

#### 步骤三：环境验证

终端内使用以下命令验证是否成功安装：

```bash
go version 
```

成功之后，创建目录并初始化和go文件进行验证。

```bash
mkdir gopr
cd gopr
go mod init myproject //使用Module模式创建，会在当前目录下生成go.mod文件
touch main.go
```

go mod init myproject ，其中myproject是笔者自己定义的，用户可以自行定义，防止后续import出错。

main.go文件内写入以下内容：

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, Go!")
}
```

成功输出即可成功。

注意：如遇到切换用户之后无法使用GO环境，请将环境变量添加到/etc/profile文件中，并source /etc/profile。
