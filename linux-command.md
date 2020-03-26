# linux 常用命令

1. `~$ su`
   调出root用户时认证失败
   需要初始化root用户密码
   `~$ sudo passwd`

2. 更改文件权限

   `chmod [选项]... 八进制模式 文件...`

   eg. `sudo chmod -R 777 /opt`

   - -R: 递归目录下的所有文件

3. 解压缩

   