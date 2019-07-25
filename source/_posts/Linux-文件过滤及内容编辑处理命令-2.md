---
title: Linux-文件过滤及内容编辑处理命令-2
copyright: true
date: 2019-06-30 19:26:29
categories: Linux系列
tags:
  - Linux
---

# 3.0 做Linux运维的多个好处

1. 做运维可以认识更多人，同时也被更多人认识。

2. 做运维可以让自己沟通，交际能力变得比开发人员更强。

3. 相比开发岗位，运维的岗位更重要一些。

# 3.1 cat 合并文件或查看文件内容

`concatenate`

__cat命令5大常用功能__

| 序号 | 常用功能                 | 例子以及说明                                              |
|------|--------------------------|-----------------------------------------------------------|
| 1    | 查看文件内容             | cat file.txt 查看文件内容，最基本的使用                   |
| 2    | 多个文件合并为一个文件   | cat f1 f2 > newf 将f1和f2的内容合并到newf                 |
| 3    | 创建编辑新文件           | cat > file 输入后会请求输入，快捷键Ctrl+d/c可退出         |
| 4    | 非交互式编辑或追加新内容 | cat >>file<<EOF 输入后会请求输入，在结束时输入EOF即可退出 |
| 5    | 清空文件内容             | cat /dev/null > file 清空文件内容                         |

语法格式

~~~
cat [选项] [文件]
~~~

| 选项 | 说明                                   |
|------|----------------------------------------|
| -n   | 为输出的内容加编号，从1开始            |
| -b   | 与-n类似，但它忽略空白行               |
| -s   | 当遇到有多行空白行，则合并为单行空白行 |
| -v   | 除了LFD和TAB之外，使用^和M-引用        |
| -T   | 将Tab字符显示为^I                      |
| -E   | 在行尾显示$符号                        |
| -t   | 等价与-vT                              |
| -A   | 等价于-vE                              |

例子

__使用cat命令以非交互式的方式编辑文件__

方式1:

~~~
> cat >file<<EOF


Just a test.
EOF
> cat file


Just a test
~~~

解释: EOF为字符标签，用于标记从何开始与从何结束，可替换为任意字符，常用做法是用EOF表示，并且字符标签必须成对出现

易错点: 结束EOF必须置前，前面不能有任意字符

方式2:

~~~
> cat >file<<-EOF


Just a test.
  EOF
> cat file


Just a test.
~~~

解释: 在字符标签前加上`-`可以使得结束标签前可放置制表符，但仅仅只可放制表符

__合并多个文件为单个文件__

~~~
> cat > f1
f1
Ctrl+d
> cat > f2
f2
Ctrl+c
> cat f1 f2
f1
f2
> cat f1 f2 > newfile
> cat newfile
f1
f2
~~~

# 3.2 tac 反向显示文件内容

跟`cat`命令类似，反向输出文件内容

| 选项 | 说明                             |
|------|----------------------------------|
| -b   | 在行前添加分隔标志               |
| -r   | 将分隔标志视作正则表达式进行解析 |
| -s   | 使用指定字符作为换行的标志       |

例子

~~~
> cat file1
hello
world
> tac file2
world
hello
~~~

# 3.3 more 分页显示文件内容

`more`类似于`cat`命令，区别在于`cat`命令是将文件内容一次性全部显示在屏幕上，而`more`则会分页进行显示。

语法格式

~~~
more [选项] [文件]
~~~

`more`参数选项说明

| 选项 | 说明                                                                         |
|------|------------------------------------------------------------------------------|
| -num | 指定屏幕显示大小为num行                                                      |
| +num | 从行号num开始显示                                                            |
| -s   | 把连续的多个空行显示在一行                                                   |
| -p   | 不滚屏，而是清除整个自己屏幕，然后显示文件                                   |
| -c   | 不滚屏，而是从每一屏的顶部开始显示文本，每显示完一行，就清除这一行的剩余部分 |

`more`交互式说明

