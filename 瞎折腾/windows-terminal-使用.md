---
title: windows terminal 使用
date: 2020-05-20 19:25:38
tags:
    - terminal
categories:
    - windows
---


<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="https://figure-bed-y.oss-cn-beijing.aliyuncs.com/img/20200520192902.png">
    <br>
    <div style="color:orange; border-bottom: 1px;
    display: inline-block;
    color: #999;
    margin:4px;
    padding: 2px;">PowerShell的效果</div>
</center>


## 前言
时隔一年经历多个预览版本，积极拥抱开源的微软推出了Windows Terminal 1.0 的正式版本。

在此之前因为经常需要使用linux服务器，先后在windows10下使用过git bash和cmder，一直到现在主力使用的是cmder。因为cmder支持很多linux上的命令，与linux的使用习惯相互统一，除了打开速度比cmd略微慢了那么一些其他基本挑不出毛病，支持多窗口，方便的自定义菜单，简洁的界面......真的全是优点，大概会有人说powershell折腾折腾也很好用，也能很好看，但我好像一直感知不到powershell的存在。但一直用着cmder也觉得着不太geek，我个人是个能用系统自带或者系统推出的软件就不去用第三方的，这能尽可能的保持简洁与简单，所以window terminal一推出正式版我就下载下来折腾了半天，真好看真香，但就像最初的vscode一样，配置什么的都很不人性化。下面我会从我都整个配置过程和槽点两方面来介绍，目前而言这不是我心目中的最佳terminal除了配置完比cmder好看一些其他的真的还需要继续更近。

## 配置过程

