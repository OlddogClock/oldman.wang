---
title: 为Windows打造完美终端（非WSL）
categories: 程序员杂志
tags:
  - Windows
date: 2021-12-01
---
![](https://oldmanblog.oss-cn-guangzhou.aliyuncs.com/blog/20211130115934.png)
就是要美化Powershell，Mac下有iTerm和oh-my-zsh，很多文章让你使用WSL来实现同样的效果，那叫Windows吗？！而且终端也不能太臃肿和花哨了，我认为有：
1. 彩色命令行
2. git分支显示
3. conda环境提示
4. 历史命令提示
就可以了。

<!--more -->
Windows下的oh-my-zsh是oh-my-posh，以下操作于2021年12月进行。
## 基本环境
我使用的是Windows 10 21H2，下面三个软件按顺序安装，全部使用默认安装路径

1. Windows Terminal [下载](https://www.microsoft.com/zh-cn/p/windows-terminal/9n0dx20hk701?rtc=1&activetab=pivot:overviewtab)
2. PowerShell 7.2 [下载](https://github.com/PowerShell/PowerShell/releases/)
3. Git for Windows 2.34 [下载](https://github.com/git-for-windows/git/releases/)
4. Miniconda3（Conda 4.10.3 Python 3.9.5） [下载](https://docs.conda.io/en/latest/miniconda.html)

## 将conda配置进Windows Terminal
打开Windows Terminal设置：

![](http://oldmanblog.oss-cn-guangzhou.aliyuncs.com/blog/20211130130500.png)

打开设置的JSON文件：

![](http://oldmanblog.oss-cn-guangzhou.aliyuncs.com/blog/20211130130711.png)

根据实际情况，替换或增加如下内容：

```
"defaultProfile": "{45b48db0-01a4-4d92-937b-60f9928e8c5c}",
"profiles": 
    {
        "defaults": {
            "fontFace": "Hack Nerd Font", //默认字体
            "fontSize": 14,
            "cursorShape": "bar"
        },
        "list":  // Anaconda，其它我都用不到，这里注意根据自己实际情况改，否则会覆盖已有的配置文件，比如有WSL的
        [
            {
                "bellStyle": 
                [
                    "window",
                    "taskbar"
                ],
                "colorScheme": "One Half Dark",
                "commandline": "C:\\Program Files\\PowerShell\\7\\pwsh.exe -ExecutionPolicy ByPass -NoExit -Command \"& 'C:\\ProgramData\\Miniconda3\\shell\\condabin\\conda-hook.ps1' ; conda activate 'C:\\ProgramData\\Miniconda3' \"",
                "font": 
                {
                    "face": "Hack Nerd Font",
                    "size": 14
                },
                "guid": "{45b48db0-01a4-4d92-937b-60f9928e8c5c}",
                "hidden": false,
                "name": "Anaconda Prompt",
                "startingDirectory": "%USERPROFILE%"
            }
        ]
    },
```
* `defaultProfile`Windows Terminal启动后的默认终端配置，就是list中的guid值
* `font`找一个自己喜欢的字体Mono字体，最好是[Nerd Font](https://www.nerdfonts.com/font-downloads)，含有字体图标的为后面美化做准备；
* `guid`可以用我这个，只要与文件里的其它项不重复就行；
* `icon`自己找个喜欢的图标，显示在Windows Terminal标题栏上的。
* `commandline`取值来自`Anaconda Powershell Prompt (Miniconda3)`的快捷方式属性中的“目标”，如果是`%windir%\System32\WindowsPowerShell\`开头，可能是win10自带的powershell目录，不是新安装的PowerShell 7.2版本，需要改成`C:\Program Files\PowerShell\7\pwsh.exe`，`双引号"`和`斜线\`前面要加`\`进行转义

![](http://oldmanblog.oss-cn-guangzhou.aliyuncs.com/blog/20211204155456.png)


至此，Anaconda3已被集成在Windows Terminal

## 让Windows Terminal（PowerShell）支持Git
安装Git for Windows后，环境变量的PATH会被增加上`C:\Program Files\Git\cmd`
我再PowerShell里又装了一次posh-git
```
Install-Module posh-git -Scope CurrentUser
```
如果你想为所有的用户安装，请使用`-Scope AllUsers`，下同

## 安装 oh-my-posh
oh-my-posh介绍详见官网 [https://ohmyposh.dev/docs/windows](https://ohmyposh.dev/docs/windows)
```
Install-Module oh-my-posh -Scope CurrentUser
```
编辑`$Profile`文件，此文件类似于 ~/.zshrc 会在启动 PowerShell 的时候自动执行，我们要用它引入 oh-my-posh 模块：
```
notepad.exe $Profile
```
输入以下内容：
```
Import-Module posh-git # 引入 posh-git
Import-Module oh-my-posh # 引入 oh-my-posh

Set-PoshPrompt Paradox # 设置主题为 Paradox
Set-PSReadLineOption -PredictionSource History  # 设置预测文本来源为历史记录
Set-PSReadlineKeyHandler -Key Tab -Function Complete # 设置 Tab 键补全
Set-PSReadLineKeyHandler -Key "Ctrl+d" -Function MenuComplete # 设置 Ctrl+d 为菜单补全和 Intellisense
Set-PSReadLineKeyHandler -Key "Ctrl+z" -Function Undo # 设置 Ctrl+z 为撤销

Set-PSReadLineKeyHandler -Key UpArrow -ScriptBlock {
[Microsoft.PowerShell.PSConsoleReadLine]::HistorySearchBackward()
[Microsoft.PowerShell.PSConsoleReadLine]::EndOfLine()
} # 设置向上键为后向搜索历史记录，并将光标移动到行尾

Set-PSReadLineKeyHandler -Key DownArrow -ScriptBlock {
[Microsoft.PowerShell.PSConsoleReadLine]::HistorySearchForward()
[Microsoft.PowerShell.PSConsoleReadLine]::EndOfLine()
} # 设置向下键为前向搜索历史纪录，并将光标移动到行尾
```
重启Windows Terminal即可看到最终效果。

## 自定义oh-my-posh的提示符
刚才我们已经选定了Paradox主题，通过`Get-PoshThemes`命令可以查看和预览所有的主题，按住Ctrl点击主题名称可以对其编辑
![](http://oldmanblog.oss-cn-guangzhou.aliyuncs.com/blog/20211201152417.png)

比如Paradox主题的，可以通过删减、修改`segments`内的元素就是修改提示符，比如我在管理员（root）模式时，增加了root提示：
![](http://oldmanblog.oss-cn-guangzhou.aliyuncs.com/blog/20211201153036.png)
```
{
  "$schema": "https://raw.githubusercontent.com/JanDeDobbeleer/oh-my-posh/main/themes/schema.json",
  "blocks": [
    {
      "type": "prompt",
      "alignment": "left",
      "segments": [
        {
          "type": "root",
          "style": "powerline",
          "powerline_symbol": " \uE0B0 root",
          "foreground": "#ffffff",
          "background": "#ff0000"
        },
        {
          "type": "path",
          "style": "powerline",
          "powerline_symbol": "\uE0B0",
          "foreground": "#100e23",
          "background": "#91ddff",
          "properties": {
            "folder_icon": "\uF115",
            "folder_separator_icon": " \uE0B1 ",
            "style": "full"
          }
        },
        {
          "type": "git",
          "style": "powerline",
          "powerline_symbol": "\uE0B0",
          "foreground": "#193549",
          "background": "#95ffa4",
          "properties": {
            "template": "{{ .HEAD }}"
          }
        },
        {
          "type": "python",
          "style": "powerline",
          "powerline_symbol": "\uE0B0",
          "foreground": "#100e23",
          "background": "#906cff",
          "properties": {
            "prefix": " \uE235 "
          }
        },
        {
          "type": "exit",
          "style": "powerline",
          "powerline_symbol": "\uE0B0",
          "foreground": "#ffffff",
          "background": "#ff8080",
          "properties": {
            "template": "\uE20F"
          }
        }
      ]
    },
    {
      "type": "prompt",
      "alignment": "left",
      "newline": true,
      "segments": [
        {
          "type": "text",
          "style": "plain",
          "foreground": "#007ACC",
          "properties": {
            "prefix": "",
            "text": "\u276F"
          }
        }
      ]
    }
  ],
  "final_space": false
}
```