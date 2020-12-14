## OpenSSH搭建sftp
sftp加密传输，设置也简单。ftp是不安全的协议

# 查看版本 版本不同会遇到一些奇怪问题
```
[root@VM-7-27-centos ~]# ssh -V
OpenSSH_8.0p1, OpenSSL 1.1.1c FIPS  28 May 2019

```
老版本查看：https://www.jianshu.com/p/cc4750f9a8dd
# 编辑配置
```
# 端口号
Port 22
# 打开 sftp
Subsystem       sftp    internal-sftp
# 针对 sftp 用户组的设置
Match Group sftp
        ChrootDirectory %h
        ForceCommand internal-sftp
```

# 重启服务
```
systemctl restart sshd.service
```

# 用户管理 

## 创建用户组
```
groupadd sftp
```

## 创建用户
```
useradd -G sftp -d /data/nick -s /sbin/nologin nick -M

-d              # 指定用户操作目录
-G              # 加入用户组
-s              # 指定用户登入后所使用的shell
/sbin/nologin   # 用户不允许登录
-M              # 不要自动建立用户的登入目录

userdel -rf nick

```
## 设置密码
```
passwd nick  # uWlm5rTjCNXPCFsg
```

## 修改文件夹所属主
```
mkdir /data/nick && cd /data/nick && mkdir upload download && chown nick download upload
```


# 链接
```
sftp -P 22 nick@111.111.111.111
```

参考：https://blog.csdn.net/ManWZD/article/details/108442094
