## 2019.8.4

```
1.okhttp： 用于java的post和get请求
2.https://mvnrepository.com         mvn的仓库地址          fastjson   okhttp

```

## 2019.8.6

```
UML：统一建模语言。
```

## 2019.8.12    

```
	vi快捷键：
/hello                                     查找关键词
：set nu                                   显示行号
u                                          撤销
20  shift+g                                快速定位到几行 


	关机重启：
shutdown -h now                             关机
shutdown -r now                             重启
shutdown -h                                 11分钟后关机
sync                                        把内存的数据同步到磁盘


	用户和组管理：
useradd -d /home/tiger xm                   创建新用户并指定文件夹
userdel -r xm                               删除用户xm并删除用户目录
id root                                     查看用户信息
useradd -g 用户组 用户名                     创建用户时直接指定用户组
usermod -g 用户组 用户名                     修改用户的用户组
/etc/passwd                                 存放用户信息
/etc/group                                  存放组信息
/etc/shadow                                 存放密码


	linux运行级别：
0：关机 1.单用户（找回密码）2.多用户无网络 3.多用户有网络 4.保留 5.图形界面 6.重启
init [123456]                                            设置运行级别
runlevel                                                 获取上一次和当前的运行级别
systemctl set-default multi-user.target   3              设置默认的运行级别为mulit-user
systemctl isolate multi-user.target                      在不重启的情况下，切换到运行级别mulit-user下
systemctl isolate graphical.target                       在不重启的情况下，切换到图形界面下


	如何找回root密码：
**https://blog.csdn.net/RayCongLiang/article/details/81546185**
1.按键盘“↑” “↓”，否则系统将会正常启动
2.选到第一个选项，按“e“ 进入grub界面
3.找到Linux16的那一行，并将“ro”改为“rw init=/sysroot/bin/sh”
4.改写完成后，按 ”ctrl+x" 进入单用户模式
5.chroot /sysroot/ 进入原来root系统
6.接着输入命令：passwd root 修改root密码命令
7.小方块乱码，先按“ctrl+c”**退出 然后执行命令：LANG=en 定义语言为英文
8.一定一定一定要执行这个命令：touch /.autorelabel
9.reboot 或者 init 6 或者 shutdown -r now


	文件文件夹：
mkdir -p  /home/lxl                      创建多级目录
rmdir  /home/lxl                         删除空目录
rm -rf  /home/lxl                        删除非空目录
touch 1.txt 2.txt                        创建多个文件
cp -r test1/ test2/                      将文件夹递归复制到test2文件夹下
\cp -r test1/ test2/                     如果存在相同的文件夹，强制覆盖，不提示
mv 1.txt 2.txt                           将1.txt重命名为2.txt


	cat.more.less：
cat -n /etc/profile						查看文件并显示行号
cat -n /etc/profile | more				 分页查看文件并显示行号
less								   查看大型文件时使用


	 重定向和追加：
ls -l > 1.txt                把使用ls -l 显示出来的内容添加到1.txt文件中，不存在就创建该文件，存在就覆盖
ls -l >> 1.txt               同上，只是覆盖变为追加
cat 1.txt > 2.txt            把文件1.txt中的内容覆盖写到2.txt中


	echo.head.tail：
echo $PATH                               显示当前的环境变量
head -n 5 /etc/profile                   显示文件的前5行，默认10行
tail -n 5 /etc/profile                   显示文件的后5行，默认显示10行
tail -f   /home/mydata.txt               实时更新显示文档，一般用于日志查看


	ln.history：
ln -s /home/lxl linktolxl                把/home/lxl 文件夹软连接到 linktolxl  相当于快捷方式
rm -rf linktolxl                         删除软连接，文件夹后面不能到 “/”
history                                  显示执行过的指令
history 10                               显示后10条执行过的指令
！16                                     执行history后直接执行第16条对应的指令


	date.cal：
date                                     显示当前的时间
date "+%Y %m %d"                         显示年月日
date -s "2018-10-10 11.22.22"            给电脑设置时间
cal                                      显示日历
cal     2020                             显示一年的日历
```

## 2019.8.13

