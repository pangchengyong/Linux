## chmod 修改权限

-R选型 级联更改权限 举例： chmod -R 666 /tmp/123/
```
drwxrwxrwx
```
从第2位开始就是权限

这2-10位又划分为3个段（每一段有3位） (rwx) (rwx) (rwx) 分别表示 所有者、所属组、其它用户对该文件的权限是什么样的

r==read 4 w==write 2 x==execute 1

rw- == 6 r-x == 5 -wx == 3

chmod 600 1.txt 相当于是把1.txt的权限改成了 rw-------
## t 权限
t --> stick 权限 （ 作用是： 谁的文件，谁做主） rwxrwxrwt chmod 所有者 u 所属组 g 其它用户 o u+g+o == a chmod u+x chmod g-w chmod o+t
