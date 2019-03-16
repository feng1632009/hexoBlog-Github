---
title: Pythoner必会的Linux命令
date: 2017-11-2 15:25:00
author: 空灵
img:  /medias/featureimages/3.jpg
top: false
cover: false
coverImg: 
password: 8d969eef6ecad3c29a3a629280e686cf0c3f5d5a86aff3ca12020c923adc6c92
toc: true
mathjax: false
summary: Pythoner必会的Linux命令
categories: Linux
tags:
  - Linux
---


# Pythoner 必会的linux命令

> 起因

在日常工作中，学习linux是很痛苦的过程，此文主要列出日常开发所用常用命令行总结，并非linux命令完全指南性学习

> 入门必看

##### Linux是什么

`Linux` 是一个开源系统内核，1991年由托瓦兹 (Linus Torvalds) 开发出来的，我们平时讲的 `Liunx` 系统其实并不是很准确，`Ubuntu` `CentOS` 这些才是系统，`Linux` 是参考 `Unix` 系统开发出来的。

现实生活中有哪些软件或系统是基于 `Linux` 开发出来的呢？ `嵌入式` 程序开发 `Android` 手机系统，我们经常浏览的网站服务器，基本都是运行 `Linux` 系统之上。

`Linux` 是一个支持多用户、多任务的系统。

`Linux` 系统上一切都是以文件的形式存在，文件和目录名称都区分大小写的。所有文件命名要体现文件的格式或内容，`demo.php` 代表这是一个 `php` 文件，`demo.txt` 代表 `txt` 文件，`demo.tar.gz` 代表以 `gzip`压缩的打包文件。还有一种特殊文件，有文件也有目录，文件名称以 `.` 开头的隐藏文件，例如： `.vim` 默认是不显示的，一般都是软件的配置文件。

无论是源码编译安装还是通过包管理器 `yum` 或 `apt-get` 安装的软件，默认都是需要设置开机自启动，单纯的启动服务，如果服务器有重启的话，程序无法运行，还要登录服务器排查原因，费事费力。

##### 关于Linux命令的几个分类

- 内置命令
  - 例如 `cd` `ls` 这些命令，默认系统内置无需安装
- 外置命令
  - 例如 `ccat` `wget` ,如果执行命令提示 command not found ,通常都是需要用户自己使用 `apt-get` 或 `yum` 软件包管理器安装
- 程序或软件命令
  - 例如 `nginx` `php` 或者自己写的程序编译出来的命令，这种命令与外置命令类似，但是会有配置等其他文件，外置命令一般安装之后只有一个二进制可执行文件

##### Linux的优势

- 跨平台
- 安全
- 多用户多任务
- 占用系统资源少
- 网络功能强大
- 稳定性 多作为企业服务器使用

##### Linux的运行级别

- 0停机，关机
- 1 单用户，无网络连接，不运行守护进程，仅root用户可以登录
- 2 多用户， 无网络连接，不允许守护进程
- 3 多用户，正常启动系统
- 4 用户自定义
- 5 多用户，带图形界面
- 6 重启

Linux的启动流程

1. 加载内核
2. 启动初始化进程
3. 确定运行级别
4. 加载开机启动程序
5. 用户登录
6. 进入 login shell
7. 打开 non-login shell

##### Linux 的目录结构

Ubuntu 16.04

```
/bin            #用户二进制文件
/boot           #启动核心文件
/dev            #设备文件
/etc            #配置文件
/home           #用户的主目录，在Linux中，每个用户都有一个自己的目录，一般该目录名是以用户的账号命名的
/lib            #系统库
/lib64          #系统库
/lost+found     #这个目录一般情况下是空的，当系统非法关机后，这里就存放了一些文件
/media          #可移动媒体设备
/mnt            #挂载目录
/opt            #用户安装的软件目录
/proc           #进程信息
/root           #该目录为系统管理员，也称作超级权限者的用户主目录
/run            #存放进程的I
/sbin           #s就是Super User的意思，这里存放的是系统管理员使用的系统管理程序
/tmp            #系统临时文件
/srv            #srv是service的缩写，该目录存放一些服务启动之后需要提取的数据
/sys            #这个目录跟/proc 非常类似，也是一个虚拟的文件系统，主要也是记录与内核相关的信息。
/usr            #注意 usr 并不是 user 的缩写，而是Unix Software Resource的缩写，即 Unix 操作系统软件资源放在该目录，而不是用户的数据。
/var            #软件运行所产生的文件
```