```
	
	find.locate.grep
find /home -name hello.txt         按名字查找home目录下的hello.txt文件
find /opt -user root               按用户查找opt目录下属于root用户的文件
find / -size +20M                  按大小查找文件
locate hellow.txt                  快速定位hellow.txt,使用前必须先使用updatedb更新。
cat hello.txt | grep -ni yes       不区分大小写查找hello.txt文件内yes的内容并显示行号


	压缩解压缩指令：
gzip gunzip                            不保留原文件直接解压缩
zip  unzip                             保留原文件解压缩
zip -r 	mypackge.zip /home/            递归压缩整个目录						
unzip -d /opt/tmp mypackage.zip        解压并指定解压目录
tar -zcvf a.tar.gz a1.txt a2.txt       打包并压缩a1.txt和a2.txt文件为a.tar.gz
tar -zxvf a.tar.gz                     解压
tar -zxvf a.tar.gz -C /opt/temp        指定解压目录 -C要有
tar -xvf MySQL-5.6.45-1.el7.x86_64.rpm-bundle.tar      解压mysql

	

```

## 2019.8.14

- ```
  组管理：
  chown tom 1.txt                      把文件1.txt的所有者改为tom
  chgrep police 2.txt                  把文件2.txt的所有组改为police
  usermod -g 用户组 用户名              修改用户的用户组
  
  
  	权限管理：
  rws对文件                             w不代表可以删除该文件，前提是该文件所在的目录有写权限
  chmod a+r                             给所有用户加读权限
  chown -R  tom /home                   把目录下的所有问价包括目录所有者改为tom 
  chown -R mysql:mysql /usr/local/mysql
  	
  	
  	定时任务调度crond：     -e 编辑           -l显示            -r删除
  crondtab -e                                   添加定时任务
  */1 * * * * ls -l /etc >> /temp/to.txt        没分钟自动调用ls -l /etc   把内容追加到/temp/to.txt
  
  
  
  	使用shell脚本编写date命令并把执行结果放到home/mydate:
  1.创建文件task1.sh
  2.编辑task1.sh添加     date >> /home/mydate
  2.给文件添加执行权限
  3.添加定时任务 crontab -e 
  4.编辑添加    */1 * * * * /home/task1
  	
  	
  	每天凌晨2:00将mysql数据库testdb，备份到文件mydb.bak中。
  1.先编写一个文件 /home/task3.sh
  	/usr/local/mysql/bin/mysqldump -u root -p root testdb > /tmp/mydb.bak
  2.给task3.sh一个可执行权限
  	chmod 744 /home/task3.sh
  3.crontab -e
  4. 0 2 * * * /home/task3.sh
  
  
  	
  	linux分区：
  lsblk -f                     老师不离开，显示硬盘和分区信息
  lsblk                        显示硬盘大小
  	
  	
  	如何增加一块硬盘：
  1.虚拟机添加硬盘
  2.分区 fidsk /dev/sdb
  3.格式化 mkfs -t ext4 /dev/sdb1
  4.挂载 先创建一个挂载目录 /home/newdisk ,挂载 mount /dev/sdb1 /home/newdisk
  5.设置可以自动挂载（vim /etc/fstab）
  
  
  
  
  ```


![1565834648213](C:\Users\LXL\AppData\Roaming\Typora\typora-user-images\1565834648213.png)



## 2019.8.27

```
	磁盘查询实用指令：
df -h                                 查询系统整体的磁盘使用情况
du -ach --max-depth=1 /opt            opt目录的磁盘占用情况，深度为1
ls -l /home | grep "^-" | wc -l       统计home目录下的文件个数
ls -lR /home | grep "^-" | wc -l      递归统计文件个数
find / -name '*.iso'                  查找所有的镜像文件

	添加
	
	网络配置：
vi /etc/sysconfig/network-scripts/
	
	进程管理：
ps -aux | more                               查看进程
ps -aux | grep sshd                          查看某个进程
ps -ef | more                                查看父进程
ps -ef | grep sshd                           查看某个进程的父进程
ps -ef | grep mysql                          查看mysql是否运行
kill  4010                                   杀死某个进程
killall gedit                                把归属于gedit的进程全部杀死
kill -9 4090                                 强制终止
pstree -p                                    以树状的形式展示进程id
lsof -i:50                                   查看50端口占用

	

```

