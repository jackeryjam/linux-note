# vsftpd

### 安装
yum install -y vsftpd  

### 控制
systemctl start/stop/restart vsftpd.service  
or  
service vsftpd start/stop/restart  

### ftp客户端(可选)
yum install -y ftp  
用这个客户端每次试试能不能连到自己的vsftpd  

### 设置为开机启动(可选)
systemctl enable vsftpd.service   
or    
chkconfig vsftpd on  

### 登录账号
登录的账户有两种，一种是linux的账号，另外一种是ftp的虚拟账户
* 匿名账号(ftp的虚拟账户)
匿名账号Annoymous是否能登录以及所拥有的权限是由vsftpd.conf决定的，默认是只有读的权限  
* linux账号
(用Linux账号登录的目录是对应每个账号的用户主目录，所以想让登录的根目录出现在哪里可以通过新建linux账号来实现  )
添加账号：useradd –d /var/ftp -s /sbin/nologin ftpuser  #-d指向ftp默认目录，也可以自己选择目录，-s是为了安全吧，也可以不用，最后是用户名  
修改密码：passwd ftpuser  
参考：http://www.runoob.com/linux/linux-user-manage.html  
* 虚拟账号  
不做介绍，可参考：http://blog.51cto.com/cuimk/1306637  

### 账号管理
这个很重要，就是设置好账号，如果在黑名单中呢，就访问不了了，emmmmmmmm  
黑名单ftpusers，默认那些root用户什么的都在黑名单中，如果要用root用户登录，那么就要把root在ftpusers中去掉  
这个user_list有时候是黑名单，有时候又是白名单，真的坑，下面参考写得很稳  
参考：http://blog.csdn.net/bluishglc/article/details/42273197  

### 防火墙问题(centos7为例)
ftp协议是通过21端口传数据，20端口进行控制的，如果端口防火墙，不免就会发生服务器错误  
* 直接关掉，简单暴力  
systemctl stop firewalld.service #停止firewall  
systemctl disable firewalld.service #禁止firewall开机启动  
* 设置让ip21通过  
firewall-cmd --list-ports #查看已经开放的端口
firewall-cmd --zone=public --add-port=21/tcp --permanent  #开启端口  
firewall-cmd --reload  #重启防火墙

iptable可参考http://www.linuxidc.com/Linux/2016-12/138979.htm  

### SELinux
emmmmm，这个很安全，但是呢，就又会导致无法访问等问题  
* 直接关掉，简单暴力  
修改/etc/selinux/config文件中的SELINUX="" 为 disabled，或者permissive（这个就降低安全等级而已），然后重启 
如果不想重启系统，使用命令setenforce 0  
注：  
setenforce 1 设置SELinux 成为enforcing模式  
setenforce 0 设置SELinux 成为permissive模式  
* 上面的似乎不是很安全
setsebool -P ftp_home_dir on  
setsebool -P allow_ftpd_full_access on  
参考方案：http://blog.51cto.com/chenpiaoping/1329919   

#### 最全面教程http://blog.csdn.net/wave_1102/article/details/50651433
