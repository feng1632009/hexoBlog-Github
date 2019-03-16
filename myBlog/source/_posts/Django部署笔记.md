---
title: Django部署指南
date: 2018-07-07 09:25:00
author: 空灵
img:  /medias/featureimages/6.jpg
top: true
cover: true
coverImg: 
password: 8d969eef6ecad3c29a3a629280e686cf0c3f5d5a86aff3ca12020c923adc6c92
toc: true
mathjax: false
summary: Django部署指南
categories: Django
tags:
  - Django
  - Python
---
##### 本地环境 mac os  连接远程软件 ssh shell   服务器系统 centos 7



##### 一 ：安装 python3.6

```
wget https://www.python.org/ftp/python/3.6.2/Python-3.6.2.tgz
```

 解压缩

```
tar -xzvf Python-3.6.2.tgz -C  /tmp
```

 cd到指定目录

```python
cd  /tmp/Python-3.6.2/
```

```python
把Python3.6安装到 /usr/local 目录
```

```python
./configure --prefix=/usr/local  
有几率出现
configure: error: in `/tmp/Python-3.6.2':
configure: error: no acceptable C compiler found in $PATH
See `config.log' for more details 此种错误
#由于本机缺少gcc编译环境 请运行  yum install -y gcc
```

```python
make
```

```python
make altinstall  (altinstall在安装时会区分已存在的版本)
```



```python
更改/usr/bin/python链接

ln -s /usr/local/bin/python3.6 /usr/bin/python3



```

验证是否安装成功 出现此图则代表安装成功

