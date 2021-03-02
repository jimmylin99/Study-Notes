###### slash commands

To call the original command instead of any possible alias, e.g. `\grep` to avoid calling the alias defined by `alias grep='grep --color=auto'`

###### `--` argument

> The first `--` argument that is not an option-argument should be accepted as a delimiter indicating the end of options. Any following arguments should be treated as operands, even if they begin with the `-` character.

`man 1 bash` 指出`-`等价于`--`

> A `--` signals the end of options and disables further option processing. Any arguments after the `--` are treated as filenames and arguments. An argument of `-` is equivalent to `--`.

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

See: [linux每日命令(34)：ps命令和pstree命令 - 博客园](https://www.cnblogs.com/huchong/p/10065246.html)

###### `kill`

* 通常，终止一个前台进程可以使用Ctrl+C键，但是，对于一个后台进程就须用kill命令来终止，我们就需要先使用ps/pidof/pstree/top等工具获取进程PID，然后使用kill命令来杀掉该进程

* kill命令是通过向进程发送指定的信号来结束相应进程的。在默认情况下，采用编号为`15`的TERM信号。TERM信号将终止所有不能捕获该信号的进程。对于那些可以捕获该信号的进程就要用编号为`9`的kill信号，强行“杀掉”该进程。

```bash
kill <PID>
kill -9 <PID> # 强制杀死进程，除了init进程

```

See: [每天一个linux命令（42）：kill命令 - 博客园](https://www.cnblogs.com/peida/archive/2012/12/20/2825837.html)

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
# 例如添加用户至docker组
sudo usermod -aG docker $USER
```



###### `wc`

输出文件的行数、字数、字符数(byte)、文件名

###### `users`, `who`, `w`

了解登录到计算机的所有用户的信息

```bash
who -r # 查询系统处于什么运行级别
```

###### `uname`

print system information

* -r内核 -m 32位还是64位 -a所有信息, -n主机名

###### `free`

```bash
free -m # 查询内存状态
```

###### `grep` & 正则表达式

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

[Regular Expression website with explanation and wiki](https://regex101.com/)

> `^word` ：表示搜索以word开头的内容
>
> `word$` 表示搜索以word结尾的内容
>
> `^$`   表示的是**空行**，不是空格
>
> `.`   代表**且**只能代表任意一个字符
>
> `\`   转义字符，让有着特殊身份意义的字符，脱掉马甲，还原原型。例如\.只表示原始小数点意义
>
> `*` 表示重复0个或多个**前面**的一个字符。**不代表所有**
>
> `.*`  表示匹配**所有**的字符
>
> `^.*` 表示以任意字符开头
>
> `[任意字符]` 匹配字符集内任意一个字符，如`[a-z]`
>
> `[^abc]` `^`在中括号里面是非的意思，不包含之意。意思就是不包含a或b或c的行
>
> {n，m} 表示重复n到m次前一个字符，`{n}`至少n次，多了不限，`{,m}`至多m次
>
> 注：使用grep或sed要对`{}`转义，即`\{\}`，egrep就不需要转义了

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

###### `lsof`

[lsof 一切皆文件 — Linux Tools Quick Tutorial](https://linuxtools-rst.readthedocs.io/zh_CN/latest/tool/lsof.html)

###### `ln` (硬链接&软链接)

图文详情：[Linux ln 命令 博客园 ](https://www.cnblogs.com/sparkdev/p/11275722.html)

默认硬链接(hard link)，即创建新的文件名指向同一个inode，不占用inode或block空间

> 由于硬链接只是在目录中添加了一条包含文件名和 对应 inode 的记录，所以它几乎不会消耗额外的磁盘容量。
> 另外在删除硬链接所关联的文件时，其实只是删除了一条目录中的记录，真正的文件并不受影响。只有在删除最后一个硬链接时才会真正删除文件的内容数据。

* 仅可以在同一个文件系统中有效

* 不可给目录创建硬链接

  > 由于这两个限制，实际使用中硬链接并没有软链接使用的广泛

  ```bash
  ln <source file or directory> <target>
  ```

软链接(symbolic link)

复制一份inode和占用一个新的data block（该block存储源文件的inode地址）

* 可以指向目录且可以跨文件系统

* 创建软链接并不增加原文件的链接数

```bash
ln -s <source> <target>
```



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

More: [sudo: cd: command not found-CSDN博客](https://blog.csdn.net/gatieme/article/details/49106865)

###### `awk`

[awk 入门教程 - 阮一峰的网络日志 ](https://www.ruanyifeng.com/blog/2018/11/awk.html)

> [`awk`](https://en.wikipedia.org/wiki/AWK)是处理文本文件的一个应用程序，几乎所有 Linux 系统都自带这个程序。
>
> 它依次处理文件的每一行，并读取里面的每一个字段。对于日志、CSV 那样的每行格式相同的文本文件，`awk`可能是最方便的工具。

```bash
awk '{print $0}' # 将标准输入打印一遍
awk -F ':' '{ print $1 }' demo.txt # 以冒号为分隔符(field separator)，提取每一行的第一个字段
$NF # 当前行有多少个字段
awk -F ':' '{print $1, $(NF-1)}' demo.txt # 倒数第二个字段
awk -F ':' '{print NR ") " $1}' demo.txt # 显示当前处理的是第几行

awk -F ':' '/usr/ {print $1}' demo.txt # 正则表达式过滤器
awk -F ':' 'NR % 2 == 1 {print $1}' demo.txt # 输出奇数行
awk -F ':' '$1 == "root" || $1 == "bin" {print $1}' demo.txt
awk -F ':' '{if ($1 > "m") print $0; else print "---"}' demo.txt # if-else语句
ping 192.168.X.X | awk '{ print $0"\t" strftime("%Y:%m:%d-%H:%M:%S",systime()) fflush() } '>ping.log # 时间戳
```

###### `curl`

> curl is a tool to transfer data from or to a server

```bash
curl https://www.example.com # 不带有任何参数时，curl 就是发出 GET 请求
-s # silent
-S # show error message
-L # redo curl if receiving redirection response
-f # fail silently

-i # also print HTTP response header
```

###### `tee`

tee指令会从标准输入设备读取数据，将其内容输出到标准输出设备，同时保存成文件

对比`cat`命令

###### `cat`

cat（英文全拼：concatenate）命令用于连接文件并打印到标准输出设备上

###### `tar`

归档

```bash
tar -c [-f Archive] File # basic syntax (-c for creation)
tar -cvf /mydata/etc.tar /etc # frequent used form
# -v for verbosely list files processed
```

压缩

```bash
tar -zcvf /mydata/etc.tar.gz /etc # use gzip
tar -jcvf /mydata/etc.tar.bz2 /etc # use bzip2
```

解压

```bash
tar -zxvf /mydata/etc.tar.gz # 解压到当前文件夹 (-x for extract)
tar -zxvf /mydata/etc.tar.gz -C /mydata/etc # 解压到指定文件夹 (-C for changing to the specified directory to perform any operations, this option is order-sensitive)
```

###### `tree`

```bash
tree -L <层数> -d <目录> # 可能需要apt install
```

###### `tmux`

```bash
tmux # 创建一个默认tmux会话及打开该窗口
exit 或 ctrl+d # 退出tmux窗口
ctrl+b ? # 帮助命令
tmux new -s <session-name> # 创建指定名称地会话
tmux detach 或 ctrl+b d# 分离会话与该窗口
tmux ls # list all sessions

# 重新接入已存在的会话
tmux attach -t <session-id or session-name>
tmux a

tmux kill-session -t <session-id or session-name> # kill session
tmux switch -t <session-id or session-name> # switch session
tmux rename-session -t <session-id> <new-name>
```

窗口分屏等其它特性和配置：[Tmux 使用教程 - 阮一峰的网络日志](http://www.ruanyifeng.com/blog/2019/10/tmux.html)

###### `cp`

```bash
cp -r dir1 dir2 # copy recursively
```

###### `systemctl`

Linux中如何启动、重启、停止、重载服务以及检查服务（如 httpd.service Apache）状态

```bash
systemctl start httpd.service
systemctl restart httpd.service
systemctl stop httpd.service
systemctl reload httpd.service
systemctl status httpd.service
systemctl kill httpd
# 列出所有状态
systemctl list-unit-files --type=service 
```

