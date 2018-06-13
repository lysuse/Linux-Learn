---------------《1》命令部分 ----------------
more 分页查看文件内容
head 查看文件头几行数据 head -20 filennae
tail 查看文件尾行数据 tail -5 filename
软连接文件权限 lrwxrwxrwx  创建方式 ln -s source target
硬链接文件权限 类似cp+知识还可以同步更新创建时间值等不相同
chmod u +
      g -
      o =
u - 所有者
g - 所属组
o - 其他人
rwx 可读可写可执行 对目录列出|创建、删除|进入
umask 默认权限 777-022=真正的权限值
linux 权限规则，缺省创建文件任何时候不能授予可执行X权限
find -name 文件名
* 匹配任意字符 init*
? 匹配单个字符 init??
-size 文件大小 block数据块 512字节=05KB
大于 +
小于 -
等于　＝
－user 文件所有者
-a  and 逻辑与 o or 

alias 定义别名 如： alias drm="rm -rf"
删除别名 unalias drm

重定向 > >> 
管道符 |
输入重定向 < 如广播newfile1的内容 wall < newfile1

linux 流的三中 输入流 0 输出流 1 错误流 2

wc -l 统计行数

&&前面命令执行成功后才执行后面的命令
||前面的命令执行失败后才执行后面的命令

vi编辑器的使用
三种模式
1.命令模式
2.末行命令模式
3.插入编辑模式
1 到  2  用 : / ?
2 到  1 自动返回
1 到  3 用i a 
3 到  1 用ESC键

在命令模式下用noG定位到nu行 如666G定位到666行或在末行命令下输入行号(G)
设置显示行号，在末行命令下输入set nu
dd 删除光标所在行
ndd 删除n行
D 删除光标至行尾的内容
:no1,n2d删除指定范围的行
yy、Y复值当前行
nyy、ny 复制n行
p、P粘贴内容到光标下一行或上一行
u 取消上一步操作
/string 向前搜索 忽略大小写:set ic
:r !date 将当期系统时间导入到文件末尾
用ctrl+v+ctrl+字符 实现输入ctrl键+字符


---------------《Linux系统引导部分及内核》----------------------
Linux 引导流程


固件firmware (CMOS/BIOS)   ->系统自检
自举程序BootLoader	   ->载入内核				 术语MBR(Master Boot Record)
载入内核Kernel 		   ->驱动硬件
启动进程init
读取执行配置文件/etc/init

linux 内核文件: kernel /vmlinuz-2.6.18-164.el5 ro root=LABEL=/ rhgb quiet
2表示主版本号，6表示次版本号,奇数代表测试版


Linux 系统的shell环境配置
1) .bash_history --- 记录你以前输入的命令序列
2) .bash_logout --- 当用户退出shell时，需要执行的命令列表
3) .bash_profile --- 当用户登录shell时，需要执行的命令序列
4) .bashrc --- 每次打开一个新的shell时，需要执行的命令
5) .inputrc --- 用户的键盘设定

/etc/rc.d/init.d 为系统启动的服务列表文件夹
cp -R apache-tomcat-7.0.54 /usr

/var/log
系统日志文件
Korn shell 的环境设置
.profile 用户主目录下


在 /etc/rc.d/rc3.d 目录下存在的文件：

K01dnsmasq         K89rdisc            S25netfs
K02avahi-dnsconfd  K99readahead_later  S25pcscd
K02NetworkManager  S00microcode_ctl    S26acpid
K05conman          S02lvm2-monitor     S26haldaemon
K05saslauthd       S03vmware-tools     S26hidd
K05wdaemon         S04readahead_early  S28autofs
K10psacct          S05kudzu            S55sshd
K20nfs             S06cpuspeed         S56cups
K24irda            S08ip6tables        S56rawdevices
K35vncserver       S08iptables         S56xinetd
K35winbind         S08mcstrans         S57vmware-tools-thinprint
K50netconsole      S10network          S80sendmail
K69rpcsvcgssd      S11auditd           S85gpm
K73ypbind          S12restorecond      S90crond
K74ipmi            S12syslog           S90xfs
K74nscd            S13irqbalance       S95anacron
K74ntpd            S13portmap          S95atd
K85mdmpd           S14nfslock          S97rhnsd
K87multipathd      S15mdmonitor        S97yum-updatesd
K88wpa_supplicant  S18rpcidmapd        S98avahi-daemon
K89dund            S19rpcgssd          S99firstboot
K89netplugd        S22messagebus       S99local
K89pand            S25bluetooth        S99smartd