![image-20180729224426236](https://ws2.sinaimg.cn/large/006tKfTcgy1ftr45udzsxj315q066jsr.jpg)





##### 二 ：安装 mysql

1、先检查系统是否装有mysql

rpm -qa | grep mysql

下载mysql的repo源

```linu
# wget http://repo.mysql.com/mysql-community-release-el7-5.noarch.rpm
```



2、安装mysql-community-release-el7-5.noarch.rpm包

```
# sudo rpm -ivh mysql-community-release-el7-5.noarch.rpm

```

3、安装mysql

```
# sudo yum install mysql-server
```

 根据步骤安装就可以了，不过安装完成后，没有密码，需要重置密码。

安装后再次查看mysql

![image-20180729225800704](https://ws2.sinaimg.cn/large/006tKfTcgy1ftr4jxiqq1j30x408240q.jpg)



此处可能存在报错

```
Error: Package: mysql-community-libs-5.6.35-2.el7.x86_64 (mysql56-community) Requires: libc.so.6(GLIBC_2.17)(64bit) Error: Package: mysql-community-server-5.6.35-2.el7.x86_64 (mysql56-community) Requires: libc.so.6(GLIBC_2.17)(64bit) Error: Package: mysql-community-server-5.6.35-2.el7.x86_64 (mysql56-community) Requires: systemd Error: Package: mysql-community-server-5.6.35-2.el7.x86_64 (mysql56-community) Requires: libstdc++.so.6(GLIBCXX_3.4.15)(64bit) Error: Package: mysql-community-client-5.6.35-2.el7.x86_64 (mysql56-community) Requires: libc.so.6(GLIBC_2.17)(64bit) You could try using --skip-broken to work around the problem You could try running: rpm -Va --nofiles --nodigest
```

解决方法 

```
#yum install glibc.i686
# yum list libstdc++* 
# 本机未曾出现 只是暂时做记录
```



4、重置密码

重置密码前，首先要登录

mysql - u root

> 登录时有可能报这样的错：ERROR 2002 (HY000): Can’t connect to local MySQL server through socket ‘/var/lib/mysql/mysql.sock’ (2)，原因是/var/lib/mysql的访问权限问题。下面的命令把/var/lib/mysql的拥有者改为当前用户：

解决命令

```
sudo chown -R openscanner:openscanner /var/lib/mysql
```

此时可能会报错误

如果报`chown: 无效的用户: "openscanner:openscanner"`错误，更换命令，并用 ll 查看目录权限列表

```
chown root /var/lib/mysql/ 
ll

命令解释：
附： 
① 更改文件拥有者 (chown ) 
[root@linux ~]# chown 账号名称 文件或目录 
② 改变文件的用户组用命令 chgrp 
[root@linux ~]# chgrp 组名 文件或目录 
③ 对于目录权限修改之后，默认只是修改当前级别的权限。如果子目录也要递归需要加R参数 
Chown -R : 进行递归,连同子目录下的所有文件、目录
```



重启mysql服务

```
service mysqld restart
```

接下来登录重置密码

```mysql
 mysql -u root -p
```

 出现入下图界面则成功

![image-20180729231001119](https://ws2.sinaimg.cn/large/006tKfTcgy1ftr4wfyg5bj31ak0hidj9.jpg)



接下来登录重置密码：

```mysql
mysql -u root -p
mysql > use mysql;
mysql > update user set password=password('123456') where user='root';
mysql > exit;
```

**重启mysql服务后才生效** `# service mysqld restart`

 必要时加入以下命令行，为root添加远程连接的能力。链接密码为 “root”（不包括双引号）

```mysql
mysql> GRANT ALL PRIVILEGES ON *.* TO root@"%" IDENTIFIED BY "root";　
```

查询数据库编码格式，确保是 UTF-8

```mysql
show variables like "%char%";
```

修改数据库编码

1、进入编辑my.cnf

```
vi /etc/my.cnf
```

2、 在[mysqld]**下边**添加以下内容

```
character-set-server=utf8

init_connect='SET NAMES utf8'
```

3，在[mysqld]**上边**添加以下内容

```
 [mysql]

 default-character-set=utf8
```

![image-20180729232831715](https://ws2.sinaimg.cn/large/006tKfTcgy1ftr5foybfmj31g20mon1q.jpg)

如上图所示



重启MySQL

```mysql
 systemctl restart mysqld
```

然后查看mysql编码

```
mysql> show variables like '%character%';
```

修改完成后显示结果如下：

![image-20180729233139497](https://ws4.sinaimg.cn/large/006tKfTcgy1ftr5iy84g0j31260gggo2.jpg)



##### 三、安装mysqlclient

```
yum install python-devel mariadb-devel -y
```

```
pip3 install mysqlclient
然后发现没有pip命令 开始如下安装操作
```

1、安装pip

安装依赖环境

```
yum -y install zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gdbm-devel db4-devel libpcap-devel xz-devel
```



2.**安装setuptools**

```
wget --no-check-certificate  https://pypi.python.org/packages/source/s/setuptools/setuptools-19.6.tar.gz#md5=c607dd118eae682c44ed146367a17e26

tar -zxvf setuptools-19.6.tar.gz

cd setuptools-19.6

python3 setup.py build

python3 setup.py install #这步可能会引起下面异常
```

 中间可能会报错如

```
RuntimeError: Compression requires the (missing) zlib module
```

```
yum install zlib

yum install zlib-devel
```

安装完成后，重新编译 python3.6【不需要删除，只需要重新编译，make，安装就行了】

> (cd到原来安装python的目录 直接执行make命令)
>
> 然后在切回来这个目录 重新运行python3 setup.py install 

成功之后设置软连接

```
ln -s /usr/local/bin/pip3.6 /usr/bin/pip3 
```

如软链接设置错误,删除软链接命令

 **rm -rf /usr/bin/pip3(后面/usr/bin/pip为软链接名称,切记不能加结尾/  如:/usr/bin/pip3/则表示删除软连接及真实文件)**

输入pip3-V 出现入下图所示 即为安装成功

![image-20180730064148611](https://ws4.sinaimg.cn/large/006tKfTcgy1ftrhyi9834j314203wjsa.jpg)





##### 四、安装virtualenvwrapper

```python
yum install python-setuptools python-devel
pip install virtualenvwrapper
```

 编辑.bashrc文件

```python
export WORKON_HOME=$HOME/.virtualenvs
source /usr/local/bin/virtualenvwrapper.sh
```

```
重新加载.bashrc文件
source  ~/.bashrc

新建虚拟环境
mkvirtualenv test

进入虚拟环境 
workon test

出现下图界面则代表虚拟环境创建成功
```

![image-20180730064739942](https://ws1.sinaimg.cn/large/006tKfTcgy1ftri4lfw09j30pg028weu.jpg)





安装nginx

## 第一步 - 添加Nginx存储库

要添加CentOS 7 EPEL仓库，请打开终端并使用以下命令：

```
sudo yum install epel-release
```

## 第二步 - 安装Nginx

现在Nginx存储库已经安装在您的服务器上，使用以下`yum`命令安装Nginx ：

```
sudo yum install nginx
```

在对提示回答yes后，Nginx将在服务器上完成安装。

## 第三步 - 启动Nginx

Nginx不会自行启动。要运行Nginx，请输入：

```
sudo systemctl start nginx
```

如果您正在运行防火墙，请运行以下命令以允许HTTP和HTTPS通信：

```
sudo firewall-cmd --permanent --zone=public --add-service=http 



sudo firewall-cmd --permanent --zone=public --add-service=https



sudo firewall-cmd --reload
```

您将会看到默认的CentOS 7 Nginx网页，这是为了提供信息和测试目的。

它应该看起来像这样：

![image-20180805180811338](https://ws1.sinaimg.cn/large/0069RVTdgy1ftyzikyrorj31kw0l9gpw.jpg) 

 导出本地开发环境

进入虚拟环境 

输入命令  pip freeze > requirements.txt 会生成一个文件![image-20180805181335542](https://ws2.sinaimg.cn/large/0069RVTdgy1ftyzo5yqovj31dq02i755.jpg)

复制内容或者直接拷贝文件到服务器

进入服务器的虚拟环境 

pip install -r requirements.txt



通过第三方软件或者sftp命令把本地代码传输到远端服务器

安装uwsgi

pip install uwsgi



把本地的数据库传输到服务器 如图 一定不要勾选其他选项 防止出现失败 如下图

![image-20180805185257398](https://ws4.sinaimg.cn/large/0069RVTdgy1ftz0t4he6bj312m0ss449.jpg)



在服务器启动项目测试

虚拟环境：python manage.py runserver 0.0.0.0:8000 

ALLOWED_HOSTS = ['*']  

输入ip+端口正常访问 

![image-20180805203632618](https://ws4.sinaimg.cn/large/0069RVTdgy1ftz3t2waojj31kw0sznpd.jpg)



uwsgi 启动项目 

uwsgi --http :8000 --module MxOnline_py3.wsgi 

一般是进入项目的根目录 --module指向 项目里面的wsgi

![image-20180805204018588](https://ws2.sinaimg.cn/large/0069RVTdgy1ftz3wtbz34j31kw02g0te.jpg)

开启之后测试能否拉起django 项目

新建一个uc_nginx.conf文件

> \# the upstream component nginx needs to connect to
>
> upstream django {
>
> \# server unix:///path/to/your/mysite/mysite.sock; # for a file socket
>
> server 127.0.0.1:8000; # for a web port socket (we'll use this first)
>
> }
>
> \# configuration of the server
>
> server {
>
> \# the port your site will be served on
>
> listen      80;
>
> \# the domain name it will serve for
>
> server_name etreal.xyz ; # substitute your machine's IP address or FQDN
>
> charset     utf-8;
>
> \# max upload size
>
> client_max_body_size 75M;   # adjust to taste
>
> \# Django media
>
> location /media  {
>
> ​    alias /MxOnline_py3/media;  # 指向django的media目录
>
> }
>
> location /static {
>
> ​    alias /MxOnline_py3/static; # 指向django的static目录
>
> }
>
> \# Finally, send all non-media requests to the Django server.
>
> location / {
>
> ​    uwsgi_pass  django;
>
> ​    include     uwsgi_params; # the uwsgi_params file you installed
>
> }
>
> }



server_name 可以配置ip或者域名

##### 将该配置文件加入到nginx的启动配置文件中

```
sudo ln -s /MxOnline_py3/uc_nginx.conf  /etc/nginx/conf.d/
```





##### 拉取所有需要的static file 到同一个目录

```
在django的setting文件中，添加下面一行内容：

    STATIC_ROOT = os.path.join(BASE_DIR, "static/")
运行命令
    python manage.py collectstatic
    
   
```

注意 写上STATIC_ROOT 注销掉 STATICFILES_DIRS 如图

![image-20180805220244860](https://ws1.sinaimg.cn/large/0069RVTdgy1ftz6amuic2j316u0tsten.jpg)

在conf文件下 建立

```
新建uwsgi.ini 配置文件， 内容如下：

    # mysite_uwsgi.ini file
    [uwsgi]

    # Django-related settings
    # the base directory (full path)
    chdir           = /home/bobby/Projects/MxOnline
    # Django's wsgi file
    module          = MxOnline.wsgi
    # the virtualenv (full path)

    # process-related settings
    # master
    master          = true
    # maximum number of worker processes
    processes       = 10
    # the socket (use the full path to be safe
    socket          = 127.0.0.1:8000
    # ... with appropriate permissions - may be needed
    # chmod-socket    = 664
    # clear environment on exit
    vacuum          = true
    virtualenv = /home/bobby/.virtualenvs/mxonline
    
    
    注：
    chdir： 表示需要操作的目录，也就是项目的目录
    module： wsgi文件的路径
    processes： 进程数
    virtualenv：虚拟环境的目录
```

uwsgi 带着配置文件启动 注意目录结构

![image-20180805222316371](https://ws4.sinaimg.cn/large/0069RVTdgy1ftz6vxo1ptj30ye034dgf.jpg)

此时访问域名

http://etreal.xyz/

即可出现如图结果

![image-20180805222546460](https://ws2.sinaimg.cn/large/0069RVTdgy1ftz6yq6r22j31kw0thnpd.jpg)





pkill -f uwsgi  自动重启并加载

pkill -f -9 uwsgi   停止wsgi 

注意 setting debug 一定要改为True