| 子命令    | 说明                     |
|-----------|--------------------------|
| h/?       | 查看帮助                 |
| 空格键    | 向下滚动一屏             |
| z         | 向下滚动一屏             |
| Enter     | 向下显示一行             |
| f         | 向下滚动一屏             |
| b         | 返回上一屏               |
| =         | 输出当前行的行号         |
| /查找文本 | 查找指定的文本           |
| :f        | 输出文件名和当前行的行号 |
| v         | 调用vi编辑器             |
| !命令     | 调用Shell，并执行命令    |
| q         | 退出more                 |

# 3.4 less 分页显示文件内容

`less`类似`more`命令，`less`的功能比`more`要强大

语法格式

~~~
less [选项] [文件]
~~~

`less`命令的参数选项及说明

| 选项 | 说明                                                        |
|------|-------------------------------------------------------------|
| -i   | 搜索时忽略大小写                                            |
| -m   | 显示进度百分比                                              |
| -N   | 显示行号                                                    |
| -s   | 多行空行显示为单行                                          |
| -e   | 当显示到文件结尾时自动退出，若没有此选项则需要用交互式q退出 |

`less`命令的交互式子命令及说明

| 子命令    | 说明         |
|-----------|--------------|
| b         | 向前翻一页   |
| 空格键    | 向后翻一页   |
| u         | 向前翻半页   |
| d         | 向后翻半页   |
| y         | 向上滚动一行 |
| 回车键    | 向下滚动一行 |
| 方向键-上 | 向上滚动一行 |
| 方向键-下 | 向下滚动一行 |
| Page UP   | 向上滚动一屏 |
| Page Down | 向下滚动一屏 |

例子:

__显示文件内容时并且显示行号__

~~~
> less -N 文件名
~~~

__分页显示命令的结果__


~~~
> ls -l | less
~~~

# 3.5 head 显示文件内容头部

`head`命令用于显示文件内容头部，默认输出行数为10行。

语法格式

~~~
head [选项] [文件]
~~~

`head`命令的参数选项及说明

| 选项     | 说明                                     |
|----------|------------------------------------------|
| -n<行数> | 指定显示的行数                           |
| -c<字节> | 指定显示的字节数                         |
| -q       | 显示时不包含指定的文件名作为文件头部     |
| -v       | 显示时包含指定的文件的文件名作为文件头部 |

例子:

__默认显示文件的前10行__

~~~
> head /etc/passwd
root:x:0:0::/root:/bin/bash
nobody:x:65534:65534:Nobody:/:/sbin/nologin
dbus:x:81:81:System Message Bus:/:/sbin/nologin
bin:x:1:1::/:/sbin/nologin
daemon:x:2:2::/:/sbin/nologin
mail:x:8:12::/var/spool/mail:/sbin/nologin
ftp:x:14:11::/srv/ftp:/sbin/nologin
http:x:33:33::/srv/http:/sbin/nologin
systemd-journal-remote:x:982:982:systemd Journal Remote:/:/sbin/nologin
systemd-network:x:981:981:systemd Network Management:/:/sbin/nologin
~~~
__显示文件的前n行__

~~~
> head -n 2 /etc/passwd
root:x:0:0::/root:/bin/bash
nobody:x:65534:65534:Nobody:/:/sbin/nologin
~~~

__显示文件的前n个字节__

~~~
> head -c 4 /etc/passwd
root
~~~

__显示多个文件__


~~~
> head -1 /etc/passwd /etc/profile
==> /etc/passwd <==
root:x:0:0::/root:/bin/bash

==> /etc/profile <==
# /etc/profile
~~~

# 3.6 tail显示文件内容尾部

`tail`命令用于显示文件内容尾部，默认输出行数为10行。

语法格式

~~~
tail [选项] [文件]
~~~

`tail`命令的参数选项及说明

| 选项         | 说明                                       |
|--------------|--------------------------------------------|
| -c<字节>     | 指定显示的字节数                           |
| -n<行数>     | 指定显示的行数                             |
| -f           | 实时输出文件内容追加的数据                 |
| --retry      | 不停的尝试打开文件，直到打开为止           |
| -F           | 等同于-f--retry                            |
| --pid=进程号 | 若进程关闭则tail的-f选项则不会继续输出内容 |
| -s 秒数 N    | 监视文件的间隔秒数                         |
| -q           | 显示时不包含指定的文件名作为文件头部       |
| -v           | 显示时包含指定的文件的文件名作为文件头部   |

