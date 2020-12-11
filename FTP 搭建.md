# pure-ftpd 搭建 FTP 并允许外联
pure-ftpd比vsft更轻量，更简单

## 停掉vsftpd服务
启动 pure-ftpd 前要先停止vsftpd，因为它同样监听21端口
```
systemctl stop vsftpd
```

## 安装
```
yum install -y epel-release
yum install -y pure-ftpd
```

## 配置
```
vi /etc/pure-ftpd/pure-ftpd.conf
PureDB                      /etc/pure-ftpd/pureftpd.pdb    开启密码配置文件
MinUID                      1000
```

## 创建用户
```
useradd ftpuser    创建系统用户
mkdir /home/ftp    创建ftp操作目录
chown ftpuser:ftpuser /home/ftp    修改属主和属组，改成pure-ftpd这个用户
```

## 添加 FTP 虚拟用户并设置密码
```
命令解释：
#pure-pw useradd 虚拟用户名 -u ftpuser -d 链接后可以操作的目录
创建一个虚拟用户 user1 ，通过 -u 选项将 user1 与系统用户 ftpuser 关联在一起。-d 后面的目录是系统用户的目录。通过user1 登录后，将拥有系统用户 ftpuser 的一切权限

pure-pw useradd user1 -u ftpuser -d /home/ftp     
  Password:          输入2次密码
  Enter it again: 

pure-pw mkdb    创建ftp用户数据库文件，如果不执行这步是无法登录的。
pure-pw list 
```

## 启动
```
systemctl start pure-ftpd

firewall-cmd --add-port=21/tcp --permanent
firewall-cmd --add-service=ftp --permanent
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

