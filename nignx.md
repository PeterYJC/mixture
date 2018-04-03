#LNMP

工作室目前大部分web环境均为Nginx，而其使用epoll事件模型、有丰富的模块库、配置灵活等优点不一一介绍了。本篇文章主要讲安装使用等基本事项。    

### 安装
>### CentOS    

# <code>sudo yum -y install epel-release</code> #依赖库    

#### <code>sudo yum -y install nginx</code>  #安装Nginx    

##### <code>sudo systemctl enable nginx</code> #开机启动    

<code>sudo systemctl start nginx</code> #启动     

> Debian

### <code>sudo apt-get install nginx</code> #安装      

<code>sudo update-rc.d nginx defaults</code> #开机启动
<code>sudo service nginx start</code> #启动

```
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
cd /etc/yum.repos.d
vim CentOS-Base.repo
sudo yum makecache #将服务器上的软件信息缓存到主机上，提高查找速度
yum install epel-release  #epel源
yum install nginx
systemctl enable nginx && systemctl start nginx
lsof -i:80 #查看80端口 netstat -lanp|grep 80
firewall-cmd --zone=public --add-port=80/tcp --permanent
yum install php-fpm -y 
yum install  php-mysql -y
netstat -lanp|grep 80
curl localhost:80
yum install  mariadb mariadb-server -y
systemctl enable mariadb && systemctl start mariadb
mysql_secure_installation
systemctl enable mariadb && systemctl start mariadb
yum search php
yum install php php-mysql php-fpm
systemctl enable php-fpm && systemctl start php-fpm
```

### 配置数据库

```
mysql_secure_installation  # 开始配置

NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL MariaDB
      SERVERS IN PRODUCTION USE!  PLEASE READ EACH STEP CAREFULLY!

In order to log into MariaDB to secure it, we'll need the current
password for the root user.  If you've just installed MariaDB, and
you haven't set the root password yet, the password will be blank,
so you should just press enter here.

Enter current password for root (enter for none):
OK, successfully used password, moving on...

Setting the root password ensures that nobody can log into the MariaDB
root user without the proper authorisation.

Set root password? [Y/n] Y
New password:
Re-enter new password:
Password updated successfully!
Reloading privilege tables..
 ... Success!


By default, a MariaDB installation has an anonymous user, allowing anyone
to log into MariaDB without having to have a user account created for
them.  This is intended only for testing, and to make the installation
go a bit smoother.  You should remove them before moving into a
production environment.

Remove anonymous users? [Y/n]
 ... Success!

Normally, root should only be allowed to connect from 'localhost'.  This
ensures that someone cannot guess at the root password from the network.

Disallow root login remotely? [Y/n]
 ... Success!

By default, MariaDB comes with a database named 'test' that anyone can
access.  This is also intended only for testing, and should be removed
before moving into a production environment.

Remove test database and access to it? [Y/n]
 - Dropping test database...
 ... Success!
 - Removing privileges on test database...
 ... Success!

Reloading the privilege tables will ensure that all changes made so far
will take effect immediately.

Reload privilege tables now? [Y/n]
 ... Success!

Cleaning up...

All done!  If you've completed all of the above steps, your MariaDB
installation should now be secure.

Thanks for using MariaDB!
```



>关于systemctl及service差异

![屏幕快照 2017-11-18 下午8.33.03](/Users/yangjiacheng/Desktop/屏幕快照 2017-11-18 下午8.33.03.png)



**service**命令是Redhat Linux兼容的发行版中用来控制系统服务的实用工具，它以启动、停止、重新启动和关闭系统服务，还可以显示所有系统服务的当前状态。

**chkconfig**命令检查、设置系统的各种服务。这是Red Hat公司遵循GPL规则所开发的程序，它可查询操作系统在每一个执行等级中会执行哪些系统服务，其中包括各类常驻服务。谨记chkconfig不是立即自动禁止或激活一个服务，它只是简单的改变了符号连接。



