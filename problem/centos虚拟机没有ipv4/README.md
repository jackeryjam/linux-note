# centos只有ipv6，没有ipv4
cd /etc/sysconfig/network-scripts/   
ls
编辑没有ipv4对应网卡的文件，之后把最后的ONBOOT=no改成ONBOOT=yes就可以了

如下：  
TYPE=Ethernet  
PROXY_METHOD=none  
BROWSER_ONLY=no  
BOOTPROTO=dhcp  
DEFROUTE=yes  
IPV4_FAILURE_FATAL=no  
IPV6INIT=yes  
IPV6_AUTOCONF=yes  
IPV6_DEFROUTE=yes  
IPV6_FAILURE_FATAL=no  
IPV6_ADDR_GEN_MODE=stable-privacy  
NAME=ens33  
UUID=3ef87d57-2401-46d6-aba8-1614b423d387  
DEVICE=ens33  
ONBOOT=YES  

重启网卡： 
 /etc/init.d/network restart  
激活ens33，启动网络  
 ifup ens33  