补充: 其他 `Linux` 系统的发行版目录大致一致

cd-ls pwd 

#### cd 命令 change dirname 的缩写形式，进入指定目录

 

```
cd /usr/local #进入 /usr/local 目录

cd / #进入 / 目录

cd ~ #进入当前用户家目录

cd - #进入执行上一条命令所在目录

cd ../ #进入当前目录的上一级目录
```

补充：`.` 代表当前目录，`..` 代表上一级目录，`~` 代表当前用户的家目录，`/` 代表根目录，根目录的上一级目录指向的还是根目录。推荐安装autojump插件进行目录跳转

关于文件名的重要规则

> 1. 以 “.” 字符开头的文件名是隐藏文件。这仅表示，ls 命令不能列出它们， 用 ls -a 命令就可以了。当你创建帐号后，几个配置帐号的隐藏文件被放置在 你的家目录下。稍后，我们会仔细研究一些隐藏文件，来定制你的系统环境。 另外，一些应用程序也会把它们的配置文件以隐藏文件的形式放在你的家目录下面。
>
> 2. 文件名和命令名是大小写敏感的。文件名 “File1” 和 “file1” 是指两个不同的文件名。
>
> 3. Linux 没有“文件扩展名”的概念，不像其它一些系统。可以用你喜欢的任何名字 来给文件起名。文件内容或用途由其它方法来决定。虽然类 Unix 的操作系统， 不用文件扩展名来决定文件的内容或用途，但是有些应用程序会。
>
> 4. 虽然 Linux 支持长文件名，文件名可能包含空格，标点符号，但标点符号仅限 使用 “.”，“－”，下划线。最重要的是，不要在文件名中使用空格。如果你想表示词与 词间的空格，用下划线字符来代替。过些时候，你会感激自己这样做。

 

#### ls 命令 以列表的形式显示文件

```
ls 显示当前目录的文件和目录，不包含隐藏文件

ls -a 显示所有文件和目录，包含隐藏文件

ls -l #显示文件和文件夹的详细信息

ll #ls -l 命令的简写

ls -A #只显示隐藏的文件和目录，不显示 . 和 ..

ls -R dirname #递归显示目录的所有内容

ls -t #按修改时间排序显示

ls -lS #按文件从大到小排序

ls -lh #将文件大小转成 b kb mb g 的形式

ls -al #以长格式显示目录和文件的内容列表，包含隐藏文件。输出的信息从左到右依次包括文件名，文件类型、权限模式、硬连接数、所有者、组、文件大小和文件的最后修改时间等
```

#### pwd 显示所在位置的命令 print work dirname 简写

```
pwd #显示所在位置
```

> 用户与用户组

##### user-group

#### users 命令

```
users #查看当前在线的用户名称列表
```

#### 用户分 3 种

1. root 用户，即是超级管理员
2. 拥有root权限的用户，可以执行 sudo 命令
3. 普通用户

#### 用户文件与配置

- /etc/password
- /etc/shadow

#### adduser命令

```
adduser demo #添加账号
```

- 补充：此命令在 CentOS7.2 系统中为软连接，源目标是 adduser 命令。在 Ubuntu16.04 中是独立的命令，添加账号时会有友好的提示，设置密码账号信息等

#### useradd 命令

