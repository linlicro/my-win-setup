# 搭建现代化的(时髦的)Windows开发环境

> 背景: 因公司的上市it审计要求，公司要求使用内部配置的电脑(Lenovo)。
>
> 另外出于个人私心，借此机会突破下自己的圈子，打破信息茧房。

作为5年MAC老兵拥抱Windows，开始搭建现代化的(时髦的)Windows开发环境。

以下是个人如何配置Windows的记录，让Windows用起来跟Mac OS X一样轻松愉悦。

## 第一步

做的第一件事就是启用Windows开发者模式: Settings - Update & security > For developers

启用`Windows Subsystem for Linux`(适用于Linux的Windows子系统): 搜索 `Windows Features` - 选择 `Turn Windows features on or off` - 选中 `适用于Linux的Windows子系统`

### 接着打造 Linux

**最新版本的Windows，可以使用WSL2了，升级吧~**: WSL 2采用的完整的Linux内核，所以例如Docker，Kubernetes都可以安装了。

更多信息 - 查阅官方文档 - [https://docs.microsoft.com/zh-cn/windows/wsl/about](https://docs.microsoft.com/zh-cn/windows/wsl/about)

#### 加速

将包管理工具 apt 源更换至中科大镜像源：

```sh
# 备份文件：
sudo cp /etc/apt/sources.list /etc/apt/sources.list.bac
```

`/etc/apt/sources.list` 文件内容前面添加如下内容：

```sh
deb https://mirrors.ustc.edu.cn/ubuntu/ bionic main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic main restricted universe multiverse

deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-security main restricted universe multiverse

deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse

deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse

## Not recommended
# deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
# deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
```

更改完 sources.list 文件后请运行 sudo apt-get update 更新索引以生效

#### zsh + oh-my-zsh

```sh
sudo apt-get install zsh
# 把默认的Shell改成 zsh
chsh -s /bin/zsh
# 安装 oh-my-zsh:
$ sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

更改zsh主题:

```sh
$ vi .zshrc
ZSH_THEME="ys"
```

## 包管理: Chocolatey

Chocolatey是一个功能强大的Windows包管理器，工作起来有点像apt-get或homebrew。

管理员权限运行CMD.exe，输入以下命令安装:

```sh
@powershell -NoProfile -ExecutionPolicy Bypass -Command "iex ((new-object net.webclient).DownloadString('https://chocolatey.org/install.ps1'))" && SET PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin
```

常用安装命令是 `cinst`(choco install的缩写)，其他命令查阅[官网](https://docs.chocolatey.org/en-us/choco/commands/)。

Chocolatey的安装包可查阅[这里](https://chocolatey.org/packages)。

## 终端: CMDer and Hyper (with PowerShell Support)

Win10的PowerShell做了很多方面的升级，假如与[CMDer](https://github.com/cmderdev/cmder)和[Hyper](https://hyper.is/)配合使用 将会更加强大，这两个工具可以完成更多的命令行工作。

安装命令:

```sh
cinst cmder -pre
cinst hyper
```

个人偏向于使用 `hyper`，更多的hyper资料参考这个仓库<https://github.com/bnb/awesome-hyper>。

如果常使用 `PowerShell`，应该开启脚本执行功能(作为研发人员，终端是你的好盆友):

```sh
Set-ExecutionPolicy Unrestricted -Scope CurrentUser
```

接下来，还需要配置PowerShell Profile，Visual Stutio Code打开方式是 `code $PROFILE`，也可以使用Vim `vim $PROFILE`。

todo: 待补充 我的PowerShell Profile 信息。

## Java

安装JDK8

```sh
cinst -y jdk8

## 推荐安装OpenJDK
choco install openjdk8
```

```sh
cint -y maven
```

IDE 推荐使用 [IntelliJ IDEA](https://www.jetbrains.com/idea/), 另附 [IntelliJ-IDEA-Tutorial](https://github.com/judasn/IntelliJ-IDEA-Tutorial)。

## Node and npm

安装方式:

```sh
cinst nodejs.install
```

因 npm的升级命令`npm i -g npm`在Windows不好用，改用 `npm-windows-upgrade`:

```sh
npm install -g npm-windows-upgrade
npm-windows-upgrade
```

## Python

```sh
cinst -y python
cinst -y pip
cinst -y easy.install
```

## DevOps: Docker

参考官方文档: <https://docs.docker.com/docker-for-windows/>

```sh
cinst docker-for-windows
```

## 开发工具

```sh
## Insomnia-具有漂亮界面的现代REST客户端:
cinst -y insomnia-rest-api-client
## 数据库-IDE; 更加推荐使用 DataGrid
cinst -y heidisql
```

## 其他

```sh
cinst -y vlc
cinst -y GoogleChrome
cinst -y 7zip.install
## an alternative to Mac's Alfred App, more plugins <http://www.wox.one/plugin>
cinst -y wox
## 邮件客户端: 推荐使用Mozilla Thunderbird(outlook也是不错的)
choco install thunderbird
```

时间管理工具: Microsoft To Do<https://to-do.live.com/tasks/myday>; (基于GTD理论)

### 参考资料

* [Microsoft官方指南](https://docs.microsoft.com/zh-cn/windows/dev-environment/overview)
* [Setup Windows 10 for Modern/Hipster Development](https://github.com/felixrieseberg/windows-development-environment)
* [Installing Windows Subsystem for Linux 2, Hyper, ZSH, Node.js and VSCode extensions](https://gist.github.com/leodutra/a6cebe11db5414cdaedc6e75ad194a00)
* [microsoft/windows-dev-box-setup-scripts](https://github.com/microsoft/windows-dev-box-setup-scripts)
* [Awesome-Windows/Awesome](https://github.com/Awesome-Windows/Awesome)
* [不用装双系统，直接在 Windows 上体验 Linux：Windows Subsystem for Linux](https://sspai.com/post/43813)
* [Ubuntu 镜像使用帮助](https://lug.ustc.edu.cn/wiki/mirrors/help/ubuntu/)
