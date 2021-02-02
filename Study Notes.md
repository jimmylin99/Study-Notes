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

### 配置vim

[chxuan/vimplus: An automatic configuration program for vim (github.com)](https://github.com/chxuan/vimplus)

### Shell

###### 命令执行和搜索机制

`shell`一般作为用户与内核之间的“桥梁”，通过shell命令（or程序语言）可以调用内核或使用已编译好的二进制文件（如`/bin/ls`，而这些二进制文件会通过诸如system call等方式调用内核）

有许多shell解释器（或语言，因其各有对应语法），如`/bin/sh`, `/bin/bash`, `/usr/bin/zsh`， 它们是如何启动的，这个流程过于复杂，暂时没研究清楚，参考资料[Zsh/Bash startup files loading order (.bashrc, .zshrc etc.) | by Raja Sekar Durairaj | Medium](https://medium.com/@rajsek/zsh-bash-startup-files-loading-order-bashrc-zshrc-etc-e30045652f2e)

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

###### 中括号总结及比较参数

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

in order to use your anaconda version of python type the following command

```bash
sudo env "PATH=$PATH" python <enter python command>
```



###### `chmod`, `chown`, `chgrp`

```bash
chmod +x test.sh
./test.sh
chown username_or_uid file # set the owner of file to be certain user
chgrp group_or_gid file # .. the group of ..
```

###### `usermod`

```bash
sudo usermod -a -G groupName userName
# The -a (append) switch is essential. Otherwise, the user will be removed from any groups, not in the list.
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

###### `find`

```bash
find . -maxdepth 1 -name <name>
```



###### `ls` & long format

```
ls -laihH
```

long format 具体解释：[ls -- list file and directory names and attributes (mkssoftware.com)](https://www.mkssoftware.com/docs/man1/ls.1.asp)

###### `lsattr`, `chattr`

> Linux内核中有大量安全特征。EXT2/3/4**文件系统的扩展属性**（Extended Attributes）可以在某种程度上保护系统的安全
>
> **常见的扩展属性：**
>
> - A（Atime）：告诉系统不要修改对这个文件的最后访问时间。
>   - **使用A属性可以提高一定的性能**。
> - S（Sync）：一旦应用程序对这个文件执行了写操作，使系统立刻把修改的结果写到磁盘。
>   - **使用S属性能够最大限度的保障文件的完整性**。
> - a（Append Only）：系统只允许在这个文件之后追加数据，不允许任何进程覆盖或者截断这个文件。如果目录具有这个属性，系统将 只允许在这个目录下建立和修改文件，而不允许删除任何文件。
> - i（Immutable）：系统不允许对这个文件进行任何的修改。如果目录具有这个属性，那么任何的进程只能修改目录之下的文件，不允许建立和删除文件。
>   - **a属性和i属性对于提高文件系统的安全性和保障文件系统的完整性有很大的好处**。
>
> - 显示扩展属性：`lsattr [-adR] [文件|目录]`
> - 修改扩展属性：`chattr [-R] [[-+=][属性]] <文件|目录>`

###### `df`, `stat`

```bash
df -ih # list inode info instead of block usage
stat file # display file or file system status
```

###### `tr`

对来自标准输入的字符进行替换、压缩和删除

###### `sudo`, `su`

```bash
su
sudo -s # both run the shell (generally) as a superuser, this solves the error generated by `sudo cd ...`
su - # this will change the environment variable, essentially it starts the shell as a login shell
```

More: [sudo: cd: command not found_OSKernelLAB-CSDN博客](https://blog.csdn.net/gatieme/article/details/49106865)

###### `awk`

[awk 入门教程 - 阮一峰的网络日志 (ruanyifeng.com)](https://www.ruanyifeng.com/blog/2018/11/awk.html)

> [`awk`](https://en.wikipedia.org/wiki/AWK)是处理文本文件的一个应用程序，几乎所有 Linux 系统都自带这个程序。
>
> 它依次处理文件的每一行，并读取里面的每一个字段。对于日志、CSV 那样的每行格式相同的文本文件，`awk`可能是最方便的工具。

```bash
awk -F ':' '{ print $1 }' demo.txt # 以冒号为分隔符(field separator)，提取每一行的第一个字段
$NF # 当前行有多少个字段
awk -F ':' '{print $1, $(NF-1)}' demo.txt # 倒数第二个字段
awk -F ':' '{print NR ") " $1}' demo.txt # 显示当前处理的是第几行

awk -F ':' '/usr/ {print $1}' demo.txt # 正则表达式过滤器
awk -F ':' 'NR % 2 == 1 {print $1}' demo.txt # 输出奇数行
awk -F ':' '$1 == "root" || $1 == "bin" {print $1}' demo.txt
awk -F ':' '{if ($1 > "m") print $1; else print "---"}' demo.txt # if-else语句
ping 192.168.X.X | awk '{ print $0"\t" strftime("%Y:%m:%d-%H:%M:%S",systime()) fflush() } '>ping.log # 时间戳
```

###### `curl`

```bash
curl https://www.example.com # 不带有任何参数时，curl 就是发出 GET 请求
```

###### `tee`

tee指令会从标准输入设备读取数据，将其内容输出到标准输出设备，同时保存成文件

对比`cat`命令

###### `cat`

cat（英文全拼：concatenate）命令用于连接文件并打印到标准输出设备上

### Linux/Unix基本概念

##### block

操作系统读取磁盘时的最小单位，一般约4KB，由若干sector组成；sector是磁盘存储的最小单位，一般约512B

##### inode

存储文件的元信息，中文译名：索引节点；存储内容包括链接数，即有多少文件名指向该inode，文件数据block的位置，权限等信息（除了文件名）

inode本身也会消耗磁盘空间，可用`df -i`查询，一般一个inode的大小为128/256字节（Byte），可用`sudo dumpe2fs -h <filesystem_like_/dev/sda5> | grep "Inode size"`

##### major & minor number

Character or block device 的 major number identifies the driver (驱动) with this device. 内核根据major number在`open`的时候用来选择对应的驱动

而minor number只有driver会使用（kernel不会收到这个参数），用来让驱动区分具体这是哪个device

##### 环境变量

The environment variables of a process exist at runtime, and are not stored in some file or so. They are stored in the process's own memory (that's where they are found to pass on to children). But there is a virtual file in

```bash
/proc/pid/environ
```

`env` 可以显示环境变量

##### 根目录的子目录

[Ubuntu根目录下各文件夹的功能详细介绍 - Yudar - 博客园 (cnblogs.com)](https://www.cnblogs.com/yudar/p/5809219.html)

英文参考 [Filesystem Hierarchy Standard - Wikipedia](https://en.wikipedia.org/wiki/Filesystem_Hierarchy_Standard)

注意到`Ubuntu 20.04`已有改动，包括建立`/bin`至`/usr/bin`的硬链接等

* `/etc`存放配置文件

##### 动态链接库

##### 与用户账号有关的系统文件（查看用户/用户组）

######  /etc/passwd——用户账户文件

 每个用户都在文件/etc/passwd中有一个对应的记录行，它记录了这个用户的一些基本属性。这个文件对所有用户可读，查看命令为：

```shell
cat /etc/passwd
```

可以看到如下所示的输出：

```shell
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
stu1:x:1001:1001::/media/StudentGroup/stu1/:
stu2:x:1002:1001:I_love_this_test:/media/StudentGroup/stu2/:/bin/bash
```

可以看出，一行记录对应着一个用户，每行记录又被冒号(:)分隔为7个字段，其格式和具体含义如下：
 用户名:口令:用户标识号:组标识号:注释性描述:主目录:登录Shell

######  /etc/group——用户组文件

 每个用户组会对应文件/etc/group中的一行记录，查看方式如下：

```shell
cat /etc/group
```

此文件的格式也类似于/etc/passwd文件，由冒号(:)隔开若干个字段，这些字段有：
 组名:口令:组标识号:组内用户列表

######  /etc/shadow

 /etc/shadow中的记录与/etc/passwd中的记录一一对应，并且只有超级管理员才有权限查看，记录着每个用户更为隐私的信息。
 它的文件格式与/etc/passwd类似，由若干个字段组成，字段之间用":"隔开。这些字段是：
 登录名:加密口令:最后一次修改时间:最小时间间隔:最大时间间隔:警告时间:不活动时间:失效时间:标志。

##### Add User to Group

[How to add existing user to an existing group? - Ask Ubuntu](https://askubuntu.com/questions/79565/how-to-add-existing-user-to-an-existing-group)

```bash
sudo usermod -a -G groupName userName
# The -a (append) switch is essential. Otherwise, the user will be removed from any groups, not in the list.
```



### 虚拟机(mainly VMWare)

##### 虚拟机网络模式

[实例讲解虚拟机3种网络模式(桥接、nat、Host-only) - ggjucheng - 博客园 (cnblogs.com)](https://www.cnblogs.com/ggjucheng/archive/2012/08/19/2646007.html)

[NAT模式、路由模式、桥接模式 区别对比_bytxl的专栏-CSDN博客](https://blog.csdn.net/bytxl/article/details/35569217)

VMWare 配置：编辑>虚拟网络编辑器

[NAT vs. bridged network: A simple diagram (techgenix.com)](http://techgenix.com/nat-vs-bridged-network-a-simple-diagram-178/)

###### 桥接

###### NAT

###### Host-Only

### Shadowsocks

> Shadowsocks是一种基于Socks5代理方式的网络数据加密传输包，并采用Apache许可证、GPL、MIT许可证等多种自由软件许可协议开放源代码。shadowsocks分为服务器端和客户端，在使用之前，需要先将服务器端部署到服务器上面，然后通过客户端连接并创建本地代理。目前包使用Python、C、C++、C#、Go语言等编程语言开发。

利用Socks5协议（TCP/IP模型中应用层的协议），本身是不安全的，于是引入了sslocal，local默认监听localhost的1080端口，并代理（在windows上默认影响系统代理）

但看起来，windows上目前是用http进行本地代理的，不只是Socks5（IE不支持Socks5代理）

浏览器一般自动启动系统代理，其它软件一般需要手动配置

> Socks5客户端 <---socks5---> sslocal <–密文–> ss-server <—正常请求—> 目标主机
>
> Shadowsocks的处理方式是将Socks5客户端与Socks5服务器的连接提前，Socks5协议的交互完全是在本地进行的，在网络中传输的完全是利用加密算法加密后的密文，这就很好的进行了去特征化，使得传输的数据不是很容易的被特征识别。

<img src="img/image-20210126155343521.png" alt="image-20210126155343521" style="zoom: 50%;" />

[SS和SSR的原理 | A Big Boy Blog - Tech Articls & Notes (sulangsss.github.io)](https://sulangsss.github.io/2018/12/18/Network/SS-SSR 原理/)

[shadowsocks实现原理 | Hexo (bingtaoli.github.io)](https://bingtaoli.github.io/2016/11/23/shadowsocks实现原理/)

<img src="img/image-20210126162307483.png" alt="image-20210126162307483" style="zoom: 25%;" />

[Shadowsocks代理方式 - vimcaw的个人博客](https://vimcaw.github.io/blog/2017/08/13/ShadowsocksR代理方式/)

#### PAC

#### Socket 代理

> 代理分为HTTP代理和SOCKET代理
>
>   HTTP代理是在HTTP协议层的代理服务，只能处理HTTP/HTTPS请求，主要满足用户Web浏览网页需求，由于只处理HTTP请求，处理速度极快。
>   SOCKET代理不解析网络流量，传递数据包而并不关心是何种应用协议,这使得SOCKET代理可以用于多种环境，支持FTP、SMTP、HTTP等，也支持QQ、BT下载等多种应用，典型的有Shadowsocks。通常分为socks 4 和socks 5两种类型，socks 4只支持TCP协议而socks 5支持TCP/UDP协议，还支持各种身份验证机制等协议



### Windows Checksum 效验

```bash
certUtil -hashfile <filename> <algorithm> # built-in command for windows 10
get-filehash -algorithm <algorithm> <filename> # built-in for powershell
# algorithm can take SHA256 MD5 etc
```

### Nginx

[Ubuntu安装配置Nginx（一）——部署Web服务 - SegmentFault 思否](https://segmentfault.com/a/1190000015797789)

[nginx快速入门之配置篇 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/31202053)

### Supervisor

👍[Python 进程管理工具 Supervisor 使用教程 - restran - 博客园 (cnblogs.com)](https://www.cnblogs.com/restran/p/4854623.html)

### InfluxDB

#### Documentation

Root URL of the Doc. [InfluxDB OSS 1.8 Documentation (influxdata.com)](https://docs.influxdata.com/influxdb/v1.8/)

#### CLI

主要关于启动CLI：[Using influx - InfluxDB command line interface | InfluxDB OSS 1.8 Documentation (influxdata.com)](https://docs.influxdata.com/influxdb/v1.8/tools/shell/)

#### InfluxQL

This section introduces InfluxQL, the InfluxDB SQL-like query language for working with data in InfluxDB databases.

[Influx Query Language (InfluxQL) | InfluxDB OSS 1.8 Documentation (influxdata.com)](https://docs.influxdata.com/influxdb/v1.8/query_language/)

##### Line Protocol

[InfluxDB line protocol tutorial | InfluxDB OSS 1.8 Documentation (influxdata.com)](https://docs.influxdata.com/influxdb/v1.8/write_protocols/line_protocol_tutorial/)

* `insert into <retention policy> <line protocol>`
* line protocol = `<measurement>,[<tag set>] <field set> [timestamp]`
* field set is a must
* tag set and timestamp are optional

##### SELECT and FROM clause

[Explore data using InfluxQL | InfluxDB OSS 1.8 Documentation (influxdata.com)](https://docs.influxdata.com/influxdb/v1.8/query_language/explore-data/)

* `FROM <database_name>.<retention_policy_name>.<measurement_name>`

* `FROM <database_name>..<measurement_name>`

* `SELECT *::field`

* > A query requires at least one [field key](https://docs.influxdata.com/influxdb/v1.8/concepts/glossary/#field-key) in the `SELECT` clause to return data. If the `SELECT` clause only includes a single [tag key](https://docs.influxdata.com/influxdb/v1.8/concepts/glossary/#tag-key) or several tag keys, the query returns an empty response. This behavior is a result of how the system stores data.



##### GROUP BY clause

> **Note:** You cannot use `GROUP BY` to group fields.

###### GROUP BY tags

###### GROUP BY time()

[Explore data using InfluxQL | InfluxDB OSS 1.8 Documentation (influxdata.com)](https://docs.influxdata.com/influxdb/v1.8/query_language/explore-data/#group-by-time-intervals)

##### INTO clause

[Explore data using InfluxQL | InfluxDB OSS 1.8 Documentation (influxdata.com)](https://docs.influxdata.com/influxdb/v1.8/query_language/explore-data/#examples-4)

###### Rename a database

```sql
SELECT * INTO "copy_NOAA_water_database"."autogen".:MEASUREMENT FROM "NOAA_water_database"."autogen"./.*/ GROUP BY *
```

* `GROUP BY *` here preserves the tags from automatically transforming into fields

##### Other Usage

###### specify a tag with None value

#### [Use a regular expression to specify a tag with no value in the WHERE clause](https://docs.influxdata.com/influxdb/v1.8/query_language/explore-data/#use-a-regular-expression-to-specify-a-tag-with-no-value-in-the-where-clause)

```sql
SELECT * FROM "h2o_feet" WHERE "location" !~ /./
```

###### show series

```sql
SHOW series
```

###### show all keys given a tag key

```sql
SHOW tag values from <measurements> with KEY=<key_name>
```

##### Gossip

`service influxdb start` to start influxdb daemon

`influx -precision rfc3339` to start influxdb CLI

`0.0.0.0` 比 `127.0.0.1` 好用... 可以连接到外网，TODO: 请查明原因

### How to edit root file with vim

```bash
:w !sudo tee % > /dev/null
```

* tee指令会从标准输入设备读取数据，将其内容输出到标准输出设备，同时保存成文件

* `%` means "the current file name"

* The `> /dev/null` part **explicitly** throws away the standard output

* You can add this to your `.vimrc` to make this trick easy-to-use: just type `:w!!`.

  ```bash
  " Allow saving of files as sudo when I forgot to start vim using sudo.
  cmap w!! w !sudo tee > /dev/null %
  ```

[How does the vim "write with sudo" trick work? - Stack Overflow](https://stackoverflow.com/questions/2600783/how-does-the-vim-write-with-sudo-trick-work)

### Use sudo with redirection

[How do I use sudo to redirect output to a location I don't have permission to write to?](https://stackoverflow.com/questions/82256/how-do-i-use-sudo-to-redirect-output-to-a-location-i-dont-have-permission-to-wr)

```bash
sudo sh -c 'ls -hal /root/ > /root/test.out' # use shell with -c
sudo ls -hal /root/ | sudo tee /root/test.out > /dev/null # use tee with > /dev/null
```



### 其它名词解释

###### Blob

* Binary Large OBject



