## LVS（linux virtual server）负载均衡配置

```
1.准备三台服务器，一台做负载均衡，2台做服务器
2.配置负载服务器：
	（1）ifconfig eth0:8 192.168.9.100/24            配置vip，因为是192.168.9.0网段
	（2）echo 1 > /proc/sys/net/ipv4/ip_forward      开启转发功能，只能使用重定向来改
3.配置2台真实服务器
	（1）echo 1 > /proc/sys/net/ipv4/conf/eth0/arp_ignore    
	 (2) echo 1 > /proc/sys/net/ipv4/conf/all/arp_ignore      配置都是临时的
	 (3)echo 2 > /proc/sys/net/ipv4/conf/eth0/arp_announce
	 (4)echo 2 > /proc/sys/net/ipv4/conf/all/arp_announce
	 (5)ifconfig lo:7 192.168.9.100 netmask 255.255.255.255   配置vip对应到负载服务器
	 (6)yum install httpd -y   		安装httpd
	 (7)cd /var/www/html
	 (8)vi index.html      在里面填写内容from ip 就可以
	 (9)service httpd start   启动httpd 
4.在负载服务器配置
	（1）yum install ipvsadm -y                           安装ipvsadm管理工具
	 (2)ipvsadm -A -t 192.168.9.100:80 -s rr              轮询tcp监听80端口
	 (3)ipvsadm -ln                                       查看规则
	（4）ipvsadm -a -t 192.168.9.100 -r 192.168.9.12 -g   配置负载哪台服务器
	 (5)ipvsadm -ln                                       查看规则
	（6）ipvsadm -a -t 192.168.9.100 -r 192.168.9.13 -g   配置负载另一台服务器
5.此时访问192.168.9.100会负载到两台真实服务器
	（1）ipvsadm -lnc    查看偷窥记录本
	  
	netstat -natp        查看端口占用
	curl 192.168.1.100   可以模拟浏览器请求
	
```

## 高可用配置，keepalived

```
keepalived原理：
	vrrpx协议（虚拟路由冗余协议） virtual router redundancy protocol

配置高可用，两台lvs（负载均衡）两台rsv（真实服务器）
 1.如果有vip要先清除vip
 	ifconfig eth0:3 down
 2.如果安装过ipvsadm要清除内核模块
 	ipvsadm -C
 3.配置2台真实服务器
	（1）echo 1 > /proc/sys/net/ipv4/conf/eth0/arp_ignore    
	 (2) echo 1 > /proc/sys/net/ipv4/conf/all/arp_ignore      配置都是临时的
	 (3)echo 2 > /proc/sys/net/ipv4/conf/eth0/arp_announce
	 (4)echo 2 > /proc/sys/net/ipv4/conf/all/arp_announce
	 (5)ifconfig lo:7 192.168.9.100 netmask 255.255.255.255   配置vip对应到负载服务器
	 (6)yum install httpd -y   		安装httpd
	 (7)cd /var/www/html
	 (8)vi index.html      在里面填写内容from ip 就可以
	 (9)service httpd start   启动httpd 
4.两台lvs服务器配置：
	(1)yum install keepalived     安装高可用工具
	(2)yum install ipvsadm        安装负载均衡管理工具用来看日志
 	(3)cd /etc/keepalived        配置文件目录，把配置文件拷贝一份
 	(4)修改 virtual_ipaddress{192.168.9.100/24 dev eth0 label eth0:7}
 	后面又恶心又复杂没记录。。。
 	scp ./keepalived.conf root@192.168.9.14:/etc/keepalived  拷贝当前目录下的keeplived.conf到以root用户登录的192.168.9.14服务器下的/etc/keepalived
 	
```

Nginx

```
1.高性能的HTTP和方向代理服务器，也是一个IMAP/pop3/SMTP代理服务器。
2.在计算机网络中，反向代理是代理服务器的一种。服务器根据客户端的请求，从其关联的一组或多组后端服务器（如Web服务器）上获取资源，然后再将这些资源返回给客户端，客户端只会得知反向代理的IP地址，而不知道在代理服务器后面的服务器簇的存在[1]。
3.nginx和apache最核心的区别在于apache是同步多进程模型，一个连接对应一个进程，nginx是异步的，多个连接（万级别）可以对应一个进程

安装Nginx
1.yum install gcc pcre-devel openssl-devel -y   安装前置包
2.
```

