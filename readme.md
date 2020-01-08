## Linux

### 命令汇总

##### 常见默认端口

- 22：SSH（安全登录）SCP（文件传输）、端口号重定向
- 21：FTP（文件传输）协议代理服务器常用端
- 39000/40000：FTP被动模式常用端口
- 80/8080/3128/8081/9098：HTTP协议代理服务器常用端口号
- 1080：SOCKS代理协议服务器常用端口号
- 23：Telnet（不安全的文本传送）
- 69(udp)：TFTP（Trivial File Transfer Protocol）
- 25：SMTP Simple Mail Transfer Protocol（E-mail），默认端口号
- 110：POP3 Post Office Protocol（E-mail）
- 9080：Webshpere应用程序
- 9090：webshpere管理工具
- 3389：windows RDP远程登录
- 1521：Oracle数据库
- 3306：MySQL
- 11211：MEMCACHED
- 5432：PostgreSQ
- 1433：MS SQL
- 6379：Redis
- 8888：宝塔面板初始端口
- 888：宝塔面板phpmysql端口

##### 连接远程服务器

```shell
ssh -l username -p 22 ipaddress
ssh root@xxx.xxx.xxx.xxx

-l 登录用户名
-p 指定远程服务器上端口
-1 -2 ssh协议版本
-4 ipv4地址
-6 ipv6地址

ssh-keygen -t rsa -C '这是注释'
-t 指定密钥类型
-C 添加注释
-f 用于保存密钥的文件名
-b 指定密钥长度
```

##### 用户及用户组

```shell
// 超级用户返回普通用户
exit

// 切换超级用户
sudo passwd root
// 设置密码为 root

// 成为root用户
su

// 切换用户
su [user_name]
su – username
```

su root 和 su - root 有什么区别

- su 后面不加用户是默认切到 root
- su 是不改变当前变量
- su - 是改变为切换到用户的变量

也就是说su只能获得root的执行权限，不能获得环境变量，而su -是切换到root并获得root的环境变量及执行权限.

##### 文件及文件夹

```shell
// parents 递归创建目录
mkdir -p index/data

// -r 就是向下递归，不管有多少级目录，一并删除
// -f 就是直接强行删除，不作任何提示的意思
rm -rf dirname
```

##### 查看linux服务的运行情况

```shell
ps aux | grep nginx
// g/re/p（globally search a regular expression and print)，使用正则表示式进行全局查找并打印。

//查看当前所有tcp端口·
netstat -ntlp   

// 杀死某个进程
kill -9 PID

// 查看ssh服务是否启动
netstat -anp | grep :22
```

##### 复制命令

```shell
cp [options] source dest

// 将当前目录"test/"下的所有文件复制到新目录"newtest"下
cp –r test/ newtest        
```

- -a：此选项通常在复制目录时使用，它保留链接、文件属性，并复制目录下的所有内容。其作用等于dpR参数组合。
- -d：复制时保留链接。这里所说的链接相当于Windows系统中的快捷方式。
- -f：覆盖已经存在的目标文件而不给出提示。
- -i：与-f选项相反，在覆盖目标文件之前给出提示，要求用户确认是否覆盖，回答"y"时目标文件将被覆盖。
- -p：除复制文件的内容外，还把修改时间和访问权限也复制到新文件中。
- -r：若给出的源文件是一个目录文件，此时将复制该目录下所有的子目录和文件。
- -l：不复制文件，只是生成链接文件。 

##### source

作用：在当前bash环境下读取并执行FileName中的命令。

```shell
source filename
. filename #（中间有空格） 
```

##### 查找文件夹位置

```bash
find / -name dirname -d
```

##### 启动ssh服务

```sh
service sshd start
```

##### 关闭防火墙

```sh
service iptables stop
// 检查防火墙状态
service iptables status
```

### Centos

如果使用`yum install xxxx`，会找到安装包之后，询问你`Is this OK[y/d/N]`，需要你手动进行选择。但是如果加上参数`-y`，就会自动选择`y`，不需要你再手动选择！

- 安装git

- 安装java

  https://www.cnblogs.com/lumama520/p/11058927.html

  ```sh
  which java
  ls -lrt /usr/bin/java
  ls -lrt /etc/alternatives/java
  ```

  ls 表示列出当前目录下的文件。后面的 -lrt 是这个命令的一些选项补充。-lrt 实际上是代表了 "-l -r -t" 这三个选项集合。

  - -l 表示开启长列表输出，打开了就会输出文件权限、引用计数、所有者、所属组、文件大小、修改日期和文件名称这些详细的信息。
  - -t 以时间排序，最新的文件会排在上面。
  - -r 表示反向排序、倒序输出。
  - -x 按列输出，横向排序。
  - -u 按照文件上次被访问的时间排序。

- 安装wget

- 安装Jenkins

  https://www.cnblogs.com/loveyouyou616/p/8714544.html

#### rpm

**rpm命令**是RPM软件包的管理工具。rpm原本是Red Hat Linux发行版专门用来管理Linux各项套件的程序，由于它遵循GPL规则且功能强大方便，因而广受欢迎。逐渐受到其他发行版的采用。RPM套件管理方式的出现，让Linux易于安装，升级，间接提升了Linux的适用度。

```sh
rpm -ivh your-package     # 直接安装
rpm -ql your-package        # 查询
rpm -e your-package         # 卸载
```

https://www.cnblogs.com/ftl1012/p/rpm.html

#### wget

 wget是Linux中的一个下载文件的工具，wget是在Linux下开发的开放源代码的软件，作者是Hrvoje Niksic，后来被移植到包括Windows在内的各个平台上。

```sh
yum install -y wget
```

https://www.cnblogs.com/zhoul/p/9939601.html