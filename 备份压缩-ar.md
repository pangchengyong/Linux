# ar 创建备份文件 不能操作目录只能操作文件
以下列表
```
[root@VM-7-27-centos ~]# ll -rt
total 4
-rw-r--r-- 1 root root    0 Dec 14 12:17 cc.a
-rw-r--r-- 1 root root    0 Dec 14 12:17 bb.a
-rw-r--r-- 1 root root    0 Dec 14 12:17 aa.a
-rw-r--r-- 1 root root    0 Dec 14 12:19 c
-rw-r--r-- 1 root root    0 Dec 14 12:19 b
-rw-r--r-- 1 root root    0 Dec 14 12:19 a
drwxr-xr-x 2 root root 4096 Dec 14 12:23 aaa_dir
```
## 打包文件 a b c 文件为 abc.bak
```
[root@VM-7-27-centos ~]# ar rv abc.bak a b c 
ar: creating abc.bak
a - a
a - b
a - c
```
## 打包以.c结尾的文件
```
[root@VM-7-27-centos ~] ar rv two.bak *.c
```

## 显示打包的内容
```
[root@VM-7-27-centos ~] ar t two.bak
```

## 删除打包的内容
```
[root@VM-7-27-centos ~] ar d two.bak aa.a
```

## 给包中加入新内容
```
[root@VM-7-27-centos ~] ar r two.bak aa.a
```

## 将包里面的内容拿出来
```
[root@VM-7-27-centos ~] ar x two.bak
```