```
useradd demo #添加用户，默认家目录在 /home/ 目录下 demo 目录，与用户名一致

passwd demo #设置密码，默认添加用户之后没设置密码不允许登录

useradd -u 2333 demo #添加用户并指定用户 id ，一般情况很少使用

useradd -d /home/testuser demo #添加用户并指定家目录，一般情况很少使用

useradd -G xxx,xxx demo #添加用户并加入多个附加组
```

#### usermod 命令

```
usermod -L demo #锁定用户，不允许登录，相当于手动修改 /etc/shadow 文件中用户密码前加入 ! 一个感叹号

usermod -U demo #解除锁定
```

#### userdel 命令

```
userdel demo #删除用户，不删除用户家目录数据

userdel -r demo #删除用户并删除用户家目录

userdel -rf demo #强制删除用户无论是否登录，并删除用户家目录
```

#### passwd 目录

```
passwd -l demo #锁定用户，不允许登录，相当于手动修改 /etc/shadow 文件中用户密码前加入 !! 双感叹号

passwd -u demo #解除锁定
```

#### groups 命令

```
groups #查看当前用户的用户组名称
```

#### 用户组分两种

1. 初始组(默认组)
2. 其他组(附加组)

#### 用户组文件与配置

- /etc/group
- /etc/gshadow

#### addgroup 命令

```
groupadd test #添加用户组
```

#### groupmod 命令

```
groupmod -n new_test test #讲 test 组名修改为 new_test
```

#### gpasswd 命令

```
gpasswd -a nick new_test #将 nick 用户添加到 new_test 组

gpasswd -d nick new_test #将 nick 用户从 new_test 组删除
```

#### groupdel 命令

```
groupdel new_test #删除用户组
```

补充：每添加一个用户会自动创建一个用户组，称为默认组或初始组，这个组是不允许删除的，默认组一般不修改最好，用户 id 也一样。

#### sudo 命令

```
sudo vim /etc/passwd #以另一个用户身份执行命令，通常是 root 用户
```

补充：普通用户添加执行 `sudo` 的方法，在 `/etc/sudoers` 文件中添加 `username ALL=(ALL) ALL` 即可。

#### su 命令

```
su   #切换到 root 用户，仅切换路径

su - #切换到 root 用户，环境变量和路径一起切换

su - nick #切换到 nick 用户，环境变量和路径一起切换
```

#### whoami 命令

```
whoami #查看当前用户的名称
```

#### id 命令

```
id #查看当前用户的 id 、组 id 、附加组 id 

id -un #查看当前用户的名称

id -g #当前用户的组 id

id -G #当前用户的附加组 id

id -u root #查看指定用户的 id

id root #查看指定用户的 id gid groupsid
```

#### who 命令

```
who #查看当前的在线用户、终端号、IP、登录时间

who -q #查看在线的用户的用户名，用户数
```

> 文件与目录

file-dir

#### touch 命令

```
touch index.html #新建空文件
```

#### mkdir 命令

```
mkdir demo #创建目录

mkdir -p ~/python/flask #递归创建目录

mkdir -m 755 www #创建目录并指定权限
```

#### rmdir 命令

```
rmdir demo #删除空目录，如果目录有文件无法删除
```

#### cp 命令

```
cp index.html html5 #复制 index.html 文件到当前 html5 目录下

cp -a index.html html5 #复制 index.html 文件到当前 html5 目录下,保留文件的属性、权限、软连接自动指向目标文件

cp index.html ~/app.html5 #复制文件到指定目录下并修改文件名

cp -R html5 html #递归复制 html5 目录下所有文件到 html 目录
```

#### mv 命令

```
mv index.html html5.html #将 index.html 名称更改为 html5.html

mv ~/python /home #将家目录下 python 目录移动到根目录 home 目录下
```

#### rm 命令

```
rm index.html #删除文件

rm -i index.html #执行删除询问 y 代表删除 n 代表取消

rm -r test #递归删除目录

rm -f test #强制删除目录

rm -rf python #强制递归删除目录
```

#### unlink

