### 1、使用`grep`命令过滤日志文件中的“ERROR”关键字
```
[admin@localhost cao]$ grep -in "error" 20250408.log 
12:error
13:Error
14:ERROR
```
### 2、用`vi`编辑文件并保存退出
```
:wq
```

### 3、通过`chmod`修改文件权限为755
```
$ chmod -R 755 20250408.log 
$ ls -al
total 4
drwxrwxr-x. 2 admin admin 26 Apr  8 08:14 .
drwxr-xr-x. 3 admin admin 17 Apr  8 08:06 ..
-rwxr-xr-x. 1 admin admin 69 Apr  8 08:14 20250408.log
```