## 	docker：

​	开源的容器引擎，能将基础设施和应用程序隔离，能将基础设施当做程序一样来管理，可以快速的打包、测试以及部署应用程序，并可以缩短从编写到部署运行代码的周期。

uname -r       查看内核，docker内核要求3.10以上

yum  -y update  确保yum包更新到最新

## 2018.8.28

```
	服务管理：
firewall-cmd --zone=public --add-port=5672/tcp --permanent       # 开放5672端口
firewall-cmd --zone=public --remove-port=5672/tcp --permanent    # 关闭5672端口
firewall-cmd --reload                                            # 配置立即生效
firewall-cmd --zone=public --list-ports                          查看防火墙所有开放的端口
systemctl status firewalld                             查看防火墙细节
firewall-cmd --state                                   查看防火墙
systemctl stop firewalld                               关闭防火墙
systemctl start firewalld                              启动防火墙
telnet 192.168.253.10 22                               windows下查看端口是否在用
systemctl list-unit-files                              列出所有的服务
scp -r /home/TAS2.8.2/ root@11.1.1.2:/home/            远程复制本地文件到服务器
nmcli con show                                         查看网卡uuid
route add 10.0.0.0 mask 255.0.0.0 192.168.1.1          内外网同时访问：添加路由就可以实现。
ip neighbour                                           查看局域网设备MAC
yum install net-tools                                  ifconfig无效

	开机流程说明：
开机，bios，/boot/init进程1/运行级别/运行级对应的服务

top -d 10                                              动态监控进程10秒刷新一次
netstat -anp | more                                    查看所有的网络服务
netstat -anp | grep sshd                               查看特定服务
netstat -lnpt                                          查看监听的端口
netstat -lnpt |grep 5672                               检查端口被哪个进程占用


	rpm，yum
rpm -qa | grep *                                      	查看是否安装*rpm包
rpm -qi firefox                                      	查看软件安装包的具体信息
rpm -ql firefox                                     	查看软件的安装位置
rpm -qf /etc/passwd                                     查看文件属于哪个rpm包
rpm -e firefox                                          删除火狐
rpm -e --nodeps firefox                                 强制删除
rpm -ivh                                                i:安装，v:提示，h：进度条

```

## 2019.8.29——javaEE

```
	 JDK配置环境变量：
 vi  /etc/profile       
export JAVA_HOME=jdk路径
export PATH=$PATH:$JAVA_HOME/bin
source  /etc/profile
javac hellow.java 编译  java hello 运行
	
```

## 2019.9.3——mysql

```
默认安装路径： /usr/local/mysql

```

## shell编程