#### Tips
# Nginx默认端口80，目录/etc/nginx     

# 在curl或浏览器等访问有问题时       

0. ps检查进程是否启动     
1. 用lsof或netstat查看端口号是否被监听    
2. iptables(firewalld)服务是否有限制策略
### 字段及配置讲解
Nginx配置很多，以下主要挑取常见的讲解，遇到问题及时Google。    
>全局字段

1. user Nginx进程用户名，推荐用nginx(注意开启nologin)
2. work_process 工作进程数量，建议设置为等于CPU核心数
3. error_log/access_log  全局错误/访问日志          
4. events->worker_connections 连接数上限

>http字段

1. server_tokens nginx版本号，建议为off 

2. Gzip压缩(建议开启，节约流量)  

  ```
  gzip on; #开启gzip压缩输出     

  gzip_min_length 1k; #最小压缩文件大小     

  gzip_buffers 4 16k; #压缩缓冲区     

  gzip_comp_level 2; #压缩等级     

  gzip_types application/javascript application/x-javascript     

  text/css image/jpeg image/gif image/png; #需要压缩的类型

  ```

   

>server字段 
>多虚拟主机域名即多个server字读

1. listen 80; 监听端口
2. ## server_name www.starstudio.org; 域名
3. ## index index.html index.htm index.php; 主页加载优先顺序
4. root /var/share/html; web根目录

>location
>location需要熟悉正则匹配语法，以更好处理请求

