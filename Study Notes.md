---
typora-copy-images-to: img
---

---

### Install and Configure Windows Terminal

**Official Doc**: [Windows 终端概述 | Microsoft Docs](https://docs.microsoft.com/zh-cn/windows/terminal/)

* Tutorial (It uses PS5, please also reference tutorial of PS7 listed below)
  [新生代 Windows 终端：Windows Terminal 的全面自定义 - 少数派 (sspai.com)](https://sspai.com/post/59380)
  [Windows 终端 Powerline 设置 | Microsoft Docs](https://docs.microsoft.com/zh-cn/windows/terminal/tutorials/powerline-setup)
* 安装字体
  
  * `ttf` and `ttc` 格式，安装or直接拷贝到`C:\Windows\Fonts`即可
  
  * 注意字体名称不是文件名而是
  
    <img src="./img/font name.png" alt="font name" style="zoom:50%;" />
  
    这将在`wt`的设置里用到，例如(在`wt`中用`Ctrl+,`打开)
  
    ```json
    {//...
     "list":
            [
                {
                    // Make changes here to the powershell.exe profile.
                    "guid": "{61c54bbd-c2c6-5271-96e7-009a87ff44bf}",
                    "name": "Windows PowerShell",
                    "commandline": "powershell.exe",
                    "fontFace": "JetBrainsMono Nerd Font Mono",
                    // "fontFace": "MesloLGL NF",
                    // "fontFace": "Cascadia Mono PL",
                    "hidden": false
                },
                // ...
            ]
    //...
    }
    ```
  
    

#### Install `Chocolately` and `Scoop`

* [Chocolatey Software | Installing Chocolatey](https://chocolatey.org/install)
* [Scoop](https://scoop.sh/)
* 暂时关闭杀毒软件McAfee, and restart WT. The way to stop McAfee, is to stop real-time scanning and auto-update by clicking the gear icon.
* `Windows Terminal` run as admin: Taskbar > `SHIFT` + Right-click > ...
  * `WIN+<num>+Ctrl+Shift`

#### Powershell (PS 7) Configuration

==-----==

*If install powershell 7, please follow this instruction*: [给 PowerShell 带来 zsh 的体验 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/137251716)

now `oh-my-posh3` is also configured using [Themes | Oh my Posh 3](https://ohmyposh.dev/docs/themes)

**Other plugins & some important notices (e.g. proxy)** (including `z`, `sudo`, new `bucket`, new `shell`: [Windows Terminal美化+PowerShell插件配置 - DiaosSama's Blog](https://diaossama.work/2020/05/windows-terminal-powershell.html)

==-----==

**Deprecated**

~~The following is releated to *PS5*, while most of them are similar to *PS7*, so it's ok to reference them.~~

* ~~`notepad $PROFILE` to open profile for powershell: (simple configuration is shown as below)~~

  ```shell
  Import-Module posh-git # 引入 posh-git
  Import-Module oh-my-posh # 引入 oh-my-posh
  
  Set-Theme Powerlevel10k-Classic
  
  Set-PSReadLineOption -PredictionSource History # 设置预测文本来源为历史记录
   
  Set-PSReadlineKeyHandler -Key Tab -Function Complete # 设置 Tab 键补全
  Set-PSReadLineKeyHandler -Key "Ctrl+d" -Function MenuComplete # 设置 Ctrl+d 为菜单补全和 Intellisense
  Set-PSReadLineKeyHandler -Key "Ctrl+z" -Function Undo # 设置 Ctrl+z 为撤销
  Set-PSReadLineKeyHandler -Key UpArrow -Function HistorySearchBackward # 设置向上键为后向搜索历史记录
  Set-PSReadLineKeyHandler -Key DownArrow -Function HistorySearchForward # 设置向下键为前向搜索历史纪录
  
  # Alias for jumping to iCloud
  function Go-To-iCloud { cd C:\Users\jimmy\iCloudDrive\ }
  Set-Alias cd-icloud Go-To-iCloud
  
  # Alias for jumping to Notes
  function Go-To-Notes { cd C:\Users\jimmy\iCloudDrive\Notes\ }
  Set-Alias cd-notes Go-To-Notes
  
  # Alias for jumping to E:\share
  function Go-To-share { cd E:\share\ }
  Set-Alias cd-mnt Go-To-share
  ```

* ~~[Windows 终端 Powerline 设置 | Microsoft Docs](https://docs.microsoft.com/zh-cn/windows/terminal/tutorials/powerline-setup) 配置 `posh-git` and `oh-my-posh`~~

### Apple iTunes Backup

#### Change backup location

* Default location: instead of `%appdata%\Apple Computer`, it is directly located under `<user>\Apple`.
* 建立链接：[iphone备份太大，严重挤占C盘空间，怎么办？ - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/30324383)
  * 在Powershell 中，请使用`cmd /c mklink /J "<link>" "<target>"`, and make sure the directory of `<link>` does not exist (or say be created in advance)
  * More: `mklink /J` link directories  [Windows 中的 mklink 命令 | 始终 (liam.page)](https://liam.page/2018/12/10/mklink-in-Windows/)
    * Similar to `ln` command in Linux

### 目录生成器 适用于添加到Markdown

`treer`: [markdown如何写出项目目录结构 - 简书 (jianshu.com)](https://www.jianshu.com/p/e38a07f824a2)

### Github 访问速度慢解决方案

Link: [github访问加速 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/75994966)

**Search for IP Address**: [The Best IP Address, Email and Networking Tools - IPAddress.com](https://www.ipaddress.com/)

[(62 封私信 / 7 条消息) git clone一个github上的仓库，太慢，经常连接失败，但是github官网流畅访问，为什么？ - 知乎 (zhihu.com)](https://www.zhihu.com/question/27159393)

* 解决DNS污染：直接配置本地host
  * 主要配置三个域名的ip地址映射，请自行通过上述链接查询ip
    * github.com
    * github.global.ssl.fastly.net
    * assets-cdn.github.com
    * 其余域名如需要，请参考：[访问GitHub很慢的问题解决 | Bruce's Blog (a1049145827.github.io)](https://a1049145827.github.io/2018/07/31/访问GitHub很慢的问题解决/)
* 可能依旧需要SSR服务
  
* 一般是1080端口，可以查询SSR选项
  
* 设置git全局代理（不推荐，仅限http, https）

  ```bash
  git config --global http.proxy http://127.0.0.1:1080
  git config --global https.proxy https://127.0.0.1:1080
  ```

  设置git全局代理（较推荐，仅针对github)

  ```bash
  git config --global http.https://github.com.proxy https://127.0.0.1:1080
  git config --global https.https://github.com.proxy https://127.0.0.1:1080
  ```

  取消全局代理

  ```bash
  git config --global --unset http.proxy
  git config --global --unset https.proxy
  ```

### Gvim (Not finished)

Link: [Vim：gvim安装配置（windows） - 整合侠 - 博客园 (cnblogs.com)](https://www.cnblogs.com/lizm166/p/8514129.html)

### Everything (A Global Search Software for Windows)

Link: [voidtools](https://www.voidtools.com/zh-cn/)

### Git 常用建议

###### `git help`

`git help <command>`

* `git help -a`
* `git help -g`

###### `git branch`

* `git branch -M [<oldbranch>] <newbranch>]`:  `-M` shortcut for `--move -m`, move/rename a branch and corresponding reflog(不知道这是什么)

###### `git config`

* `git config --global`

* `git config --list`

  ```bash
  git config --global user.name <name>
  git config --global user.email <email>
  ```


###### `git push`

* `git push -u origin <branch name>`
  * `-u` is equivalent to `--set-upstream`

### 其它名词解释

###### Blob

* Binary Large OBject



