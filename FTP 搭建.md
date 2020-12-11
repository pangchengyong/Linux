# pure-ftpd 搭建 FTP 并运行外联
## 安装
```
yum install -y epel-release
yum install -y pure-ftpd
```
## 配置
```
vi /etc/pure-ftpd/pure-ftpd.conf
PureDB                        /etc/pure-ftpd/pureftpd.pdb
MinUID                      1000

## 创建用户

```
useradd ftpuser   #创建用户
mkdir /home/ftp   #创建用户目录
chown ftpuser:ftpuser /home/ftp  #修改宿主和用户
```

## 添加FTP用户
```
# pure-pw useradd 链接用户名 -u ftpuser -d /home/ftp 
pure-pw useradd user1 -u ftpuser -d /home/ftp 
pure-pw mkdb
pure-pw list 
```

## 启动
```
systemctl start pure-ftpd

firewall-cmd --add-port=21/tcp --permanent
firewall-cmd --add-service=ftp --permanent
firewall-cmd --reload
```

## 测试
```
yum install -y lftp
lftp user1@127.0.0.1
```


## Win测试
下载filezilla client https://filezilla-project.org/download.php?type=client
安装

## SFTP
```
走的ssh的端口 22
支持SFTP的常用软件：filezilla xftp 
```