1. php-fpm     
  安装 <code>sudo yum install php-fpm php-mysql</code>     
  配置 <code>     

  ```
  location ~ .php$ { #正则匹配.php结尾的文件请求      

  root/var/share/html/php;    

  fastcgi_pass  127.0.0.1:9000; #转发到9000端口     

  #fastcgi_pass   unix:/var/run/php-fpm.sock; #转发到php-fpm.sock    

  fastcgi_index  index.php;     

  fastcgi_param  SCRIPT_FILENAME;         

  $document_root  $fastcgi_script_name;     

  include   fastcgi_params;
  }
  ```

  #### Nginx与php的内在联系

  ># Nginx只能处理静态的页面请求，php处理动态页面请求。当用户连接Nginx80端口时，首先由Nginx判断请求是静态还是动态，若是静态页面，Nginx直接将请求结果返回给客户机；若是动态页面，则nginx将请求转交给本机的9000端口。php监听本机9000端口，正好由php解释器去处理动态页面。最终将请求结果返回给Nginx，再由Nginx将结果返回给客户端。

  #### php与mysql

  ​

  ## LNMP平台简介

  LNMP指的是多款软件的集合。L指的是Linux系统，N指的是Nginx网站服务器，M指的是Mariadb数据库软件，P指的是php软件。Linux目前是一款最流行的免费开源的操作系统。Nginx可以做为高性能的HTTP和反向代理服务器，也可以作为IMAP/POP3/SMTP代理服务器。

  关于Nginx学习与入门，可参考下面这篇文章，详细介绍了Nginx相关知识【[https://www.qcloud.com/community/article/593436](https://cloud.tencent.com/community/article/593436) 】。Mysql是一个小型关系型数据库管理系统。PHP是一种在服务器端执行的嵌入HTML文档的脚本语言。

  Nginx软件包采用的是模块化的设计，模块分为内置模块和第三方模块。

  Nginx默认监听本机80端口。

  ### 安装Mariadb数据库

  Mariadb:多线程，多用户的SQL数据库管理系统。软件包:`mariadb`，`mariadb-server`；服务：`mariadb`

  安装软件包：

  >```
  >yum -y install mariadb mariadb-server mariadb-devel
  >Mariadb默认监听本机3306端口
  >```
  >
  >### 安装php软件
  >
  >PHP: 一种编程语言，最初用于设计生产动态网站。与PERL，PYTHON类似。软件包:php php-mysql php-fpm。php-fpm软件用来连接nginx。php-mysql用来连接数据库。
  >
  >```
  >[root@cc]# yum –y install php php-mysql php-fpm
  >
  >```
  >
  >php默认监听本机9000端口。
  >
  >​
  >
  >- nginx
  >
  >```
  >[root@cc]# nginx                              //前面已经做好软连接
  >
  >```
  >
  >- mariadb
  >
  >```
  >[root@cc]# systemctl restart mariadb        
  >[root@cc]# systemctl enable  mariadb          //开机自启动
  >
  >```
  >
  >- php
  >
  >```
  >[root@cc]# systemctl restart php-fpm
  >[root@cc]# systemctl enable  php-fpm
  >
  >```
  >
  >## 建立LNMP平台
  >
  >Nginx与php的内在联系
  >
  >Nginx只能处理静态的页面请求，php处理动态页面请求。当用户连接Nginx80端口时，首先由Nginx判断请求是静态还是动态，若是静态页面，Nginx直接将请求结果返回给客户机；若是动态页面，则nginx将请求转交给本机的9000端口。php监听本机9000端口，正好由php解释器去处理动态页面。最终将请求结果返回给Nginx，再由Nginx将结果返回给客户端。

  ​

  ####php连接Mariadb

  >php连接Mariadb数据库，进行数据的读取。

  ####log

之所以单独抽出log来，是希望大家能养成用log定位问题的习惯，还有就是log需要跟进web访问量进行以天/周等为粒度的切分



####nginx.conf

```
vim /usr/local/nginx/conf/nginx.conf

location / {
            root   html;
            index  index.php  index.html   index.htm;
           }
 location  ~  \.php$  {                //修改Nginx连接php的配置部分
            root           html;
            fastcgi_pass   127.0.0.1:9000;
            fastcgi_index  index.php;
            include        fastcgi.conf;
        }

```

#### phpinfo

```
vim /usr/local/nginx/html/test.php   //php初始界面

<?php
phpinfo();
?>
```



#### php-mysql

```
vim /usr/local/nginx/html/test3.php

<?php
$mysqli = new mysqli('localhost','root','','mysql');
if (mysqli_connect_errno()){
    die('Unable to connect!'). mysqli_connect_error();
}
$sql = "select * from user";
$result = $mysqli->query($sql);
while($row = $result->fetch_array()){
    printf("Host:%s",$row[0]);
    printf("</br>");
    printf("Name:%s",$row[1]);
    printf("</br>");
}
?>
```

#### https

```
SSL协议提供的服务主要有：

　　1）认证用户和服务器，确保数据发送到正确的客户机和服务器；

　　2）加密数据以防止数据中途被窃取；

　　3）维护数据的完整性，确保数据在传输过程中不被改变。
```

```
服务器认证阶段：1）客户端向服务器发送一个开始信息“Hello”以便开始一个新的会话连接；2）服务器根据客户的信息确定是否需要生成新的主密钥，如需要则服务器在响应客户的“Hello”信息时将包含生成主密钥所需的信息；3）客户根据收到的服务器响应信息，产生一个主密钥，并用服务器的公开密钥加密后传给服务器；4）服务器恢复该主密钥，并返回给客户一个用主密钥认证的信息，以此让客户认证服务器。

　　用户认证阶段：在此之前，服务器已经通过了客户认证，这一阶段主要完成对客户的认证。经认证的服务器发送一个提问给客户，客户则返回（数字）签名后的提问和其公开密钥，从而向服务器提供认证。
```



```
ssl_certificate "/etc/pki/nginx/server.crt";
ssl_certificate_key "/etc/pki/nginx/private/server.key";
```

```
server {
       listen       443 ssl;                               //监听https的443端口
       server_name  www.bb.com;

       ssl_certificate      cert.pem;                      //证书
       ssl_certificate_key  cert.key;                      //私钥
       location / {
           root   html;
           index  index.html index.htm;
       }
    }
```