```
unlink demo.php #删除文件，与 rm 类似，但只能删除文件
```

#### tree 命令

```
tree ~ #以树状图列出指定目录的内容

tree -d laravel #以树状图列出指定目录的内容，不包括文件

tree ~/test -H '目录结构' -o out.html --nolinks #将内容以树状形式写入到 HTML 文件中，不包含链接
```

#### file 命令

```
file demo.php #查看文件格式类型
```

> 文件目录权限

file-dir-permissions

#### Linux 的文件类型

1. 普通文件, 符号为 `-`
2. 目录, 符号为 `d`
3. 字符设备，符号为 `c`
4. 块装备，符号为`b`
5. 套接字文件 符号为`c`
6. 软链接文件，类似 `Windows` 系统的快捷方式，删除了并不会影响源文件，符号为 `l`
7. 管道文件，符号为 `p`

补充：常见的文件类型为 `-` `d` `l`

#### Linux 的 3 种权限

- `r` 代表 `read` 读权限，数字代表为 `4`
- `w` 代表 `wirte` 写权限，数字代表为 `2`
- `x` 代表 `execute` 执行权限，数字代表为 `1`

#### Linux 的 3 种用户

- 拥有者（owner)
- 用户组（group）
- 其它人（others）

#### 解读

```
-rw-rw-r-- 1 nick nick 0 May 12 16:05 demo.html

- 第 1 列代表文件类型
rw- 第 2-4 列代表所属用户的权限
rw- 第 5-7 列代表所属用户组的权限
r-- 第 8-11 其他用户所属权限
1 第 11 列代表文件被引用的次数
nick 第 12 列代表文件所属用户名称
nick 第 13 列代表文件所属用户组名称
0 第 14 列代表文件大小
May 12 16:05 第 15-17 列代表文件最后的修改时间
demo.html 第 18 列代表文件名称
```

### stat 命令

```
stat filename #查看文件的详细信息，文件的访问时间、修改时间、改变时间，linux 文件没有创建时间
```

#### chmod 命令

```
chmod 755 filename #将文件权限修改为 755 ,即拥有者有读写执行权限，用户组有读执行权限，其他人有读执行权限 

chmod -R 755 dirname #递归修改目录的权限
```

#### chown 命令

```
chown nick index.html #将文件拥有者改为 nick

chown :nick index.html #将文件用户组修改为 nick

chown nick:nick index.html #将文件拥有者和用户组修改为 nick

chown nick python #将目录拥有者修改为 nick

chown :nick python #将目录用户组修改为 nick

chown nick:nick python #将目录拥有者和用户组修改为 nick

chown -R nick:nick python #递归将目录拥有者和用户组修改为 nick
```

#### chgrp 命令

```
chgrp nick demo #改变目录的所属用户组

chgrp nick test.html #改变文件的所属用户组

chgrp -R nick demo #递归改变目录的所属用户组
```

#### umask 命令

默认的补码为 022 ,补码越小权限越大，补码的计算规则为 rwx 读写执行即 777 减补码是权限数字 root 的 umask 为 022 ，创建的目录或文件为 755 rwxr-xr-x 权限 普通用户的 umask 为 002 ，创建的目录或文件为 775 rwxrwxr-x 权限 最大权限减 umask 等于默认权限，结果为奇数，则奇数位 +1

```
umask #查看权限补码

umask -S #查看当前用户权限补码
```

#### chattr 命令

```
chattr +i index.php #设置文件不允许修改、删除、移动、复制，root 用户也生效

chattr -i index.php #取消文件属性设置

chattr +i demo #设置目录属性，目录内的文件目录只能修改，不能新建与修改，子目录的文件目录不生效，目录层级只有一层

```

#### lsattr 命令

`

```linux
lsattr filename  #查看文件属性`
`lsattr -d demo #查看目录属性`
`lsattr -R demo #递归查看目录属性`
`lsattr -a demo #查看所有目录文件隐藏文件
```