例子:

__显示文件最后10行__

~~~
> tail /etc/passwd
git:x:973:973:git daemon user:/:/usr/bin/git-shell
lightdm:x:620:620:Light Display Manager:/var/lib/lightdm:/sbin/nologin
nm-openconnect:x:972:972:NetworkManager OpenConnect:/:/sbin/nologin
nm-openvpn:x:971:971:NetworkManager OpenVPN:/:/sbin/nologin
ntp:x:87:87:Network Time Protocol:/var/lib/ntp:/bin/false
polkitd:x:102:102:PolicyKit daemon:/:/sbin/nologin
usbmux:x:140:140:usbmux user:/:/sbin/nologin
evanmeek:x:1000:1000:EvanMeek:/home/evanmeek:/usr/bin/zsh
nvidia-persistenced:x:143:143:NVIDIA Persistence Daemon:/:/sbin/nologin
privoxy:x:42:42:Privoxy:/:/sbin/nologin
~~~

__显示文件最后5行__

_第一种写法_

~~~
> tail -n 5 /etc/passwd
~~~

_第二种写法_

~~~
> tail -5 /etc/passwd
~~~

__指定从第几行开始显示文件__

~~~
> tail -n +10 /etc/passwd
~~~

__实时监控文件的变化__

监测
~~~
> tail -f --retry test.txt
testwords
~~~
追加内容

~~~
> echo testwords >> test.txt
~~~

# 3.7 tailf 跟踪日志文件

`tailf`命令日常工作中用于跟踪日志文件，它与`tail -f `命令基本相同，但唯一的区别是若文件内容无增加，则不会重复访问磁盘文件，也不会修改文件访问时间。

语法格式

~~~
tailf [选项] [文件]
~~~

`tailf`命令的参数选项及说明

| 选项     | 说明                               |
|----------|------------------------------------|
| -n<行数> | 指定显示的行数，默认为文件最后10行 |

例子：

__跟踪日志文件__

~~~
> tailf 
~~~

# 3.8 cut 从文本中提取一段文字并输出

`cut`命令从文件的每一行剪切字节，字符或字段，并将其输出至标准输出。

语法格式

~~~
cut [选项] [文件]
~~~

`cut`命令的参数选项及说明

| 选项 | 说明                                |
|------|-------------------------------------|
| -b   | 以字节为单位进行分割                |
| -n   | 取消分割多字节字符                  |
| -c   | 以字符为单位进行分割`!`             |
| -d   | 自定义分隔符，默认以tab为分隔符`!`  |
| -f   | 与选项-d一起使用，指定显示哪个区域  |
| N    | 第N个字节、字符或字段`!`            |
| N-   | 从第N个字节、字符或字段开始直至行尾 |
| N-M  | 从第N到第M个字节、字符或字段        |
| -M   | 从第一到第M个字节、字符或字段       |

例子:

__以字节为分隔符__

~~~
> cat test.txt
Hello World
# 截取第5个字节，并输出
> cut -b 5 test.txt
o
# 截取第5个字节之前的所有字符(包括第5个)，并输出
> cut -b -5 test.txt
Hello
# 截取第5个字节之后的所有字符，并输出
> cut -b 5- test.txt
o World
# 截取第5个字节和第10个字节，并输出
> cut -b 5,10 test.txt
ol
# 截取第1个字节到第5个字节，并输出
> cut -b 1-5 test.txt
Hello
~~~

__以字符为分隔符__

~~~
> cat test.txt
Hello World
你好世界
# 截取第1到第6个字符，并输出
> cut -b 1-6 test.txt
Hello
你好
~~~

__自定义分隔符__

