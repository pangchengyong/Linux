chown 用来更改一个文件或者目录的所有者或者所属组

-R 级联更改一个目录下所有的目录和文件

例如：

chown  user1:users 1.txt 
chown user1.users 1.txt
chown user1. 1.txt 所属主和属组都是 user1
chown user1 1.txt 只改所属主为user1
useradd 添加用户的命令，如 useradd user1 添加user1用户，同时也会添加一个user1组

查看刚添加的用户 tail -2 /etc/passwd

tail命令是用来查看一个文件最后几行的命令
用法： tail 1.txt  ;  tail -5 1.txt ; tail -n 5 1.txt 
查看一个用户属于哪一个组：

id username 查看，其中一个用户会有两个组，一个是主组，一个是附属组
增加组的命令 groupadd ，如 groupadd users1 tail -2 /etc/group 查看刚刚添加的组

history 查看命令历史

!ls 执行命令历史中，从下网上看，第一个ls开头的命令