在这些文件中:
S99smartd
S-start 启动时调用
K-kill 停止时调用


Linux 运行级别
# Default runlevel. The runlevels used by RHS are:
#   0 - halt (Do NOT set initdefault to this)
#   1 - Single user mode
#   2 - Multiuser, without NFS (The same as 3, if you do not have networking)
#   3 - Full multiuser mode
#   4 - unused
#   5 - X11
#   6 - reboot (Do NOT set initdefault to this)


Linux 启动步骤
firmware
   |
BootLoader
   |
Kernel
   |
 init
   |
/etc/inittab
   |
initdefault
   |
/etc/rc.d/rc.sysinit
   |
/etc/rc.d/rc
   |
/etc/rc.d/rcN.d N=0-6
   |
username
password


启动顺序

firmware CMOS/BIOS  ---POST
	|
       MBR
   BootLoader  GRUB  root - /boot kernel- initrd-
        |
      kernel 载入内核（加载硬件驱动程序)
        |
      init   PID=1 恒为1,父进程为内核调度程序 PID=0
	|
      /etc/inittab  id:runlevels:action:process id:运行级别:动作(wait[等待]，ctrlaltdel[按此组合键执行的命令],powerfail[系统电源未连接]
	|
      initdefault
	|
     /etc/rc.d/rc.sysinit 	系统设置
	|
     /etc/rc.d/rc
	|
     /etc/rc.d/rcN.d N=0-6	S satrt K kill 5 运行优先级 服务名称,一般生成软连接
	|
     username password

Grub配置应用

/etc/grub.conf<- /boot/grub

在grub.conf中default=0 表示默认启动顺序第一个,可以修改为第二个2
timeout=5设置启动延时时间
splashimage 设置启动界面的图片
(hd0,0)表示boot分区

启动 进入到grub界面 按ESC然后按e编辑命令选择kenel ，然后输入运行级别就可启动到指定的运行级别,然后按b(boot)进入系统
进入单用户模式不需要密码,如果要想进入单用户是需要输入grub密码，因此可以采用grub-md5-crypt 命令
password --md5 加密后的密码
root(hd0,0)
kernel /vmlinux-2.....
initrd /init....

如果在grub.conf的配置出差错，可能系统无法正确引导，此时可以在grub界面，键入c进入命令行状态,直接键入cat /grub/grub.conf下
查看配置文件的配置信息，找出错误位置，然后手动输入引导信息即可,最后boot 即可

Linux设置系统时钟频率
hwclock --systohc 用系统时间来设置cmos时间
date --hctosys 用cmos时间来设置系统时间

------------《Linux 软件包管理》-------------

1.二进制软件包管理(RPM,YUM)
2.源代码包安装
3.脚本安装(Shell或Java脚本)
4.Debian系Linux软件包管理简介

一.RPM包管理
sudo-1.7.2pl-5e1.i386.rpm
其中包括软件名(sudo),版本号(1.7.2pl),发行号(5.e15),和硬件平台(i386)

  (1)卸载
    #rpm -e sudo  使用--nodeps强行卸载。
  (2)安装
    #rpm -ivh sudo-1.7.2pl-5e1.i386.rpm
    rpm -q sudo查询软件包是否已安装
    --excludedocs 不安装软件包中的文档文件
    --prefix PATH 将软件安装到由PATH指定的路径下
    --test 只对安装进行测试，并不进行实际安装
   (3)升级
    rpm -Uvh sudo---XXXX 升级
    rpm -V 软件名称 检验文件的md5值,即可判断文件是否被修改过{5 文件的md5值,S 文件的大小，M 文件的权限}
二.YUM包管理
   1.自动解决软件包依赖关系
   2.方便软件包升级
   
   安装 yum install
   检测升级 yum check-update
   升级 yum update
   软件包查询 yum list packageName
   软件包信息 yum info
   卸载 yum remove
   帮助 yum -help ,man yum

------------《Linux网络设置》------------------

/etc/hosts
IP地址 主机名或域名   别名
Windows的hosts文件在C:\Windows\System32\drivers\etc下

NIS  -Network Information System(现在一般不使用)
文件集中管理
NIS Server /etc/passwd /etc/shadow

DNS --Domain Name System
BIND

客户端  ---->www.baidu.com
1.本机dns服务器
缓存
2.根域.
---->.com
3.顶级域.com
---->baidu.com
返回DNS服务器 --->  客户端
MAC ----->IP
    ----->IP
IP  ----->MAC
    ----->MAC

cluster 集群  有心跳检测机制

Domain -----> IP
       -----> IP

IP ------> Domain
   ------> Domain

设置IP
/etc/sysconfig/network-scripts/ifcfg-eth0
IPADDR=新的IP地址
NETMASK=子网掩码
BROADCAST=广播地址
GATEWAY=网关

/etc/ysconfig/network
HOSTNAME=主机名

/etc/rc.d/init.d/network 网络启动脚本

/etc/resolve.conf 指定DNS服务器地址(多个小于等于3个)

ifconfig eth0 down/up

ethtool eth0

zerba 路由软件

netstat -an 查看所有连接

-----------------------《Linux用户管理》---------------
用户存放文件 /etc/passwd
root:x:0:0:root:/root/bin/bash
用户名:密码位(X):用户ID,组ID,描述信息,用户主目录

Linux用户分为三种:
超级用户 (root，UID=0)
普通用户 (UID 500-60000)
伪用户 (UID 1-499) 

伪用户与系统和程序服务相关

用户密码等存放文件/etc/shadow
lysuse:$1$IJ7XuqZh$YsX4fglQPMM0Rr/LWDZrX0:16245:0:99999:7:::
如果将加密后的密码删除,将可以不要密码登陆

考虑一个问题:
在Linux系统中，允许普通用户修改密码，然而/etc/passwd和/etc/shadow 文件只有超级管理员才有写权限
请问这是怎么回事呢？
答:因为查看/usr/bin/passwd文件可发现其权限为:
-rwsr-xr-x 1 root root 27768 Jul 17  2006 /usr/bin/passwd
可以看到它的可执行位变为了s，这个s代表setuid=2的意思
setuid指的是当一个可执行程序具有s权限,那么可执行命令将会以所有者的身份执行，恰好在系统
里面的大部分可执行命令的所有者都是root权限,所以就可以执行写操作（root灵魂附体),如在自己执行setuid命令时，如果这个文件不是可执行
的将会显示S；
如：
[lysuse@localhost test]$ ls -l b.txt
-rw-r--r-- 1 root root     69 Jun 27 04:14 b.txt	
[lysuse@localhost test]$ sudo chmod u+s b.txt
[lysuse@localhost test]$ ls -l b.txt
-rwSrw-rw- 1 root root     29 Jun 27 04:14 b.txt	//显示为大写S，因为不为可执行程序无可执行权限
[lysuse@localhost test]$

为文件设置setuid可以采用以下方法
1.chmod u+s

2. 4755 因为大多数可执行文件权限为755

谨慎使用改命令


可以通过 chmod u-s 实现取消setuid权限

setGid
chmod 6755

如果一个目录具有rwx权限，那么就具有删除目录下的文件的权限，即使文件权限比较高

如果一个权限为777的目录被设置了黏着位，每个用户都可以在目录下创建文件，但可以删除自己是所有者的文件.
设置黏着位命令 chmod o+t /test


find -perm -XXXX 查找所有文件权限为XXXX的文件和目录
find -perm -4000 -o -perm 2000 查找所有被设置了setuid权限及setGid权限的文件及文件夹

添加用户 useradd -u 6666 -g root G sys,apache -d /backup -s /bin/bash -c "project zhang xiaoguang" -e 指定用户实现时间

u UID
g 缺省用户所属组
G 指定用户所属多个组
d 宿主目录
s 命令解释shell
c 描述信息
e 指定用户失效时间 

passwd sam 修改sam密码

usermod 修改用户
usermod -G softgroup sample 将用户sample添加到softgroup 组中

gpasswd -R webmin 禁止用户切换组
gpasswd -d 从用户组中删除用户