~~~
> cat /etc/passwd | head -n 5
root:x:0:0::/root:/bin/bash
nobody:x:65534:65534:Nobody:/:/sbin/nologin
dbus:x:81:81:System Message Bus:/:/sbin/nologin
bin:x:1:1::/:/sbin/nologin
daemon:x:2:2::/:/sbin/nologin
# 自定义分隔符为`:`，并且之输出文件前5行(:为分隔符，-f 1为指定显示第1个区域
> cut -d : -f 1 /etc/passwd | head -n 5
root
nobody
dbus
bin
daemon
# 指定显示区域为1,6-8
> cut -d : -f 1,6-8 /etc/passwd | head -n +5
~~~

# 3.9 split 分割文件

`split`命令可以将文件进行分割，支持根据行数或文件大小进行分割。

语法格式

~~~
split [选项] [输入文件] [输出文件名[前缀]]
~~~

_输出文件的格式会加上前缀，例如`PREFIXaa,PREFIXab`_

`split`命令的参数选项及说明

| 选项 | 说明                        |
|------|-----------------------------|
| -b   | 指定分割后文件的最大字节数  |
| -l   | 指定分割后文件的最大行数`!` |
| -a   | 指定后缀长度，默认为2位字母 |
| -d   | 使用数字后缀                |

例子:

__按行分割文件，以及指定后缀形式__

~~~
# wc 命令可以查看文件的行数
> wc -l /etc/passwd
31 /etc/passwd
# 按行进行分割，每10行分割为一个新文件，文件前缀为split_
> split -l 10 /etc/passwd split_
> wc -l split_*
10 split_aa
10 split_ab
10 split_ac
 1 split_ad
31 总用量
# 参数-a指定分割文件的前缀长度，这里设置的是1
> split -l 10 -a 1 /etc/passwd split2_
> wc -l split2_*
10 split2_a
10 split2_b
10 split2_c
 1 split2_d
31 总用量
# 参数-d指定文件使用数字后缀
> split -l 10 -d -a 1 /etc/passwd split_
> wc -l split_*
10 split_0
10 split_1
10 split_2
 1 split_3
31 总用量<Paste>
~~~

__按文件大小分割文件__

~~~
# 准备测试文件
> cp /sbin/lvm .
> ls -l
总用量 2.3M
-r-xr-xr-x 1 evanmeek evanmeek 2.3M  7月 22 21:23 lvm
# 按文件字节进行分割，每500K字节分割为一个文件，以数字作为文件后缀
> split -b 500K -d lvm lvm_split_
> ls -l
总用量 4.5M
-r-xr-xr-x 1 evanmeek evanmeek 2.3M  7月 22 21:23 lvm
-rw-r--r-- 1 evanmeek evanmeek 500K  7月 22 21:25 lvm_split_00
-rw-r--r-- 1 evanmeek evanmeek 500K  7月 22 21:25 lvm_split_01
-rw-r--r-- 1 evanmeek evanmeek 500K  7月 22 21:25 lvm_split_02
-rw-r--r-- 1 evanmeek evanmeek 500K  7月 22 21:25 lvm_split_03
-rw-r--r-- 1 evanmeek evanmeek 254K  7月 22 21:25 lvm_split_04
~~~

# 3.10 paste 合并文件

`paste`命令能将文件按照行与行进行合并，中间使用tab隔开

语法格式
~~~
paste [选项] [文件]
~~~

`paste`命令的参数选项及说明

| 选项 | 说明                           |
|------|--------------------------------|
| -d   | 指定合并的分隔符，默认是Tab`!` |
| -s   | 每个文件占用一行               |

例子:

__合并文件__

~~~
> more t1 t2
::::::::::::::
t1
::::::::::::::
test1
test1
test1
::::::::::::::
t2
::::::::::::::
test2
test2
test2
# t1与t2合并，将内容写入t3
> paste t1 t2 > t3
> cat t3
test1	test2
test1	test2
test1	test2
# 合并时，自定义分隔符
> paste -d - t1 t2 > t3
> cat t3
test1-test2
test1-test2
test1-test2
~~~

__文件内容合并成一行__

~~~
> paste -s t1 t2 > t3
> cat t3
test1	test1	test1
test2	test2	test2
~~~

