# pure-ftpd 搭建 FTP 并允许外联
pure-ftpd比vsft更轻量，更简单

## 停掉vsftpd服务
启动 pure-ftpd 前要先停止vsftpd，因为它同样监听21端口
```
systemctl stop vsftpd
```

## 安装
```
yum install -y epel-release    先安装epel-release扩展源
yum install -y pure-ftpd       使用yum命令安装pure-ftpd
```

## 配置
```
vim /etc/pure-ftpd/pure-ftpd.conf
PassivePortRange             30000 31000                      被动模式端口范围
ForcePassiveIP               118.xxx.xxx.117                  外网IP
PureDB                       /etc/pure-ftpd/pureftpd.pdb      开启密码配置文件，否则无法登录 【这里一定要绝对路径】
BrokenClientsCompatibility   yes                              兼容不同客户端
NoAnonymous                  yes                              不允许匿名用户连接
```

## 创建用户
```
useradd ftpuser    创建系统用户

mkdir /home/ftp    创建ftp操作目录
chown -R ftpuser:ftpuser /home/ftp      修改属主和属组，改成pure-ftpd这个用户

```

## 添加 FTP 虚拟用户并设置密码
```
命令解释：
#pure-pw useradd 虚拟用户名 -u ftpuser -d 链接后可以操作的目录
创建一个虚拟用户 user1 ，通过 -u 选项将 user1 与系统用户 ftpuser 关联在一起。-d 后面的目录是系统用户的目录。通过user1 登录后，将拥有系统用户 ftpuser 的一切权限

pure-pw useradd user1 -u ftpuser -d /home/ftp     
  Password:          输入2次密码
  Enter it again: 

pure-pw mkdb    创建用户信息数据库文件

pure-pw list    查看用户列表
pure-pw show user1   查看用户详情
pure-pw passwd user1   修改用户密码
```

## 启动
```
systemctl start pure-ftpd

防火墙没有开这里就别做
firewall-cmd --add-port=21/tcp --permanent
firewall-cmd --reload
```

## 本机测试
```
yum install -y lftp
lftp user1@127.0.0.1


[root@VM-7-27-centos ftp]# lftp user1@127.0.0.1
密码: 
lftp user1@127.0.0.1:~> ls                      
drwxr-xr-x    2 1000       ftpuser          4096 Dec 11 15:23 .
drwxr-xr-x    2 1000       ftpuser          4096 Dec 11 15:23 ..
-rw-r--r--    1 1000       ftpuser             0 Dec 11 15:23 a
lftp user1@127.0.0.1:/>
```


## Win测试
```
下载filezilla client https://filezilla-project.org/download.php?type=client
安装
```

## SFTP
```
走的ssh的端口 22
支持SFTP的常用软件：filezilla xftp 
```

## 日志记录
```
修改配置文件
# 记录日志  vim /etc/pure-ftpd/pure-ftpd.conf
VerboseLog                   yes

建立文件 /var/log/pureftpd.log

修改/etc/rsyslog.conf
在这行的cron.none后面添加 ;ftp.none 使ftp的日志信息成私有
*.info;mail.none;authpriv.none;cron.none /var/log/messages为
*.info;mail.none;authpriv.none;cron.none;ftp.none /var/log/messages

在/etc/rsyslog.conf文件最后加上
#pureftp日志

ftp.*               -/var/log/pureftpd.log
不要去掉/var前面的-号,否则日志会在/var/log/messages与/var/log/purefpd.log里各记录一份. 添加了-号,就只会记录在/var/log/purefptd.log内
```

使/etc/syslog.conf生效

```
重启系统日志     service rsyslog restart
重启puerftpd     service pure-ftpd restart
```


## 遇到的问题
引用：https://blog.csdn.net/wssnxcj/article/details/82762009

tail -f /var/log/messages

### 登陆报错：530 Login authentication failed
```
vim /etc/pure-ftpd/pure-ftpd.conf
默认是1000 调整到 500
MinUID                      500

pure-ftpd配置中只允许uid大于等于500的,才可以登录ftp（系统安全考虑）
我们可以修改配置，把uid阈值调小，也可以在pure-ftp网页管理中设置一个uid大于500的用户。
```


### 链接报错 421 Unable to read the indexed puredb file (or old format detected) - Try pure-pw mkdb
这个是配置不对，好好看上面引用。

### lftp 本机可以连接 但是xshell 无法链接
报错：SSH服务器拒绝了密码，请再试一次。

xshell 链接用21 端口链接。
powershell 链接 直接输入 ftp IP
```
PS C:\Users\nick> ftp 118.xxx.xxx.117
连接到 118.xxx.xxx.117。
220---------- Welcome to Pure-FTPd [privsep] [TLS] ----------
220-You are user number 1 of 50 allowed.
220-Local time is now 19:19. Server port: 21.
220-This is a private system - No anonymous login
220-IPv6 connections are also welcome on this server.
220 You will be disconnected after 15 minutes of inactivity.
504 Unknown command
用户(118.xxx.xxx.117:(none)): user1
331 User user1 OK. Password required
密码:
230 OK. Current directory is /
ftp>
```



|参数|说明
|--|--|        
|ChrootEveryone yes|启用chroot。|
BrokenClientsCompatibility yes|	兼容不同客户端。
Daemonize yes	|后台运行。
MaxClientsPerIP 20	|每个ip最大连接数。
VerboseLog yes	|记录日志。
DisplayDotFiles no|	显示隐藏文件。
AnonymousOnly no|	只允许匿名用户访问。
NoAnonymous yes|	不允许匿名用户连接。
SyslogFacility none|	不将日志在syslog日志中显示。
DontResolve yes	|不进行客户端DNS解析。
MaxIdleTime 15	|最大空闲时间。
LimitRecursion 2000 8	|浏览限制，文件2000，目录8层。
AnonymousCanCreateDirs no	|匿名用户可以创建目录。
MaxLoad 4|	超出负载后禁止下载。
PassivePortRange 45000 50000	|被动模式端口范围。
#AnonymousRatio 1 10	|匿名用户上传/下载比率。
UserRatio 1 10	|所有用户上传/下载比率。
AntiWarez yes|	禁止下载匿名用户上传但未经验证的文件。
AnonymousBandwidth 200	|匿名用户带宽限制（KB）。
UserBandwidth 8	|所有用户最大带宽（KB）。
Umask 133:022	|创建文件/目录默认掩码。
MinUID 100	|最大UID限制。
AllowUserFXP no	|仅运行用户进行FXP传输。
AllowAnonymousFXP no	|对匿名用户和非匿名用户允许进行匿名 FXP 传输。
ProhibitDotFilesWrite no	|不能删除/写入隐藏文件。
ProhibitDotFilesRead no	|禁止读取隐藏文件。
AutoRename yes	|有同名文件时自动重新命名。
AnonymousCantUpload yes	|不允许匿名用户上传文件。
AltLog clf:/var/log/pureftpd.log	|clf格式日志文件位置。
PureDB /etc/pure-ftpd/pureftpd.pdb	|用户数据库文件。
MaxDiskUsage 99|	当磁盘使用量打到99%时禁止上传。
CreateHomeDir yes	|如果虚拟用户的目录不存在则自动创建。
CustomerProof yes	|防止命令误操作。

