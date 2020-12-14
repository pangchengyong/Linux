# bzip2 bunzip2

## 压缩文件为bz2
```
[root@VM-7-27-centos ~]# bzip2 -skfv abc.bak
```

## 解压缩 .bz2
```
bunzip2 -skfv abc.bz2
```

s   降低程序执行时内存的使用量
k   保留原始文件
f   强制覆盖
v   显示详细输出

## 修复压缩包
bzip2是以区块的方式来压缩文件，每个区块视为独立的单位。因此，当某一区块损坏时，便可利用bzip2recover，试着将文件中的区块隔开来，以便解压缩正常的区块。
```
bzip2recover [.bz2 压缩文件]
```