# 3.11 sort 文本排序

`sort`命令将输入的文件内容按照指定的规则进行排序，然后将排序结果输出

语法格式

~~~
sort [选项] [文件]
~~~

`sort`命令的参数选项及说明

| 选项 | 说明                       |
|------|----------------------------|
| -b   | 忽略每行开头存在的空格字符 |
| -n   | 按照数值的大小进行排序`!`  |
| -r   | 倒序排序`!`                |
| -u   | 去除重复行                 |
| -t   | 指定分隔符`!`              |
| -k   | 按照指定区间排序`!`        |

例子:

__默认以行为单位进行排序__

~~~
$ cat test.txt
t1
t2
t3
t4
# sort默认是以ASCII码进行升序排序。
$ sort test.txt
t1
t2
t3
t4
# 按照数值的大小进行排序
$ sort -n test.txt
t1
t2
t3
t4
# 按照数值的大小降序排序
$ sort -nr test.txt
t4
t3
t2
t1
~~~

__去除重复行__

~~~
$ cat test.txt
t1 D
t1 D
t1 D
t2 C
t3 B
t4 A
# 去除重复行，并且逆序排序
$ sort -u test.txt | sort -nr
t4 A
t3 B
t2 C
t1 D
~~~

__自定义区间排序__

~~~
# 指定以空格作为分隔符，并且以第二列进行逆序排序
$ sort -t " " -k2 test.txt
t4 A
t3 B
t2 C
t1 D
t1 D
t1 D
~~~

__-t -k的进阶用法__

~~~
$ cat ip.txt
192.197.113.0	192.197.113.255	256
193.112.0.0	193.112.255.255	65536
195.78.82.0	195.78.83.255	512
198.175.100.0	198.175.103.255	1024
199.212.57.0	199.212.57.255	256
202.0.100.0	202.0.101.255	512
202.0.122.0	202.0.123.255	512
202.0.176.0	202.0.179.255	1024
202.3.128.0	202.3.129.255	512
202.3.134.0	202.3.134.255	256
202.4.128.0	202.4.159.255	8192
202.4.252.0	202.4.255.255	1024
# 以第3个字段的第1个字符到第3个字段的第3个字段进行数字排序
$ sort -n -t. -k3.1,3.3 ip.txt
193.112.0.0	193.112.255.255	65536
199.212.57.0	199.212.57.255	256
195.78.82.0	195.78.83.255	512
198.175.100.0	198.175.103.255	1024
202.0.100.0	202.0.101.255	512
192.197.113.0	192.197.113.255	256
202.0.122.0	202.0.123.255	512
202.3.128.0	202.3.129.255	512
202.4.128.0	202.4.159.255	8192
202.3.134.0	202.3.134.255	256
202.0.176.0	202.0.179.255	1024
202.4.252.0	202.4.255.255	1024
~~~

# 3.12 join 按两个文件的相同字段合并

`join`命令针对每一对具有相同内容的输入行，整合为一行输出到标准输出，默认情况下是把输入的第一个字段作为连接字段，字段之间用空格隔开。

语法格式

~~~
join [选项] [文件1] [文件2]
~~~

`join`命令的参数选项及说明

| 选项     | 说明                                                    |
|----------|---------------------------------------------------------|
| -a文件号 | 输出文件中不匹配的行，文件号可选值1或2,代表文件1和文件2 |
| -i       | 比较字段时忽略大小写                                    |
| -1 字段  | 以第1个文件的指定字段为基础进行合并                     |
| -2 字段  | 以第2个文件的指定字段为基础进行合并                     |

例子:

__合并文本__

~~~
$ more t1.txt t2.txt
::::::::::::::
t1.txt
::::::::::::::
张三 21岁
李四 23岁
王五 17岁
赵六 15岁

::::::::::::::
t2.txt
::::::::::::::
张三 男
李四 女
王五 男
赵六 女
# 将t1和t2进行合并
$ join t1.txt t2.txt > t3.txt
$ cat t3.txt
张三 21岁 男
李四 23岁 女
王五 17岁 男
赵六 15岁 女
~~~
