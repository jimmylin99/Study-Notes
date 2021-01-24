---
typora-copy-images-to: img
---

---

### Windows Terminal

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
  
    

#### SSH keep alive

In `~/.ssh/config` file, add

```bash
Host *
    ServerAliveInterval 40
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

### Vim常用命令

###### 跳转

1. 跳到文本的**最后一行**：按“G”,即“shift+g”
2. 跳到**最后一行**的**最后**一个字符： 先重复1的操作即按“G”，之后按“$”键，即“shift+4”。
3. 跳到第一行的第一个字符：先按两次“g”，
4. 跳转到当前行的第一个字符：在当前行按“0”

###### 撤销

`u` 撤销上一步的操作
`Ctrl+r` 恢复上一步被撤销的操作

### Python常用函数

`dict.get(key, default=None)`: default 表示若找不到key则返回缺省值

### Python查看运行时间

###### `time.perf_counter`

* 以秒为单位，参考点未定义，因而用于计算两次连续调用之间的差值
* 具有最高分辨率的时钟，包括了在睡眠期间(sleep)和系统范围内流逝的时间

### pip

###### 生成requirements.txt文件

```bash
pip freeze > requirements.txt
```

###### 安装requirements.txt依赖

```bash
pip install -r requirements.txt
```

### 

### 查询自己的公网ip

`curl ifconfig.me`

### Git 常用建议

###### `git add`

* `git add -u` 更新已track的文件 (或`git add --update`)
* `git add -A` 添加所有文件，包括新增的未track的文件

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

###### `git reset`

* 撤销add后staged的文件

  ```bash
  git reset HEAD .
  git reset HEAD <filename>
  ```

###### `git stash`

* `git stash`会把所有未提交的修改（包括暂存的和非暂存的）都保存起来，用于后续恢复当前工作目录

  See: [git-stash用法小结 - Tocy - 博客园 (cnblogs.com)](https://www.cnblogs.com/tocy/p/git-stash-reference.html)

###### `git log`

* ```bash
  git log --graph --all
  ```

###### `git commit`

* 对最近一次commit进行修改

  ```
  git commit --amend
  ```

  [更改提交消息 - GitHub Docs](https://docs.github.com/cn/github/committing-changes-to-your-project/changing-a-commit-message)

###### `git fetch`



### apt 命令

###### 更改镜像源

查询版本号 `lsb_release -c`

[【Ubuntu】修改Ubuntu的apt-get源为国内镜像源的方法](https://blog.csdn.net/zgljl2012/article/details/79065174)

### 配置zsh

[写给工程师的 Ubuntu 20.04 最佳配置指南 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/139305626)

如果遇到443错误，使用国内镜像  [Ubuntu 关于ohmyzsh下载被443拒绝连接](https://blog.csdn.net/qq_35104586/article/details/103604964)

* 另[Oh My Zsh, 『 443:Connection Refused 错误无法安装 』 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/264161761)

### Shell

###### 命令执行和搜索机制

`shell`一般作为用户与内核之间的“桥梁”，通过shell命令（or程序语言）可以调用内核或使用已编译好的二进制文件（如`/bin/ls`，而这些二进制文件会通过诸如system call等方式调用内核）

有许多shell解释器（或语言，因其各有对应语法），如`/bin/sh`, `/bin/bash`, `/usr/bin/zsh`

###### 脚本第一行

```bash
#!/bin/bash
```

###### 变量

定义时不加美元符号，变量名和等号之间不能有空格

使用变量`$variable` or `${variable}`

```bash
readonly <variable> # make it const
unset <variable> # delete it except for readonly variable
```

###### 字符串

* 单引号内的任何字符都会原样输出，单引号字符串中的变量是无效的

* 单引号字串中不能出现单独一个的单引号（对单引号使用转义符后也不行），但可成对出现，作为字符串拼接使用

* 双引号里可以有变量，双引号里可以出现转义字符

  More: [Shell 变量 | 菜鸟教程 (runoob.com)](https://www.runoob.com/linux/linux-shell-variable.html)

###### 数组

仅支持一维数组

```bash
array_name=(value0 value1 value2 value3)
${数组名[下标]} # 读取
${array_name[@]} # 所有元素
# 取得数组元素的个数
length=${#array_name[@]}
# 或者
length=${#array_name[*]}
# 取得数组单个元素的长度
lengthn=${#array_name[n]}
```

亦有其它定义方式，见上面的连接

###### 脚本参数

```bash
$0 $1 $2 # $0为执行的文件名（含路径），后面依次为参数
$# # 参数个数
$$ # 脚本运行的当前PID
```

More: [Shell 传递参数 | 菜鸟教程 (runoob.com)](https://www.runoob.com/linux/linux-shell-passing-arguments.html)

###### 表达式运算

原生bash不支持简单的数学运算，但是可以通过其他命令来实现，例如 awk 和 expr，expr 最常用

```bash
val=`expr 2 + 2`
```

###### 中括号总结

See [Shell 中的中括号用法总结 | 菜鸟教程 (runoob.com)](https://www.runoob.com/w3cnote/shell-summary-brackets.html)

###### PS1 PS2

命令提示符（一级和二级提示符），可以通过环境变量赋值更改`$PS1`,`$PS2`

###### 流程控制语句

See [Shell 流程控制 | 菜鸟教程 (runoob.com)](https://www.runoob.com/linux/linux-shell-process-control.html)



### Linux/Unix 常用命令

###### `ps` and `pstree`

```bash
ps -A # 列出所有进程
ps -e # 同上
ps -a # 列出当前终端下的所有进程
ps -u # 以用户为主的进程状态
ps -f # 更完整的输出
ps -ef
```

```bash
pstree -p # 显示PID
pstree -ap # 更详细
pstree -ap | less # 显示上下文
pstree -ap | grep <name or pid or ...>

pstree -ap $pid
```

See: [linux每日命令(34)：ps命令和pstree命令 - 听风。 - 博客园 (cnblogs.com)](https://www.cnblogs.com/huchong/p/10065246.html)

###### `kill`

* 通常，终止一个前台进程可以使用Ctrl+C键，但是，对于一个后台进程就须用kill命令来终止，我们就需要先使用ps/pidof/pstree/top等工具获取进程PID，然后使用kill命令来杀掉该进程

* kill命令是通过向进程发送指定的信号来结束相应进程的。在默认情况下，采用编号为`15`的TERM信号。TERM信号将终止所有不能捕获该信号的进程。对于那些可以捕获该信号的进程就要用编号为`9`的kill信号，强行“杀掉”该进程。

```bash
kill <PID>
kill -9 <PID> # 强制杀死进程，除了init进程

```

See: [每天一个linux命令（42）：kill命令 - peida - 博客园 (cnblogs.com)](https://www.cnblogs.com/peida/archive/2012/12/20/2825837.html)

###### `which`

###### `type`

```bash
type -a
```

###### `/etc/passwd`

```bash
cat /etc/passwd
```

第六列是home directory 即`~`

###### `env`

列出所有环境变量

###### `chmod`, `chown`, `chgrp`

```bash
chmod +x test.sh
./test.sh
chown username_or_uid file # set the owner of file to be certain user
chgrp group_or_gid file # .. the group of ..
```

###### `wc`

输出文件的行数、字数、字符数(byte)、文件名

###### `users`, `who`, `w`

了解登录到计算机的所有用户的信息

###### `grep`

```bash
grep -rnw '/path/to/somewhere' -e 'pattern'
# -r or -R is recursive
# -n is line number
# -w whole word
# -l is to just give file name of matching files
grep -o 'pattern'
# -o print only the matched parts of a matching line
grep --exclude=\*.sh -rnw . -e 'pattern'
grep --include=\*.{c,h} -rnw 'path' -e 'pattern' # only search .c / .h files
grep --exclude-dir={dir1,dir2,*.dst} -rnw . -e 'p'
```

###### `ls`

```
ls -laihH
```

long format 具体解释：[ls -- list file and directory names and attributes (mkssoftware.com)](https://www.mkssoftware.com/docs/man1/ls.1.asp)

###### `df`, `stat`

```bash
df -ih # list inode info instead of block usage
stat file # display file or file system status
```

### Linux/Unix基本概念

##### block

操作系统读取磁盘时的最小单位，一般约4KB，由若干sector组成；sector是磁盘存储的最小单位，一般约512B

##### inode

存储文件的元信息，中文译名：索引节点；存储内容包括链接数，即有多少文件名指向该inode，文件数据block的位置，权限等信息（除了文件名）

inode本身也会消耗磁盘空间，可用`df -i`查询，一般一个inode的大小为128/256字节（Byte），可用`sudo dumpe2fs -h <filesystem_like_/dev/sda5> | grep "Inode size"`



### 其它名词解释

###### Blob

* Binary Large OBject