>内容参阅：
>少数派（[1](https://sspai.com/post/59380),[2](https://sspai.com/post/52907)）
>简书 （[1](https://www.jianshu.com/p/aac4c5e87a3a)）
>官方文档（[1](https://docs.microsoft.com/zh-cn/windows/terminal/)）


### 下载安装

目前windows 10 应用商店可以直接下载正式版本。

![](https://figure-bed-y.oss-cn-beijing.aliyuncs.com/img/20200520194520.png)

### 美化配置

没有任何美化的cmd是这样的（看着就不想用，不过我后面的介绍主要是怎么美化powershell和连接远程的linux使用bash

![](https://figure-bed-y.oss-cn-beijing.aliyuncs.com/img/20200520194618.png)

整个界面的各种配置目前都没有最菜单配置完全基于`setting.json`文件，主要需要修改自定义的部分有两处，其中的list就是打开的选项是打开powershell，wsl，远程的linux，cmd，git bash之类的。schemes就是各种配色，后面会展开说明其中的具体内容

![](https://figure-bed-y.oss-cn-beijing.aliyuncs.com/img/20200520195152.png)

当然基于windows terminal只能改变终端提供的内容的配色，而真正终端提供的内容样式还是需要在终端配置的。比如配置powershell的样式就需要进入powershell配置，配置linux则需要在linux下操作。

#### PowerShell配置

1. 安装Powerline字体
   
   1. [下载链接](https://github.com/microsoft/cascadia-code/releases/download/v2005.15/CascadiaCode_2005.15.zip)
   
   2. 解压后选择所有右键点击安装即可
   
       ![](https://figure-bed-y.oss-cn-beijing.aliyuncs.com/img/20200520195951.png)
   
2. 接着用**管理员身份**打开PowerShell,运行下面的命令安装Posh-Git和Oh-My-Posh
     ```json
     Install-Module posh-git -Scope CurrentUser
     Install-Module oh-my-posh -Scope CurrentUser
     ```
   如果安装失败则先输入这句选择Y,允许加载任意脚本，再输入上面的命令
     ```json
     Set-ExecutionPolicy Bypass
     ```
   
 3. 设置PowerShell的配置文件,在`C:\Users\{username}\Documents\WindowsPowerShell\`下新建文件名称为`Microsoft.PowerShell_profile.ps1`,在其中添加上如下文件，如果想给PowerShell添加上vim功能也可以在安装好vim后加上后面的内容。
     ```json
     Import-Module posh-git
     Import-Module oh-my-posh
     Set-Theme Paradox
     ```


~~~tex
 Set-Alias vim "C:\Program Files\Vim\vim82\vim.exe"
 function Edit-Profile {
     vim $profile
 }
 function Edit-Vimrc {
     vim $HOME\_vimrc
 }
 ```
 其中的Set-Theme后面的主题可以选择其他内容，样式可以到[oh-my-posh](https://github.com/JanDeDobbeleer/oh-my-posh)的github仓库查找，只需要输入对应的主题名字即可修改。简易先直接在终端输入`Set-Theme Paradox`预览效果再写入配置文件中。
~~~

4. 到这里PowerShell部分的内容就配置完成了，看看效果先。
   ![](https://figure-bed-y.oss-cn-beijing.aliyuncs.com/img/20200520202246.png)


#### WSL和Linux配置
这两者都是一样的操作

+ 安装zsh
  ```bash
  sudo apt install zsh
  or
  yum install zsh
  ```
+ 安装on My Zsh
  ```bash
  curl -Lo install.sh https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh
  sh install.sh
  ```
+ 配置主题
  
  不同的主题可以到[ohmyzsh](https://github.com/ohmyzsh/ohmyzsh/wiki/Themes)的GitHub仓库查找，编辑`~/.zshrc`文件修改如下变量的值
  ```tex
  ZSH_THEME="robbyryssell"
  ```
  然后输入`source ~/.zshrc`

  配置第三方的则下载主题相关的文件移动到`$ZSH_CUSTOM/themes/`文件夹中，重复上面的步骤

#### Windows Terminal部分配置对不同的终端都是相同的

首先配置list部分的内容
```json
"list": [
            {
                // Make changes here to the powershell.exe profile.
                "guid": "{61c54bbd-c2c6-5271-96e7-009a87ff44bf}",
                "name": "Windows PowerShell",
                "commandline": "powershell.exe",
                "hidden": false,
                "acrylicOpacity": 0.7,
                "colorScheme": "One Half Dark",
                // "colorScheme":"Frost",
                "fontFace": "Cascadia Code PL",
                "useAcrylic": false,
                "backgroundImage": "C:\\Users\\Wentw\\Pictures\\MyerSplash\\goose.png",
                "backgroundImageStretchMode":"uniformToFill",
                "startingDirectory" : "."
            },
```

+ guid 是唯一标识可以到这个[网站](https://www.uuidgenerator.net/)直接生成一个

+ colorScheme 是配色，其中内置的配色有 Campbell，Campbell Powershell，Vintage，One Half Dark，One Half Light这些，可以直接输入名字使用，其余的可以自己从[iTerm Color Schemes](https://github.com/mbadolato/iTerm2-Color-Schemes)这个仓库中找，具体的打开其中的windowsterminal文件夹其中是每个独立的json文件
  
  ![](https://figure-bed-y.oss-cn-beijing.aliyuncs.com/img/20200520203158.png)

    具体的体现在配置文件就是添加到这里,所谓的主题就是对原本的颜色做了一个映射换成比较好看的颜色，没基础完全自定义还是挺费力的
    ```json
        "schemes": [
        {
            "name": "Frost",
            "background": "#ffffff",
            "black": "#3C5712",
            "blue": "#17b2ff",
            "brightBlack": "#749B36",
            "brightBlue": "#27B2F6",
            "brightCyan": "#13A8C0",
            "brightGreen": "#89AF50",
            "brightPurple": "#F2A20A",
            "brightRed": "#F49B36",
            "brightWhite": "#741274",
            "brightYellow": "#991070",
            "cyan": "#3C96A6",
            "foreground": "#000000",
            "green": "#6AAE08",
            "purple": "#991070",
            "red": "#8D0C0C",
            "white": "#6E386E",
            "yellow": "#991070",
            "cursorColor": "#000000"
        },
    ```

+ 到这里就配置完成下面展示一下Ubuntu(WSL)和Centos的效果图（PowerShell已经在开头了）
  
  + centos
    <center>
        <img style="border-radius: 0.3125em;
        box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
        src="https://figure-bed-y.oss-cn-beijing.aliyuncs.com/img/20200520204747.png">
        <br>
        <div style="color:orange; border-bottom: 1px;
        display: inline-block;
        color: #999;
        margin:4px;
        padding: 2px;">Centos 的效果</div>
    </center>

  + ubuntu
    <center>
        <img style="border-radius: 0.3125em;
        box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
        src="https://figure-bed-y.oss-cn-beijing.aliyuncs.com/img/20200520205241.png">
        <br>
        <div style="color:orange; border-bottom: 1px;
        display: inline-block;
        color: #999;
        margin:4px;
        padding: 2px;">ubuntu 的效果</div>
    </center>

#### 添加到鼠标右键

> 方法链接[知乎](https://zhuanlan.zhihu.com/p/91259377)

效果图：

![](https://figure-bed-y.oss-cn-beijing.aliyuncs.com/img/20200520212136.png)


## 优点缺点

+ 这部分我会考虑和cmder做对比，cmder不能针对不同的终端做不同的配置，而windows terminal可以对任何的终端PowerShell、cmd、git bash等等各种不同的终端自定义颜色主题，这点提高了可玩性也避免审美疲劳
+ 但是如果将PowerSehll和cmder在简约角度对比个人的主观喜好就比较明显了

  ![](https://figure-bed-y.oss-cn-beijing.aliyuncs.com/img/20200520211035.png)

+ 目前还没有深度使用其他的以后再补充吧。


## 我的配置文档

```json
    "theme": "system",
    "profiles": {

        "defaults": {
            // Put settings here that you want to apply to all profiles.
        },
        "list": [
            {
                // Make changes here to the powershell.exe profile.
                "guid": "{61c54bbd-c2c6-5271-96e7-009a87ff44bf}",
                "name": "Windows PowerShell",
                "commandline": "powershell.exe",
                "hidden": false,
                "acrylicOpacity": 0.7,
                "colorScheme": "One Half Dark",
                // "colorScheme":"Frost",
                "fontFace": "Cascadia Code PL",
                "useAcrylic": false,
                "backgroundImage": "C:\\Users\\Wentw\\Pictures\\MyerSplash\\goose.png",
                "backgroundImageStretchMode":"uniformToFill",
                "startingDirectory" : "."
            },
            {
                "guid": "{2c4de342-38b7-51cf-b940-2309a097f518}",
                "hidden": false,
                "name": "Ubuntu",
                "source": "Windows.Terminal.Wsl",
                "acrylicOpacity": 0.7,
                "colorScheme": "OneHalfDarkCus",
                "suppressApplicationTitle": true,
                "fontFace": "Cascadia Code PL",
                "useAcrylic": false
            },
            {
                "guid": "{fa95e5ba-3579-46f6-b60d-8859542a7116}",
                "name": "LocalCentos",
                "commandline": "powershell.exe ssh root@192.168.102.128",
                "acrylicOpacity": 0.7,
                "colorScheme": "One Half Dark",
                "suppressApplicationTitle": true,
                "fontFace": "Cascadia Code PL",
                "useAcrylic": false,
                "icon": "C:\\Users\\Wentw\\Pictures\\icons\\centos_32px_584190_easyicon.net.ico"
            },
        ]
    },
    // Add custom color schemes to this array.
    // To learn more about color schemes, visit https://aka.ms/terminal-color-schemes
    "schemes": [
        {
            "name": "Frost",
            "background": "#ffffff",
            "black": "#3C5712",
            "blue": "#17b2ff",
            "brightBlack": "#749B36",
            "brightBlue": "#27B2F6",
            "brightCyan": "#13A8C0",
            "brightGreen": "#89AF50",
            "brightPurple": "#F2A20A",
            "brightRed": "#F49B36",
            "brightWhite": "#741274",
            "brightYellow": "#991070",
            "cyan": "#3C96A6",
            "foreground": "#000000",
            "green": "#6AAE08",
            "purple": "#991070",
            "red": "#8D0C0C",
            "white": "#6E386E",
            "yellow": "#991070",
            "cursorColor": "#000000"
        },
        {
            "name": "OneHalfDarkCus",
            "black": "#282c34",
            "red": "#e06c75",
            "green": "#282c34",
            "yellow": "#e5c07b",
            "blue": "#61afef",
            "purple": "#c678dd",
            "cyan": "#56b6c2",
            "white": "#dcdfe4",
            "brightBlack": "#282c34",
            "brightRed": "#e06c75",
            "brightGreen": "#98c379",
            "brightYellow": "#e5c07b",
            "brightBlue": "#61afef",
            "brightPurple": "#c678dd",
            "brightCyan": "#56b6c2",
            "brightWhite": "#dcdfe4",
            "background": "#282c34",
            "foreground": "#dcdfe4"
          }
```