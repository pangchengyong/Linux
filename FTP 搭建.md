# pure-ftpd 搭建 FTP 并运行外联
## 停掉vsftpd服务
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
useradd ftpuser
mkdir /home/ftp
chown ftpuser:ftpuser /home/ftp    修改属主和属组，改成pure-ftpd这个用户
```

## 添加FTP用户
```
pure-pw useradd     链接用户名 -u ftpuser -d 链接后可以操作的目录

pure-pw useradd user1 -u ftpuser -d /home/ftp     用 pure-ftpd 创建 ftp user1 用户，-u 映射到系统用户，-d 指定ftp用户的家目录。
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
下载filezilla client https://filezilla-project.org/download.php?type=client
安装

## SFTP
```
走的ssh的端口 22
支持SFTP的常用软件：filezilla xftp 
```

