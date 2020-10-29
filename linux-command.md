# linux 常用命令

1. `~$ su`
   调出root用户时认证失败
   需要初始化root用户密码
   `~$ sudo passwd`

2. ```shell
   cd ~			//用户目录
   cd /			//根目录
   cd -			//进入上一次浏览的目录
   cd .			//返回上一层目录
   
   ```


3. 解压缩

   .tar
   解包：tar xvf FileName.tar
   打包：tar cvf FileName.tar DirName
   （注：tar是打包，不是压缩！）
   ———————————————
   .gz
   解压1：gunzip FileName.gz
   解压2：gzip -d FileName.gz
   压缩：gzip FileName

   .tar.gz 和 .tgz
   解压：tar zxvf FileName.tar.gz
   压缩：tar zcvf FileName.tar.gz DirName
   ———————————————
   .bz2
   解压1：bzip2 -d FileName.bz2
   解压2：bunzip2 FileName.bz2
   压缩： bzip2 -z FileName

   .tar.bz2
   解压：tar jxvf FileName.tar.bz2
   压缩：tar jcvf FileName.tar.bz2 DirName
   ———————————————
   .bz
   解压1：bzip2 -d FileName.bz
   解压2：bunzip2 FileName.bz
   压缩：未知

   .tar.bz
   解压：tar jxvf FileName.tar.bz
   压缩：未知
   ———————————————
   .Z
   解压：uncompress FileName.Z
   压缩：compress FileName
   .tar.Z

   解压：tar Zxvf FileName.tar.Z
   压缩：tar Zcvf FileName.tar.Z DirName
   ———————————————
   .zip
   解压：unzip FileName.zip
   压缩：zip FileName.zip DirName
   ———————————————
   .rar
   解压：rar x FileName.rar
   压缩：rar a FileName.rar DirName
   ———————————————
   .lha
   解压：lha -e FileName.lha
   压缩：lha -a FileName.lha FileName
   ———————————————
   .rpm
   解包：rpm2cpio FileName.rpm | cpio -div
   ———————————————
   .deb
   解包：ar p FileName.deb data.tar.gz | tar zxf -
   ———————————————
   .tar .tgz .tar.gz .tar.Z .tar.bz .tar.bz2 .zip .cpio .rpm .deb .slp .arj .rar .ace .lha .lzh .lzx .lzs .arc .sda .sfx .lnx .zoo .cab .kar .cpt .pit .sit .sea
   解压：sEx x FileName.*
   压缩：sEx a FileName.* FileName
   
   

4. ```shell
   sudo apt-get update				//检测是否有更新
   sudp apt list --upgradable		//查看有哪些更新
   sudo apt-get upgrade			//更新
   sudo apt-get dist-upgrade		//系统更新
   
   sudo apt autoremove
   sudo apt autoclean
   
   
   ```

如果apt被占用，先解锁
   ps -e|grep apt-get
   sudo kill ***pid;



5. 安装

   ```shell
sudo apt-get install ...
sudo dpkg -i ...
   ```

6. Ubuntu下电脑合盖/息屏下仍运行

   ```shell
   vim /etc/systemd/logind.config
   ```

   ```shell
   [Login]
   #NAutoVTs=6
   #ReserveVT=6
   #KillUserProcesses=no
   #KillOnlyUsers=
   #KillExcludeUsers=root
   #InhibitDelayMaxSec=5
   #HandlePowerKey=poweroff
   #HandleSuspendKey=suspend
   #HandleHibernateKey=hibernate
   HandleLidSwitch=ignore
   #HandleLidSwitchExternalPower=suspend
   #HandleLidSwitchDocked=ignore
   #PowerKeyIgnoreInhibited=no
   #SuspendKeyIgnoreInhibited=no
   #HibernateKeyIgnoreInhibited=no
   #LidSwitchIgnoreInhibited=yes
   #HoldoffTimeoutSec=30s
   
   ```

   - HandleLidSwitch=ignore 设置成上述（如果息屏后不运行设置为：HandleLidSwitch=suspend）

   - 设置后重启，或者如下

      ```shell
     service systemd-logind restart
     ```

7. 更改文件权限

   `chmod [选项]... 八进制模式 文件...`

   eg. `sudo chmod -R 777 /opt`

    -R: 递归目录下的所有文件
   
8. 开启SSH远程连接

   ```
   sudo apt-get install openssh-server
   sudo ps -e | grep ssh
   sudo service ssh start
   ```
   
   
