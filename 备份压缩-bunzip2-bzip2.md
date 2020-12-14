# bzip2 bunzip2

## 压缩文件为bz2
```
[root@VM-7-27-centos ~]# bzip2 -skfv abc.bak
```

## 解压缩 .bz2
```
bunzip2 -skfv temp.bz2
```

s   降低程序执行时内存的使用量
k   保留原始文件
f   强制覆盖
v   显示详细输出