```
命令行解释器
第一个shell案例：
	1.创建一个.sh文件。
	2.编辑   #！/bin/bash 
			echo "hello world"
	3.给文件添加可执行权限755
	4.执行文件 ./
	

shell变量：
	系统变量：$PATH 、$USER 
	自定义变量：a=100   
			  echo "a=$a"
               unset a          清除变量
               echo "a=$a"
    静态变量：readonly a    不能被unset
    
	date=`date`         将date命令返回的结果赋值给date
	echo "date=$date"
	
	：<<!     !              多行注释
	
	
位置参数变量:                   
		#!/bin/bash
		echo "$0 $1 $2"
		echo "$*"
		echo "$@"
		echo "参数个数=$#"
	执行./positionPass 20 10
		./positionPare.sh 20 10
			20 10
			20 10
			参数个数=2	
	
	
预定义变量：
		#!/bin/bash
	echo "当前的进程号:$$"   
	#后台的方式运行 shell 
	./shell.sh &                                   
	echo "最后的进程的进程号=$!"
	echo "执行的值=$?"
	
	
运算符：
          #!/bin/bash
          #第一种方式
          result1=$(((2+3)*4))
          echo "result1=$result1"
          #第二种方式
          result2=$[(2+3)*4]
          echo "result2=$result2"
          #第三种方式
          TEmp=`expr 2 + 3`
          result3=`expr $TEmp \* 4`
          echo "result3=$result3"
          #求出两个参数的和
          SUM=$[$1+$2]
          echo "sum=$SUM"
          
判断语句：
		#!/bin/bash
         #案例一：“ok”是否等于“ok”
         if [ "ok"="ok" ]
         then
                 echo "true"
         fi
         #案例二：23是否大于等于22
         
         if [ 23 -gt 22 ]
         then
                 echo "大于"
         fi
         #案例三：/home/LXL/a.txt 文件是否存在
         if [ -e /home/LXL/a.txt ]
         then
                 echo "存在"
          fi
    
    
   流程控制输入数字大于等于60显示及格，小于显示不及格：
   		#!/bin/bash
        if [ $1 -ge 60 ]
        then
                echo "及格了"
        elif [ $1 -lt 60 ]
        then
                echo "不及格"
        fi


流程控制语句case输入参数1，显示周一，输入参数2显示周二，输入其他显示other：
         #!/bin/bash
         case $1 in
         "1")
         echo "周一"
         ;;
         "2")
         echo "周二"
         ;;
         *)
         echo "other"
         ;;
         esac
         
     
打印命令行输入的参数：
	#!/bin/bash
    #使用 $*
    for i in "$*"
    do
            echo "the num is $i"
    done
    echo "--------------------"
    #使用$@
    for j in "$@"
    do
            echo "the num is $j"
    done
    
    
控制台输入n，计算输出1+2+...n的和  
     #!/bin/bash
     SUM=0
     i=0
     while [ $i -le $1 ]
     do
             SUM=$[$SUM + $i]
             i=$[$i + 1]
     done
     echo "sum=$SUM"
     


读取控制台输入
      #!/bin/bash
      read -p "请输入一个值num1=" NUM1    read -t 10 -p     等待时间10秒
      echo "你输入的值是num1=$NUM1"


系统函数
	basename /home/aaa/test.txt        返回test.txt
	basename /home/aaa/test.txt .txt   返回test   
	可以写到shell脚本中
	dirname /home/aaa/test.txt         返回/home/aaa
	

自定义函数：控制台输入两个数求和
	#!/bin/bash
     function getSum(){
     
             SUM=$[$n1+$n2]
             echo "两数的和是=$SUM"
     
     }
     read -p "请输入第一个数n1:" n1
     read -p "强输入第二个数n2:" n2
     #调用函数
     getSum $n1 $n2
     
     
 
 shell综合案例：
 	需求
 		1.每天凌晨2:10备份数据库 atguigu 到 /date/backup/db
 		2.备份开始和备份结束能够给出相应的提示信息
 		3.备份后的文件要求以时间为文件名，并打包成.tar.gz的形式，比如2019-09-06_1996.tar.gz             
 		4.在备份的同时，检查是否有10天前备份的数据库文件，如果有就删除
        
#!/bin/bash
#完成数据库的定时备份
#备份的路径
BACKUP=/date/backup/db
#当前的时间作为文件名
DATETIME=$(date +%Y_%m_%d_%H:%M:%S)
#可以输出变量调试
echo "$DATETIME"
echo "------------开始备份---------------"
echo "备份的路径是 $BACKUP/$DATETIME.tar.gz"
#主机
HOST=localhost
#用户名
DB_USER=root
#密码
DB_PWD=root
#备份数据库名
DATABASE=atguiguDB
#创建备份路径
#如果备份的路径文件夹存在，就使用，否则就创建
[ ! -d "$BACKIP/DATETIME" ] && mkdir -p "$BACKUP/$DATETIME"
#执行mysql的备份数据库指令
mysqldump -u$DB_USER -p$DB_PWD --host=$HOST $DATABASE | gzip > $BACKUP/$DATETIME/$DATETIME.sql.gz
#打包备份文件
cd $BACKUP
tar -zcvf $DATETIME.tar.gz $DATETIME
#删除临时目录
rm -rf $BACKUP/$DATETIME
#删除10天前的备份文件
find $BACKUP -mtime +10 -name ".tar.gz" -exec rm -rf {} \;
echo "------备份文件成功-------"

接着：crontab -e 编辑内容为 10 2 * * * /usr/sbin/mysql_backup_db.sh
	
```

