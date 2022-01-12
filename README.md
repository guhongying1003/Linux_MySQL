# Linux
## Linux常用命令
### CentOS安装及设置

#### CentOS下载

```markdown
centos7下载
	https://vault.centos.org/7.6.1810/isos/x86_64/
centos8下载 :
	http://mirrors.aliyun.com/centos/8/isos/x86_64/CentOS-8.3.2011-x86_64-dvd1.iso
```

#### CentOS安装

```markdown
Linux 安装时 分区配置
	boot  1G
		挂载点 /boot
		期望容量 1024MB
		设备类型 标准分区
		文件系统 ext4
	
	swap  2G
		挂载点 空
		期望容量 2048MB
		设备类型 标准分区
		文件系统 swap
		
	root(根分区) 17G
		挂载点 /
		期望容量 17G (硬盘为20G)
		设备类型 标准分区
		文件系统 ext4
```

```markdown
Linux 安装时 Kdump
	内核崩溃转储机制
	生产环境勾上
	测试环境不用勾
```

```markdown
Linux 安装 网络和主机名
	打开以太网
	主机名可更改
```

```markdown
Linux 安装 SECURITY POLICY (安全策略)
	一般可以关闭
```

```markdown
Linux 安装
	设置root密码
	添加一个新用户
```



#### 网络连接三种方式

```markdown
1. NAT模式
	主机IP: 192.168.0.104(随机分配)
	主机VMnet8的 IP地址为: 192.168.239.1 (设置为固定IP)
	           子网掩码为: 255.255.255.0
	----------------------------------
	虚拟机VMnet8 子网IP: 192.168.239.0
	            子网掩码: 255.255.255.0
	            网关:  192.168.239.2 
	            起始IP地址: 192.168.239.128
	            结束IP地址: 192.168.239.254
	------------------------------------            
	CentOS的IP固定为: 192.168.239.128
	
2. 仅主机模式
	主机IP: 192.168.0.104(随机分配)
	主机VMnet1的 IP地址为: 192.168.4.1 (设置为固定IP)
	           子网掩码为: 255.255.255.0
	------------------------------------           
	虚拟机VMnet1 子网IP: 192.168.4.128
                子网掩码: 255.255.255.0
                起始IP: 192.168.4.128
                结束IP: 192.168.4.254
    -----------------------------------
    Win2003的IP固定为: 192.168.4.10
	
	
3. 桥接模式 (一般不用, 会占用网段)
	VMnet0 随机分配即可

```



#### 虚拟机克隆

```markdown
1. 可直接复制, 双击.vmx文件, 可直接打开虚拟机

2. 克隆, 克隆时先关闭系统
	系统上右击-->管理-->克隆-->
	虚拟机中当前状态-->
	创建完整克隆-->
	填写虚拟机名称--> 选择位置
```



#### 虚拟机快照

```markdown
1. 创建
 	系统上右击--> 快照--> 拍摄快照-->填写快照名称及描述

2. 管理
	系统上右击--> 快照--> 快照管理--> 选择快照 --> 转到
```



#### 虚拟机移除

```markdown
1. 系统上右击--> 移除
2. 找到系统存放目录--> 删除
```



#### 安装VMtools

```markdown
介绍
1. vmtools安装后, 可以让我们在windows下更好管理vm虚拟机
2. 可是设置windows和centos的共享文件夹

安装
1. 进入centos, 弹出ContOS7在桌面的光驱
2. 点击vm菜单 install vmware tools
3. cenots会出现一个vm的安装包, xx.tar.gz
4. 拷贝到/opt
5. 解压
	tar -zxvf xxxxx.tar.gz
6. 进入该vm解压目录, /opt目录下
7. 安装./vmware-install.pl
8. 全部默认设置即可, 就可以安装成功
9. 注意: 安装vmtools需要有gcc

设置共享文件夹
1. 为了方便可以设置一个共享文件夹, 比如: d:/myshare
	系统右击->setting(选择), 共享文件夹总是启用, 选择主机上的共享文件夹d:/myshare
    在CentOS /mnt/hgfs/ 下可用访问共享文件夹
    
注意: 
	实际开发中, 通过远程方式传输文件
```





### Linux基础

#### Linux目录结构

```markdown
Linux中一切皆文件
会把硬件映射成文件

Linux目录结构
/bin [常用] (/usr/bin, /usr/local/bin)
是Binary的缩写, 这个目录存放着最经常使用的命令

/sbin (/usr/sbin, /usr/local/sbin)
s就是Super User的意思, 这里存放的是系统管理员使用的系统管理程序

/home[常用]
存放普通用户的主目录, 在Linux中每个用户都有一个自己的目录,一般改目录是以用户的账号命名.

/root[常用]
改目录为系统管理员, 也称为超级权限者的用户主目录

/lib
系统开机所需要最基本的动态连接共享库, 其作用类似于Windows里的DLL文件. 几乎所有的应用程序都需要用到这些共享库.

/lost+found 这个目录一般情况下是空的, 当系统非法关机后, 这里就存放一些文件

/etc[常用]
所有系统管理所需要的配置文件和子目录 my.conf

/usr[常用]
这是一个非常重要的目录,用户的很多应用程序和文件都放在这个目录下, 类似与Windows下的program files目录

/boot[常用]
存放的是启动Linux时使用的一些核心文件, 包括一些链接文件以及镜像文件

/proc
这个目录是一个虚拟的目录, 它是系统内存的映射, 访问这个目录来获取系统信息

/srv 
service缩写, 改目录存放一些服务启动之后需要提取的数据

/sys 
这是Linux2.6内核的一个很大变化, 改目录下安装了2.6内核中新出现的一个文件系统sysfs

/tmp
这个目录是用来存放一些临时文件.

/dev
类似于windows的设备管理器, 把所有的硬件用文件的形式存储

/media [常用]
linux系统会自动识别一些设备, 例如U盘,光驱等, 当识别后, linux会把识别的设备挂载到这个目录下

/mnt [常用]
系统提供目录是为了让用户临时挂载别的文件系统的, 我们可以将外部的存储挂载在/mnt/上,然后进入该目录就可以查看里面的内容了, D:/myshare

/opt
这是给主机额外安装软件所摆放的目录 , 如安装ORACLE数据库就可以放到该目录下, 默认为空.

/usr/local [常用]
这是另一个给主机额外安装软件所安装的目录, 一般是通过编译源码方式安装的程序

/var [常用]
这个目录中存放着不断扩充着的东西, 习惯将经常被修改的目录放在这个目录下, 包括各种日志文件

/selinux [security-enhances linux]
SELinux是一种安全子系统, 它能控制程序只能访问特定文件, 有三种工作模式, 可以自行设置
```



#### 远程登录

```markdown
XShell
Xftp
```



#### vi,vim命令

```markdown
Linux系统会内置vi文本编辑器
Vim具有程序编辑的能力, 可以看做是Vi的增强版本,可以主动的以字体颜色辨别语法的正确性,
方便程序设计. 代码补完,编译及错误跳转等方面编程的功能特别丰富, 在程序员中被广泛使用.
```

```markdown
vi和vim三种模式
1. 正常模式
	以vim打开一个档案就直接进入一般模式(这是默认模式).在这个模式中, 你可以使用[上下左右]按键来移动光标,你可以使用[删除字符]或[删除整行]来处理档案内容, 也可以使用[复制,粘贴]来处理你的文件数据.
	
2. 插入模式
	按下i,I,o,O,a,A,R等任何一个字母之后才会进入编辑模式,一般来说按i即可.

3. 命令行模式
	在这个模式中,可以提供你相关指令, 完成读取,存盘,替换,离开vim,显示行号等动作则是在此模式中达成的!

```

```markdown
模式切换
一般模式  i或者a  编辑模式   esc   一般模式
一般模式  :或者/  命令行模式   esc   一般模式

退出vim编辑
命令行模式下:  wq(保存并退出) 或者 q(退出) 或者 q!(强制退出,不保存)

快捷键使用
1. [一般模式]拷贝当前行 yy, 拷贝当前行向下的5行, 5yy 并粘贴 (输入p).
2. [一般模式]删除当前行 dd, 删除当前想向下的5行, 5dd.
3. [命令模式]在文件中查找某个单词[命令行下 /+关键字,回车查找,输入n就是查找下一个]
4. [命令模式]设置文件的行号, 取消文件的行号.[命令行下 :set nu 和 :set nonu]
5. [一般模式]编辑/etc/profile文件,使用快捷键到该文档的最末行[G]和最首行[gg]
6. [一般模式]在一个文件中输入"hello",然后又撤销这个动作 u
7. [一般模式]编辑/etc/profile文件,并将光标移动到 20行 Shift+g

```



#### Linux关机重启

```markdown
shutdown -h now     立即进行关机
shutdown -h 1       一分钟后会关机
shutdown -r now     立即重启
halt                立即关机
reboot              立即重启
sync                把内存数据同步磁盘

注意
1. 不管是重启还是关机, 首先运行sync, 把内存中数据写到磁盘中
2. 目前的 shutdown/reboot/halt 等命令均已经在关机前进行了sync.
```

#### Linux登录注销

```markdown
基本介绍
1. 登录时尽量少用root账号登录, 因为它是系统管理员, 最大的权限,避免操作失误.可以利用普通用户登录, 登录后再用"su - 用户名" 命令来切换成系统管理员身份.
2. 在提示符下输入logout即可注销用户.

注意:
1. logout注销指令在图形运行级别无效,在运行级别3下有效

song --> su -root --> root用户 --> 执行logout --> song用户-->logout-->断开连接


```



#### Linux用户管理

```markdown
Linux系统是一个多用户多任务的操作系统, 任何一个要是用系统资源的用户, 都必须首先向系统管理员申请一个账号, 然后以这个账号的身份进入系统.

----------------------  添加用户  ------------------------
基本语法
useradd 用户名
如: 创建一个用户tom    useradd tom
细节说明
1. 当创建用户成功后, 会自动创建和用户同名的家目录
2. 也可以通过useradd -d 指定目录 新的用户名,给新创建的用户指定家目录

---------------------- 指定/修改密码 --------------------------
基本语法
passwd 用户名
如: 给tom 指定密码  在root下  passwd tom 回车 输入密码

pwd 显示当前用户所在目录
-------------------------- 删除用户 ----------------------------
基本语法
userdel 用户名

如: 删除tom,但是保留价目录     ---     userdel tom
    删除用户以及用户主目录,比如tom    ---    userdel -r tom

---------------------------- 查询用户信息指令 -----------------------------    
基本语法
id 用户名
如: 查询root用户信息  id root

细节说明
当用户不存在时,返回无此用户

------------------------------   切换用户  --------------------------------
介绍
在操作Linux中, 如果当前用户的权限不够, 可以通过 su - 指令, 切换到高权限用户, 比如root

基本语法
su - 切换用户名

如: 创建jack用户, 指定密码,然后切换到jack

说明
1. 从权限高的用户切换到权限低的用户, 不需要输入密码,反之需要.
2. 当需要返回到原来用户时, 使用exit/logout指令

-------------------------  查看当前用户/登录用户  -------------------------
基本语法
whoami 或者 who am i(信息更详细)

注意:
who am i 显示的是第一次登录的账号信息
```

```markdown
*********************************** 组管理 *************************
类似于角色,系统可以对有共性/权限的多个用户进行统一的管理

-- **添加组**
groupadd 组名

-- **删除组**
groupdel 组名

-- **增加用户时直接加上组**
useradd -g 用户组 用户名

如: 新增一个组grouptest, 然后新增一个用户usertest, 并加入组grouptest
groupadd grouptest
useradd -g grouptest usertest

-- **修改用户的组**
usermod -g 用户组 用户名

如: 新增一个组grouptest2 , 把usertest,放入grouptest2组中
groupadd grouptest2
usermod -g grouptest2 usertest

-- **用户和组相关文件**
/etc/passwd文件
用户(user)的配置文件, 记录用户的各种信息
	每行的含义: 用户名:口令:用户标识号:组标示号:注释性描述:主目录:登录Shell

/etc/shadow文件
口令的配置文件
	每行的含义: 登录名:加密口令:最后一次修改时间:最小时间间隔:最大时间间隔:警告时间:不活动时间:失效时间:标志

/etc/group文件
组(group)的配置文件, 记录Linu包含的组的信息
	每行含义: 组名:口令:组标识号:组内用户列表
```



#### 指定运行级别

```markdown
**基本介绍**
运行级别说明
0 : 关机
1 : 单用户[找回丢失密码]
2 : 多用户状态没有网络服务
3 : 多用户状态有网络服务
4 : 系统未使用保留给用户
5 : 图形界面
6 : 系统重启
常用运行级别是3和5, 也可以指定默认运行级别, 后面演示

命令 : init[0123456]应用案例 : 通过init来切换不同的运行级别, 比如5-3, 然后关机

CentOS7后运行级别说明
在centos7以前, /etc/inittab文件中.
进行了简化, 如下:
multi-user.target:analogous to runlevel 3
graphical.target:analogous to runlevel 5

# To view current default target,run:  查看级别
systemctl get-default
# To set a default target, run:  设置级别
 systemctl set-default multi-user.target  # 设置成3这个级别
 systemctl set-default graphical.target   # 设置成5这个级别
```



#### 找回root密码

```markdown
CentOS7以后
1. 首先,启动系统,进入开机界面,在界面中按"e"进入编辑界面.
2. 进入编辑界面, 使用键盘上的上下键把光标往下移动, 找到以"Linux16"开头内容所在行数,在行的最后面输入: init=/bin/sh
3. 接着, 输入完成后, 直接按快捷键: Ctrl+x 进入单用户模式
4. 接着, 在光标闪烁的位置中输入: mount -o remount,rw / (注意:各个单词间有空格),完成后按键盘的回车键(Enter)
5. 在新的一行最后面输入: passwd, 完成后按键盘的回车键(Enter). 输入密码, 然后再次确认密码即可,密码修改成功后, 会显示passwd...的样式, 说明密码修改成功.
6. 接着, 在鼠标闪烁的位置中(最后一行), 输入: touch /.autorelabel(注意: touch与/后面有一个空格),完成后按键盘的回车键(Enter)
7. 继续在光标闪烁的位置中,输入: exec /sbin/init(注意: exec与/后面有一个空格),完成后按键盘的回车键(Enter),等待系统自动修改密码,完成后,系统自动重启,新的密码生效.(时间会比较长)
```



#### 帮助指令

```markdown
man 获得帮助信息
基本语法: man [命令或配置文件] (功能描述: 获得帮助信息)
如: 查看ls命令的帮助信息 man ls

help指令
基本语法: help 命令 (功能描述: 获得shell内置命令的帮助信息)
如: 查看cd命令的帮助信息

注意: 
文件中 以"."开头的文件 为Linux隐含文件.
```



#### 文件目录指令

```xml
pwd : 当前目录的绝对路径

ls : 
	-a: 显示当前目录所有文件和目录, 包括隐藏的.
	-l: 以列表的方式显示信息
	-lh : 单位格式化
cd : 
	cd ~ 或者 cd : 回到自己的家目录
	cd .. 回到当前目录的上一级


mkdir指令
	用于创建目录
	mkdir [选项] 要创建的目录
	mkdir -p 目录名称  -- 多级目录

rmdir指令
	rmdir [选项] 要删除的空目录名称
	注意: rmdir删除的是空目录, 如果目录下有内容是无法删除的.
	      如果需要删除非空目录,需要使用 rm -rf 要删除的目录

rm -rf 目录名称 递归删除整个目录


touch指令
	创建空文件
	touch hello.txt
	vim hello.txt 创建并编辑


cp指令
	拷贝文件到指定目录
	cp [选项] source dest
	    -r  :  递归复制整个文件
	将/home/hello.txt 拷贝到 /home/bbb目录下
	在home下 : cp hello.txt bbb/
	
	将bbb整个目录复制到/opt下
	cp -r /home/bbb /opt/      -- 执行两次会覆盖 \cp -r /home/bbb /opt/  覆盖不提醒


rm指令
	移除文件或目录
	rm [选项] 要删除文件或目录
	如: rm /home/hello.txt    -- 会有是否删除提示
		rm -f /home/hello.txt  -- 删除不提示
		rm -rf /home/bbb        -- 递归删除整个目录且不提示


mv指令
	移动文件或目录或重命名
	mv oldNameFile newNameFile       -- 如果在同一个目录则是重命名
	mv /temp/movefile /targetFolder  -- 移动文件

	mv pig.txt /root/cow.txt         -- 移动文件并且重命名
	mv /opt/bbb /home/               -- 移动目录
	

cat指令
	查看文件内容
	cat -n /etc/profile          -- 查看文件并显示行号
	cat -n /etc/profile | more   -- 分页查看


more指令
	more指令是一个基于VI编辑器的文本过滤器, 他以全屏幕的方式按页显示文本文件的内容.
	more指令中内置了若干个快捷键
	
	空白键(space)    代表向下翻一页
	Enter            代表向下翻一行
	q                代表立刻离开more,不再显示该文件内容
	Ctrl + F         向下滚动一屏
	Ctrl + B         返回上一屏
	=                输出当前行的行号
	:f               输出文件名和当前行的行号

	more /etc/profile


less指令
	less指令用来分屏查看文件内容, 他的功能与more指令类似,但是比more指令更加强大,支持各种显示终端.
	less指令在显示文件内容时, 并不是一次将整个文件加载之后才显示, 而是根据显示需要加载内容,
	对于显示大型文件具有较高的效率
	
	空白键			向下翻一页
	[pagedown]  	向下翻动一页
	[pageup]		向上翻动一页
	/[字符串]       向下搜索[子符串]的功能; n: 向下查找; N : 向上查找;         
	?[字符串]       向上搜索[字符串]的功能; n: 向上查找; N : 向下查找;
	q



echo指令
	输出内容到控制台
	echo [选项] [输出内容]
	使用echo输出环境变量         echo $HOSTNAME
	使用echo输出hello world!	 echo "Hello world"


head指令
	用于显示文件开头部分内容,默认情况下head指令显示文件的前10行内容.
	head 文件名       -- 查看文件头10行内容
	head -n 5 文件名  -- 查看文件头5行内容, 5可以是人意数字

	head -n 11 /etc/profile

tail指令
	用于输出文件中尾部的内容, 默认情况下tail指令显示文件的前10行内容
	tail 文件名         -- 查看文件尾10行内容
	tail -n 5 文件名    -- 查看文件尾5行内容
	tail -f 文件名      -- 实时追踪该文档的所有更新
	tail -F 文件名      -- 一般使用大F  F指向文件名, f指向实际地址
	
	查看/etc/profile 最后5行代码
	tail -n 5 /etc/profile
	
	实时监控mydate.txt, 看看到文件有变化时,是否看到.
	tail -f mydate.txt

head和tail合用
	head -10 smzj2021.txt | tail -1     -- 取第10行数据


重定向 > 和 追加 >> 指令
	1) ls -l > 文件名       -- 列表的内容写入文件a.txt中(覆盖写)
	2) ls -al >> 文件       -- 列表的内容追加到文件aa.txt的末尾
	3) cat 文件1 > 文件2    -- 将文件1的内容覆盖到文件2中
	4) echo "内容" >> 文件  

	例如:  cat mydate.txt > mydate1.txt   -- 将mydate中数据覆盖到mydate1.txt
		  cat mydate.txt >> mydate1.txt  -- 将mydate中数据追加到mydate1.txt
		  ll > mydate.txt    -- 将文件列表覆盖到mydate.txt
		  ls >> mydate.txt   -- 将文件列表追加到mydate.txt
		  cal >> mydate.txt  -- 将日历信息追加到mydate.txt
	
	ls /home/aaa 2>test.txt  -- 错误输出,如果aaa不存在,则将错误信息输入到test.txt,存在则不输出任何信息
	ll /home >> test.txt 2>&1  -- 无论正确错误都将内容重定向到test.txt文件中.(test.txt自动创建)
	数据黑洞: ll /etc  >> /dev/null 2>&1  -- 不写数据, 比如不让数据打印到屏幕上
	

ln指令
	软链接也称为符号链接, 类似于Windows里的快捷方式, 主要存放了链接其他文件的路径
	
	ln -s [原文件或目录] [软链接名]       -- 给原文件创建一个软链接

	ln -s ./aaa ./myaaa                   -- 给aaa文件夹在同一个目录下创建一个软链接
	rm myaaa							  -- 删除软链接
	
	注意: 使用pwd指令查看目录时, 依然是软链接所在目录

history指令
	查看已经执行过历史命令, 也可以执行历史指令.
	
	history      -- 查看所欲执行过的命令
	history 10   -- 查看最近的10条命令
	!710         -- 执行历史命令编号为710的指令

```



#### 日期时间类

```xml
时间日期类
	date指令 - 显示当前日期
	
	1) date         -- 显示当前时间
	2) date +%Y     -- 显示当前年
	3) date +%M     -- 显示当前月
	4) date +%d     -- 显示当前日

		date "+%Y-%m-%d"   -- 2021-05-07

    5) date -s 字符串时间   -- 设置时间
		date -s "2021-05-06 17:30:00"

cal指令
	查看日历指令
	cal        -- 当前月日历
	cal 2021   -- 2021年日历


centos同步时间
    1、安装ntpdate工具
    [root@slave1 ~]#  yum install -y ntpdate

    2、设置系统时间与网络时间同步
    ntpdate 0.asia.pool.ntp.org

    3、将系统时间写入硬件时间
    [root@slave1 ~]# hwclock --systohc


日期自动同步[多服务器]
	自动同步时间
		yum install ntp -y
		ntpdate cn.ntp.org.cn
	搭建本地NTP服务
		开启本地NTP服务器
			service ntpd start
 		#vi /etc/ntp.conf
			输入:
			#======权限控制======
			restrict default kod nomodify notrap nopeer noquery  拒绝IPV4用户
			restrict -6 defalut kod nomodify notrap nopeer noquery  拒绝IPV6用户
			restrict 210.72.145.44  授权国建授时中心服务器访问本地NTP
			restrict 133.100.11.8   授权133.100.11.8访问本地NTP
			restrict 127.0.0.1
			restrict -6 ::1
			restrict 192.168.150.2 mask 255.255.255.0 nomodify   本地网段授权访问
			#=======源服务器=======
			server cn.ntp.org.cn prefer   指定上级更新时间服务器, 优先使用这个地址
			#=======差异分析=======
			driftfile /var/lib/ntp/drift
			keys  /etc/ntp/keys
```



#### 搜索查找类

```xml
find指令
	find指令将从指定目录向下递归遍历其各个子目录,将满足条件的文件或目录显示在终端.
	find [搜索范围] [选项]
	选项
        -name 	按文件名称查找文件
        -user   查找属于指定用户名所有文件
        -size   安照指定的文件大小查找文件
	* 根据名称查找/home目录下的Hello.java文件
	  find /home -name Hello.java
	* 按拥有者:查找/opt目录下, 用户名为nobody的文件
	  find /opt -user nobody
	* 查找整个linux系统下大于200M的文件(+n 大于 -n 小于 n 等于)
	  find / -size +200M  
注意 : 
	可以组合使用
	find /home/song -user song -name *.java


locate指令
	locate指令可以快速定位文件路径.
	locate指令利用事先建立的系统中所有文件名称及路径的locate数据库事先快速定位给定的文件.
	locate指令无需遍历整个文件系统,查询速度较快. 为了保证查询结果的准确度,管理员必须定期更新locate时刻.
	
	locate 搜索文件
	由于locate指令基于数据库进行查询, 所以第一次运行前,必须使用updatedb指令创建locate数据库.
	
	案例: 请使用locate指令快速定位hello.txt文件所在目录
	第一次使用先执行:
	updatedb
	locate Hello.java

which指令
	可以查看某个指令的存放路径
	which ls


grep指令和 管道符号|
	grep过滤查找, 管道符,"|",表示将前一个命令的处理结果传递给后面的命令处理.
	grep [选项] 查找内容 源文件
	      -n   显示匹配行及行号
		  -i   忽略字母大小写
	
	案例: 在Hello.java 文件中, 查找"Hello" 所在行, 并且显示行号.
		 cat Hello.java | grep  -n "Hello"
		 grep -n "Hello" ./Hello.java
```



#### 压缩与解压

```xml
gzip/gunzip指令
	gzip用于压缩,gunzip用于解压的
	用法:
	gzip 文件   -- 压缩文件,只能将文件压缩为*.gz文件
	gunzip 文件  -- 解压缩文件命令
	例子:
	gzip hello.txt  -- 在同一目录下 产生hello.txt.gz文件,同时hello.txt文件不在
	gunzip hello.txt.gz  -- 同一目录下 产生hello.txt 文件, 同时hello.txt.gz文件不在


zip/unzip指令
	zip用于压缩文件,unzip用于解压文件,这个在项目打包发布中很有用.
	语法:
	zip [选项] XXX.zip 将要压缩的内容  -- 压缩文件和目录的命令
	unzip [选项] XXX.zip             -- 解压缩文件
	
	zip常用选项:
		-r : 递归压缩,即压缩目录
	unzip常用选项:
		-d<目录> : 指定解压后文件的存放目录
            
    例子: 将/home/song下所有文件进行压缩成 mysong.zip   
         zip -r mysong.zip /home/song/   -- 将home/song目录及其目录下所有文件压缩成mysong.zip
         
         将mysong.zip解压到/opt/tmp目录下
         unzip -d /opt/tmp/ /home/song/mysong.zip  -- 将mysong.zip解压到/opt/tmp/下
            
            
tar指令
	打包指令,最后打包后的文件.tar.gz的文件.
    基本语法:
        tar [选项] XXX.tar.gz 打包内容  -- 打包目录,压缩后的文件格式.tar.gz
    选项说明:
        -c     产生.tar打包文件
        -v     显示详细信息
        -f     指定压缩后的文件名
        -z     打包同时压缩
        -x     解包.tar文件
	例子:
        -- 将mydate1.txt , mydate.txt压缩成mydate.tar.gz文件
           tar -zcvf mydate.tar.gz /home/song/mydate1.txt /home/song/mydate.txt
        -- 将/home及其下所有文件 压缩成 myhome.tar.gz  
           tar -zcvf myhome.tar.gz /home    
        -- 将mydate.tar.gz 解压
           tar -zxvf mydata.tar.gz
        -- 将myhome.tar.gz 解压到/opt/temp2目录下
           tar -zxvf /home/song/myhome.tar.gz -C /opt/tmp2
```



#### 查看版本

```shell
[root@centos7_03 /]# cat /etc/*release
CentOS Linux release 7.7.1908 (Core)
NAME="CentOS Linux"
VERSION="7 (Core)"
ID="centos"
ID_LIKE="rhel fedora"
VERSION_ID="7"
PRETTY_NAME="CentOS Linux 7 (Core)"
ANSI_COLOR="0;31"
CPE_NAME="cpe:/o:centos:centos:7"
HOME_URL="https://www.centos.org/"
BUG_REPORT_URL="https://bugs.centos.org/"

CENTOS_MANTISBT_PROJECT="CentOS-7"
CENTOS_MANTISBT_PROJECT_VERSION="7"
REDHAT_SUPPORT_PRODUCT="centos"
REDHAT_SUPPORT_PRODUCT_VERSION="7"

CentOS Linux release 7.7.1908 (Core)
CentOS Linux release 7.7.1908 (Core)
```





### Linux组管理

#### Linux组基本介绍

```markdown
	在linux中每个用户必须属于一个组, 不能独立于组外.
	在linux中每个文件有所有者,所在组,其他组概念.
	
	1. 所有者
	2. 所在组
	3. 其它组
	4. 改变用户所在的组
```



#### 文件/目录 所有者

```markdown
	一般为文件的创建者, 谁创建了该文件, 就自然的成为该文件的所有者.
	
	查看文件的所有者
	指令: ls -all
	
	修改文件所有者
	指令: chown 用户名 文件名
	案例: 使用song用户创建一个apple.txt文件, 将其所有者修改为tom
	     touch apple.txt              -- 创建文件
	     sudo chown tom apple.txt     -- 修改所有者为tom sudo防止权限不够
```



#### 组的创建,修改

```markdown
	创建基本指令
	groupadd 组名
	
	例子: 
		创建一个组, monster
		sudo groupadd monster  -- 使用sudo是因为在song用户下,权限不够
		
		创建一个用户fox, 并放入到 monster组中
		sudo useradd -g monster fox  -- 使用sudo是因为在song用户下,权限不够
		
	修改文件组指令
    chgrp 组名 文件名
    例子:
    	使用root用户创建文件 orange.txt, 看看当前这个文件属于哪个组,然后将这个文件所在组,修改到fruit组.
    	groupadd fruit
    	touch orange.txt
    	chgrp fruit orange.txt
    	
    	
    其它组
    	除文件的所有者和所在组的用户外,系统的其他用户都是文件的其它组
    
    修改用户所在组	
    root权限下
    1. usermod -g 组名 用户名
    2. usermod -d 目录名 用户名 改变改用户登录的初始目录
    
    例子: 将tom这个用户从原来所在组, 修改到jrey组
    cat /etc/group  -- 查看所有组
    cat -n /etc/group | grep jrey  -- 搜索jrey用户组
    
    sudo usermod -g jrey tom  -- 将用户tom所在组, 改为jrey
```



### Linux权限

#### 权限介绍

```markdown
ls -l中显示内容如下:
	-rw-rw-r--. 1 song song 33 5月   7 19:24 cat.txt   -- 文件
	
	drwxrwxr-x. 3 song song 17 5月   6 23:44 bbb       -- 目录
	
	lrwxrwxrwx. 1 song song 11 5月   8 17:52 mysong -> /home/song/    
	                                                   -- 链接 ln -s /home/song/ mysong
    crw-rw-rw-. 1 root root      1,   5 5月   8 17:32 zero  -- 终端设备
                                                   
    brw-rw----. 1 root disk      8,   0 5月   8 17:32 sda   -- 块设备 硬盘                                                  
0-9位说明
	1. 第0位确定文件类型(d,-,l,c,b)
		-是文件
		l是链接, 相当于windows的快捷方式
		d是目录, 相当于windows的文件夹
		c是字符设备文件,鼠标,键盘
		b是快设备, 比如硬盘
	2. 第1-3位确定所有者(该文件所有者)拥有该文件的权限.  ---User
    3. 第4-6位确定所属组(同用户组的)拥有该文件的权限.    ---Group
    4. 第7-9位确定其他用户拥有该文件的权限.             ---Other

```

#### RWX权限

```markdown
** rwx作用到文件 **
	1. [r]代表可读(read): 可以读取,查看
	2. [w]代表可写(write): 可以修改, 但是不代表可以删除,删除一个文件的前提是对该文件所在的目录有写权限,才能删除该文件.
	3. [x]代表可执行(execute): 可以被执行
	
**rwx作用到目录**
	1. [r]代表可读(read): 可以读取,ls查看目录内容
	2. [w]代表可写(write): 可以修改,对目录内创建+删除+重命名目录
	3. [x]代表可执行(execute): 可以进入该目录
	
**用数字表示权限**
	r = 4
	w = 2
	x = 1
	rwx = 4 + 2 + 1 = 7

**其它说明**
	-rw-rw-r--. 1 song song 33 5月   7 19:24 cat.txt
	1                  文件: 硬盘链接数或 目录: 子目录数
	root               用户
	root               组
	1213               文件大小(字节), 如果是文件夹, 显示4096字节
	5月   7 19:24      最后修改时间
	cat.txt            文件名
```

#### 权限修改

```markdown
** 基本说明 **
	通过chmod指令,可以修改文件或目录的权限.
	
	第一种方式 : + - = 变更权限
	u: 所有者 g: 所有组  O: 其他人 a: 所有人(u,g,o总和)
	1)chmod u=rwx,g=rx,o=r 文件/目录名
	2)chmod o+w 文件/目录名
	3)chmod a-x 文件/目录名
	
	案例:
	1) 给abc文件的所有者读写执行权限, 给所在组读执行权限,给其他组读执行权限.
		chmod u=rwx,g=rw,o=r abc
	2) 给abc文件的所有者除去执行的权限,增加组写的权限
		chmod u-x,g+w abc
	3) 给abc文件的所有用户添加读的权限
		chmod a+r abc
		
	第二种方式: 通过数字变更权限
    r = 4
	w = 2
	x = 1
	wx = 3
	rx = 5
	rw = 6
	rwx = 4 + 2 + 1 = 7
	chmod 751 文件目录名
	
	案例: 将abc.txt文件权限修改成 rwxr-xr-x , 使用数字的方式实现
	chmod 755 /home/song/abc.txt
```

#### 修改文件所有者

```markdown
** 基本介绍 **
	chown newowner 文件/目录  -- 修改所有者
	chown newowner:newgroup  文件/目录   -- 修改所有者和所在组
	-R 如果是目录, 则使其下所有子文件或目录递归生效
	
	案例:
	chown tom /home/song/abc.txt 
```

#### 修改文件/目录所在组

```markdown
** 基本介绍 **
	chgrp newgroup 文件/目录   -- 改变所在组
	
	案例:
		groupadd animal
		chgrp animal /home/song/abc.txt -- 将abc.txt文件的所在组改为animal
		
		chgrp -R tom /home/song/test   -- 将test文件夹下所有文件所在组改为tom
```

#### 权限案例

```markdown
** 最佳实践-警察和土匪游戏 **
	police , bandit
	jack ,  jerry :警察
	xh,  xq : 土匪
	1. 创建组
		groupadd police
		groupadd bandit
		
	2. 创建用户
		useradd -G police jack
		useradd -g police jerry
		useradd -g bandit xh
		useradd -g bandit xq
		
		passwd jack  -- 设置Jack登录密码
	3. Jack创建一个文件, 自己可以读写,本组人可以读, 其他组没任何权限
		jack: touch abc.txt
			  ** -rw-r--r--. 1 jack police 52 5月  10 19:04 jack.txt
			  
			  chmod u=rw,g=r,o-r abc.txt 
		   或  chmod 640 jack.txt
			
	4. jack修改该文件, 让其它组人可以读, 本组人可以读写
		jack: chmod g=rw,o=r jack.txt
	5. xh 投靠警察, 看看是否可以读写
		root: usermod -g polic xh
	6. 测试xh是否可以读写, xq是否可以读写
    	注意: 如果要对目录内的文件进行操作, 需要有对该目录的相应权限.
    	root/jack: chmod g=rwx jack 
```

练习

```markdown
	1. 建立两个组(神仙sx, 妖怪yg)
		groupadd sx
		groupadd yg
	
	2. 建立四个用户(唐僧,悟空,八戒,沙僧)
		useradd ts
		useradd wk
		useradd bj
		useradd ss
	3. 设置密码
		passwd ts
		passwd wk
		passwd bj
		passwd ss
	4. 把悟空,八戒放入妖怪 唐僧 沙僧在神仙
		usermod -g yg wk
		usermod -g yg bj
		usermod -g ts sx
		usermod -g ss sx
		
	5. 用悟空建立一个文件(monkey.java 该文件要输出 i am monkey)
		vim monkey.java
		
	6. 给八戒一个可以rw 权限
		chmod g=rw monkey.java
		
	7. 八戒修改monkey.java 加入一句话(i am pig)
		chmod g=rwx wk
		vim monkey.java
		
	8. 唐僧,沙僧对该文件没有权限
		
	9. 把沙僧放入妖怪组
		usermod -g yg ss
		** 重新登录生效, 可以修改monkey.java
		
	10. 让沙僧 修改 该文件 monkey, 加入一句话("我是沙僧, 我是妖怪")
		vim monkey.java
	
	11. 对文件夹rwx的细节讨论和测试
		1. 先收回wk目录权限  
			chmod g-r-w-x wk
		2. 此时该组其他用户没有权限 (进入,修改等等)
			cd: /home/wk: 权限不够
		3. 先给与x权限
		    chmod g+x monkey.java
			同组用户 此时可以进入wk目录
			cd wk
			同组用户 但是不能打开目录
			ls: 无法打开目录下文件列表.: 权限不够
			同组用户可以读,可以写
			-- -rw-rw-r--. 1 wk yg 51 5月  11 00:32 monkey.java
			cat monkey.java  /  vim monkey.java
		
			将读的权限给wk 文件夹
			chmod g+x wk
			
			此时同组用户 可以使用 ls
			
			但是同组用户无法 添加或删除 wk目录的文件
			touch: 无法创建"pig.java": 权限不够
			rm: 无法删除"monkey.java": 权限不够
			
			将写的权限给wk目录
			chmod g+w wk
			
			此时同组用户 可以增加/删除 目录下的文件
			touch pig.txt
			rm pig.txt 
	
    总结:
    	x : 表示可以进入目录,比如cd
		r : 表示可以ls, 将目录内容显示
		w : 表示可以增删目录下的文件
```

#### 总结

```markdown
rm -rf 文件/目录

userdel -r 用户名
groupdel   组名

 组  
	groupadd   xxxx组
	useradd -g xxxx组           --用户创建是添加组 

	chown xxx用户 xxx文件/目录  -- 修改文件所有者
	chgrp xxx组 xxx文件/目录    -- 修改文件所在组
	
	usermod -g xxx组  xxx用户   -- 修改用户所在组
	
 权限
	chmod u=rwx,g=rwx,o=rwx xxx文件/目录
```



### Crond任务调度

```markdown
概述
	任务调度: 是指系统在某个时间执行的特定的命令或程序.
	任务调度分类:
		1. 系统工作: 有些重要的工作必须周而复始地执行. 比如病毒扫描等.
		2. 个别用户工作: 个别用户希望执行某些程序, 比如mysql数据库备份.
	
基本语法:
    crontab [选项]
    	      -e : 编辑crontal定时任务
    	      -l : 查询crontab任务
    	      -r : 删除当前用户所有的crontab任务
    service crond restart [重启任务调度]	      
```

```markdown
命令说明 :
	设置任务调度文件: /etc/crontab
	设置个人任务调度: 执行 crontab -e 命令.
	接着输入任务到调度文件
	*/1 * * * * ls -l /etc/ > /tmp/to.txt
	意思是说每小时的每分钟执行ls -l /etc/ > /tmp/to.txt 命令
	
	参数细节说明
	 项目          含义             范围
	第一个* :  一小时当中的第几分钟    0-59
	第二个* :  一天当中的第几小时      0-23
	第三个* :  一个月当中的第几天      1-31
	第四个* :  一年当中的第几月        1-12
	第五个* :  一周当中的星期几        0-7(0和7都代表星期日)

	特殊符号说明
	* : 代表任何时间. 比如第一个*, 就代表一小时中每分钟都执行一次的意思.
	, : 代表不连续的时间. 比如"0 8,12,16 ***命令",代表每天的8点0分,12点0分,16点0分执行命令
	- : 代表连续时间范围,比如"0 5 * * 1-6命令" , 代表周一到周六的凌晨5点0分执行命令
	*/n : 代表每隔多久执行一次. 比如 "*/10 * * * * 命令", 代表每隔10分钟执行一遍命令
```

```markdown
特定时间案例
	时间                  含义
	45 22 * * * 命令      在22点45分执行命令
	0  17 * * 1 命令      每周一的17点0分执行命令
    0  5  1,15 * * 命令   每月1号和15号的凌晨5点0分执行命令
    40 4 * * 1-5 命令     每周1到周五的凌晨4点40分执行命令
    */10 4 * * 1-5 命令   每天凌晨4点, 每隔10分钟执行一次命令
    0 0 1,15 * 1  命令    每月1号和15号, 每周1的0点0分都会执行命令. 
    注意: 星期几和几号最好不要同时出现,它们定义的都是天 . 非常容易混淆
```

```markdown
应用案例
	案例1: 每隔1分钟,就将当前日期信息, 追加到/tmp/mydate 文件中
		   */1 * * * * date > /tmp/mydate.txt
	案例2: 每隔1分钟,就将当前日期和日历都追加到/home/mycal 文件中
	       */1 * * * * date >> /tmp/mydate.txt
	       */1 * * * * cal >> /tmp/mydate.txt
	       
	       或者:
               vim my.sh
               输入 : date >> /home/mycal
                      cal >> /home/mycal
               再给与my.sh执行权限
               		chmod u+x my.sh
               执行
               		./my.sh -- 手动执行脚本
               设置调度每分钟自动执行脚本
               		crontab -e
              		*/1 * * * * /home/my.sh
	       
	案例3: 每天凌晨2:00 将mysql数据库testdb,备份到文件中.
		   0 2 * * * mysqldump -u root -padmin mydb2 >> /home/db.bak
```



### at定时任务

#### 基础说明

```markdown
基本介绍
	1. at命令是一次性定时计划任务, at的守护进程atd会以后台模式运行,检查作业队列来运行.
	2. 默认情况下,atd守护进程每60秒检查作业队列, 有作业时,会检查作业运行时间,如果时间与当前时间匹配,则运行次作业.
	3. at命令是一次性定时计划任务, 执行完一个任务后不再执行此任务.
	4. 在使用at命令的时候, 一定要保证atd进程的启动,可以使用相关指令来查看.
	
at命令格式
	at [选项] [时间]
		-m    指定任务被完成后,将给用户发送邮件,即使没有标注输出
		-I    atq的别名(查询)
		-d    atrm的别名(删除)
		-v    显示任务将被执行的时间
		-c    打印任务的内容到标准输出
		-V    显示版本信息
		-q<队列>   使用指定的队列
        -f<文件>	 从指定文件读入任务而不是从标准输入读入
        -t<时间参数>   以时间参数的形式提交要运行的任务
		
	Crtl + D 结束at命令的输入

查看进程
	ps -ef -- 检测当前正在运行的所有进程有哪些
	ps -ef|grep atd -- 查看atd进程
```

```markdown
at时间定义

at指定时间的方法:
	1. 接受在当天的hh:mm(小时:分钟)式的时间指定.假如该时间已过去,那么久放在第二天执行. 例如 04:00
	2. 使用midnight(深夜),noon(中午),teatime(饮茶时间,一般是下午4点)等比较模糊的词语来指定时间.
	3. 采用12小时计时制,即在时间后面加上AM(上午)或PM(下午)来说明是上午还是下午.例如:12PM
	4. 指定命令执行的具体日期,指定格式为month day(月 日)或mm/dd/yy(月/日/年)或dd.mm.yy(日.月.年),指定的日期必须跟在指定时间后面.例如: 04:00 2021-03-01
	5. 使用相对计时法.指定格式为: now + count time-units, now就是当前时间, time-units是时间单位,这里能够是minutes(分钟),hours(小时),days(天),weeks(星期).count是时间的数量,几天,几小时.例如:now + 5 minutes
	6. 直接使用today(今天),tomorrow(明天)来指定完成命令的时间.
```

#### 案例

```markdown
应用案例
	案例1: 2天后的下午5点执行 /bin/ls /home
			at 5pm + 2 days
			/bin/ls /home
			--输入两次
			Ctrl + D 
			会得到
			[job 1 at Fri May 14 17:00:00 2021]
			
	案例2: atq命令来查看系统中没有执行的工作任务
			-- 查看at 任务
			atq
			
	案例3: 明天17点钟,输出时间到指定文件内 比如 /root/date100.log
			at 5pm tomorrow
			date > /root/date100.og
			Ctrl + D  -- 两次
			[job 2 at Thu May 13 17:00:00 2021]
			
	案例4: 2分钟后,输出时间到指定文件内 比如 /root/date200.log
			at now + 2 minutes
			date > /root/date200.log
			Ctrl + D -- 两次
	案例5: 删除已经设置的任务,atrm 编号
			atrm 3
```



### Linux磁盘分区

#### Linux分区

```markdown
原理结束
	1. Linux来说无论有几个分区,分给哪一目录使用,它归根结底就只有一个根目录,一个独立且唯一的文件结构.linux中每一个分区都是用来组成整个文件系统的一部分.
	2. Linux采用了一种叫"载入"的处理方法,它的整个文件系统中包含了一整套的文件和目录,且将一个分区将使它的存储空间在一个目录下获得.


硬盘说明
	1. Linux硬盘分IDE硬盘和SCSI硬盘, 目前基本上是SCSI硬盘
	2. 对于IDE硬盘,驱动器标识符为"hdx~",其中"hd"表明分区所在设备的类型,这里是指IDE硬盘了.
	"x"为盘号(a为基本盘,b为基本从属盘,c为辅助主盘,d为辅助从属盘),"~"代表分区,
	前四个分区用数字1~4表示,它们是主从区或扩展分区,从5开始就是逻辑分区.
	例如: hda3表示为第一个IDE硬盘上的第三个主分区或扩展分区,hdb2表示从第二个IDE硬盘上的第二个主分区或扩展分区.
	3. 对于SCSI硬盘则表识为"sdx~",SCSI硬盘是用"sd"来表示分区所在设备类型,其余则和IDE硬盘的表示一样. 例如: sda a表示第一块硬盘. sda1 表示第一块硬盘第一个分区
```

```markdown
查看所有设备挂载情况
	命令 : lsblk  
		   NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
            sda      8:0    0   20G  0 disk 
            ├─sda1   8:1    0  200M  0 part /boot
            ├─sda2   8:2    0    2G  0 part [SWAP]
            └─sda3   8:3    0 17.8G  0 part /
	  或者 lsblk -f
	        NAME   FSTYPE LABEL UUID                                 MOUNTPOINT
            sda                                                      
            ├─sda1 xfs          6ee70d88-2d48-4eac-a5a8-98beaf076477 /boot
            ├─sda2 swap         ec437157-224f-433d-8e26-6c4207368f13 [SWAP]
            └─sda3 xfs          2499b9da-bbb5-44ba-89a3-6917df793b70 /
```



#### 挂载经典案例

```markdown
说明: 
	下面我们以增加一块硬盘为例来熟悉下磁盘的相关指令和深入理解磁盘分区,挂载,卸载的概念.

如何增加一块硬盘
	1. 虚拟机添加硬盘
		在[虚拟机]菜单中,选择[设置],然后设备列表里添加硬盘,然后一路[下一步],
		中间只有选择磁盘大小的地方需要修改,直到完成.然后重启系统(才能识别).
		重启: reboot 
		此时lsblk 可以看到 sdb硬盘
		
	2. 分区
		fdisk /dev/sdb  -- 新增分区文件
		输入m
		再输入n
		再输入p
		再输入1   -- 一个分区
		输入w     -- 保存并退出
		
	3. 格式化
		mkfs -t ext4 /dev/sdb1
		-- 其中ext4是分区类型
		
	4. 挂载
		在根目录下创建newdisk
		mkdir newdisk
		mount /dev/sdb1  /newdisk/  -- 挂载
		注意: 用命令行挂载, 重启后会失效,sdb1并没有挂载到newdisk
		-- 取消挂载
		umount /dev/sdb1
		
	5. 设置可以自动挂载(永久挂载)
		通过修改/etc/fstab实现挂载
		vim /etc/fatab
		添加 /dev/sdb1   /newdisk  ext4  defaults  0 0
		添加完成后, 执行mount -a 即刻生效
```

#### 磁盘情况查询

```markdown
查询磁盘整体使用情况
	df -h
查询指定目录的磁盘占用情况
	du -h /目录
	查询指定目录的磁盘占用情况,默认为当前目录
	-s  指定目录占用大小汇总
	-h  带计量单位
	-a  含文件
	--max-depth=1  子目录深度
	-C 列出明细的同时, 增加汇总值

应用实例
1. 查询/opt目录的磁盘占用情况, 深度为1
	du -h --max-depth=1  /opt
	du -ha --max-depth=1 /opt  -- 包含目录和文件
	du -hac --max-depth=1 /opt  -- 汇总
```



#### 磁盘情况工作使用指令

```markdown
1. 统计/opt文件夹下文件的个数
	ls -l /opt|grep "^-"             -- 显示以"-"开头的文件
	ls -l /opt |grep "^-" | wc -l    -- 统计以"-"开头的文件
	
2. 统计/opt文件夹下目录的个数
	ls -l /opt |grep "^d" | wc -l
	
3. 统计/opt文件夹下文件的个数,包括子文件夹里的
	ls -lR /opt | grep "^-" | wc -l
	
4. 统计/opt文件夹下目录的个数,包括子文件夹里的
	ls -lR /opt |grep "^d" | wc -l

5. 以树状显示目录结构
	安装tree指令
	yum install tree
	tree /opt  或者 tree /opt | more
```



### Linux网络

```markdown
NAT网络
	VMent8
		IP : 192.168.150.1
		子网: 255.255.255.0
	虚拟机VMent8
		子网IP : 192.168.150.0
		子网掩码: 255.255.255.0
		网关IP : 192.168.150.2
		起始IP地址: 192.168.150.128
		结束IP地址: 192.168.150.254
```

#### 指定IP

```markdown
说明
	直接修改配置文件
	vim /etc/sysconfig/network-scripts/ifcfg-ens33
	需要修改参数
	BOOTPROTO=static                   -- 分配静态IP
	IPADDR=192.168.150.128             -- 设置IP地址
	GATEWAY=192.168.150.2              -- 设置网关
	DNS1=192.168.150.2                 -- 域名解析器
	
	-- 删除UUID
	
	-- 重启网络生效
	service network restart  或者  reboot

参数说明
TYPE=Ethernet                      -- 网络类型
PROXY_METHOD=none
BROWSER_ONLY=no
BOOTPROTO=static                   -- 分配静态IP
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=ens33
UUID=41934dbb-7811-4d86-aa39-a5072504062f    -- 随机id
DEVICE=ens33                                 -- 设备名称
ONBOOT=yes                                   -- 系统启动时网络接口是否有效
IPADDR=192.168.150.128                       -- 设置IP地址
GATEWAY=192.168.150.2                        -- 设置网关
DNS1=192.168.150.2                           -- 域名解析器
```

#### 设置主机名

```markdown
设置主机名
1. 为了方便记忆,可以给Linux系统设置主机名, 也可以根据需要修改主机名
2. 指令hostname : 查看主机名
3. 修改文件在/etc/hostname指定
4. 修改后,重启生效

设置hosts映射
如何通过主机名能够找到某个Linux系统?
 **windows**
 在c:\Windows\System32\drivers\etc\hosts 文件指定即可
 案例: 192.168.150.130 hadoop01
 
 **Linux**
 在/etc/hosts 文件指定
 案例: 192.168.150.130 hadoop01
 
 Hosts是什么
 	一个文本文件,用来记录IP和Hostname(主机名)的映射关系
 DNS
 	1. DNS,就是Domain Name System的缩写,翻译过来就是域名系统.
 	2. 是互联网上作为域名和IP地址相互映射的一个分布式数据库.
 
```



### Linux进程

#### 基本介绍

```markdown
1. 在每个Linux中,每个执行的程序都称为一个进程. 每个进程都分配一个ID号(pid,进程号)
2. 每个进程都可能以两种方式存在的. 前台与后台, 所谓前台进程就是用户目前的屏幕上可以进行操作的.后台进程则是实际在操作,但由于屏幕上无法看到的进程, 通常使用后台方式执行.
3. 一般系统的服务都是以后台进程的方式存在,而且都会常驻在系统中. 直到关机才结束.
```

#### 查看系统进程

```markdown
基本介绍
	ps命令是用来查看目前系统中,有哪些正在执行,以及它们执行的状况,可以不加任何参数.
	PID   进程识别号
	TTY   终端机号
	TIME  次进程所消CPU时间
	CMD   正在执行的命令或进程名
	
	ps -a : 显示当前终端的所有进程信息
	ps -u : 以用户的格式显示进程信息
	ps -x : 显示后台进程运行的参数
```

```markdown
ps -aux 参数分析
	USER      进程执行用户  
	PID       进程号
	%CPU      占用CPU的百分比
	%MEM      占用物理内存的百分比
	VSZ       占用虚拟内存大小 KB
	RSS       占用物理内存大小 KB
	TTY       终端信息    
	STAT      运行状态 s表示sleep,r表示运行,N表示进程优先级低,D短期等待,Z 僵死进程,
	          T被跟踪或被停止等等
	START     执行开始时间
	TIME      占用CPU时间
	COMMAND   启动该进程的指令和参数
```



#### 父子进程

```markdown
以全格式显示当前所有的进程,查看进程的父进程
	ps -ef 是以全格式显示当前所有的进程
	   -e  显示所有的进程.  -f  全格式
	ps -ef|grep xxx    -- 查看特定进程
    是BSD风格
    UID : 用户ID
    PID : 进程ID
    PPID : 父进程ID
    C : CPU用于计算执行优先级的因子.数值越大,表明进程是CPU密集型运算,执行优先级会降低;
        数值越小,表明进程是I/O密集型运算,执行优先级会提高.
    STIME : 进程启动的时间
    TTY :  完整的终端名称
    TIME : CPU时间
    CMD  : 启动进程所用的命令和参数
```

#### 终止进程

```markdown
kill [选项] 进程号
killall 进程名(子进程也会被终止)

常用选项
	-9 : 表示强制进程立即停止
	
最佳实践
	案例1: 踢掉某个非法登录用户
	  ps -aux|grep sshd  -- 查询tom用户登录进程id
      [root  3677  0.2  0.2 161012  5580 ?  Ss   18:28   0:00 sshd: tom [priv]]
      kill 3677   -- 终止tom用户进程
	案例2: 终止远程登录服务sshd,在适当时候再次重启sshd服务
	  kill 进程号
	  /bin/systemctl start sshd.service
	案例3: 终止多个gedit
	   同时打开abc.txt 和 abcd.txt
	   killall gedit  -- 关掉文本编辑器abc.txt 和 abcd.txt全部关闭
	案例4: 强制杀掉一个终端
		在本地打开两个终端
		ps -ef|grep bash  -- 查看本地的终端
		kill -9 PID   --  强制终止
```

#### 查看进程树pstree

```markdown
基本语法
	pstree [选项], 可以更加直观的来看进程信息

常用选项
	-p : 显示进程的PID
	-u : 显示进程的所属用户

应用实例
	案例1: 请以树状的形式显示进程pid
		pstree     -- 显示所有进程
		pstree -p  -- 显示进程和进程号
	案例2: 请以树状的形式进程的用户id
	    pstree -u   -- 该进程属于哪个用户
```

#### 后台管理nohup &

& : 后台运行  ping www.baidu.com >> test.txt & 

改服务会后台运行(jobs 命令查看), 但是重新开启一个链接后,就找不到该进程了.

nohup : 挂载   nohup ping www.baidu.com >> test.txt &   

会报一个错误, 要让正确和错误的情况都写入文件 

完整写法:

nohup  ping www.baidu.com  >> test.txt  2>&1 &



### 服务管理

```markdown
介绍
服务(service)本质就是进程,但是是运行在后台的,通常都是会监听某个端口,等待其他程序的请求,
比如(mysqld,sshd,防火墙等),因此我们有称为守护进程,是Linux中非常重要的知识点.
```

```markdown
service管理服务
	1. service  服务名 [start | stop | restart | reload | status]
	2. 在CentOS7.0后 很多服务不再使用service, 而是 systemctl.
	3. service 指令管理的服务在/etc/init.d查看
        ls -l /etc/init.d/     -- 查看哪些服务被service管理
        [-rw-r--r--. 1 root root 18281 8月  19 2019 functions]
        [-rwxr-xr-x. 1 root root  4569 8月  19 2019 netconsole]
        [-rwxr-xr-x. 1 root root  7928 8月  19 2019 network]
        [-rw-r--r--. 1 root root  1160 4月   1 2020 README]

service管理指令案例
	请使用service指令,查看,关闭,启动 network[注意:在虚拟系统演示,因为网络连接会关闭]

查看系统服务
    setup
	tab键退出
```

#### 服务运行级别

```markdown
服务的运行级别(runlevel)
Linux系统又7种运行级别: 常用的是3和5
运行级别0: 系统停机状态,系统默认运行级别不能设为0,否则不能正常启动.
运行级别1: 单用户工作状态,root权限,用于系统维护,禁止远程登录.
运行级别2: 多用户状态(没有NFS),不支持网络.
运行级别3: 完全的多用户状态(有NFS),登陆后进入控制台命令行模式
运行级别4: 系统未使用,保留
运行级别5: X11控制台,登录后进入图形GUI模式.
运行级别6: 系统正常关闭并重启,默认运行级别不能设为6,否则不能正常启动.

开机流程说明
	开机 -> BIOS -> /boot -> systemd进程1 -> 运行级别 -> 运行级对应的服务
查看当前运行级别
	systemctl get-default
使用命令设置运行级别
	systemctl set-default multi-user.target  -- 设置运行级别为3
	systemctl set-default graphical.target   -- 设置运行级别为5
```

#### chkconfig指令

```markdown
介绍
	1. 通过chkconfig 命令可以给服务的各个运行级别设置 自 启动/关闭.
	2. chkconfig 指令管理的服务在 /etc/init.d 查看
	3. 注意: CentOS7.0后,很多服务使用systemctl管理

基本语法
	查看服务 chkconfig  --list [| grep xxx]
	[netconsole     	0:关	1:关	2:关	3:关	4:关	5:关	6:关]
	[network        	0:关	1:关	2:开	3:开	4:开	5:开	6:关]

	chkconfig 服务名 --list
	chkconfig --level 5 服务名 on/off

案例: 对network服务 进行各种操作
	chkconfig --level 5 network off  -- 关闭network服务在运行级别为5下的自启动
	[network        	0:关	1:关	2:开	3:开	4:开	5:关	6:关]
	chkconfig --level 5 network on   -- 打开network服务在运行级别为5下的自启动
	[network        	0:关	1:关	2:开	3:开	4:开	5:开	6:关]

注意: chkconfig重新设置服务后自启动或关闭, 需要重启机器reboot生效.	
```



#### systemctl指令

```markdown
systemctl管理指令
   1.基本语法: systemctl [start | stop | restart | status]服务名
   2.systemctl指令管理的服务在 /usr/lib/systemd/system查看

systemctl设置服务的自启动状态
   1.systemctl list-unit-files [|grep 服务名] (查看服务名开机启动状态,grep可以过滤)
   2.systemctl enable 服务名 (设置服务开机启动,3和5级别)
   3.systemctl disable 服务名 (关闭服务开机启动,3和5级别)
   4.systemctl is-enable 服务名 (查询某个服务是否有自启动)

应用案例: 
	查看当前防火墙的状况,关闭防火墙和重启防火墙.
	ls -l /usr/lib/systemd/system | grep fire  -- 查询防火墙相关指令
	[-rw-r--r--. 1 root root  657 4月   1 2020 firewalld.service]
	-- 查询防火墙状态
	systemctl status firewalld
	-- 查看防火墙开机启动状态
	systemctl list-unit-files | grep firewalld
	[firewalld.service                             enabled]
	--是否开机启动
	systemctl is-enabled firewalld


细节讨论:
1.关闭或者启用防火墙后,立即生效. [telnet 测试, 某个端口即可]
	systemctl stop firewalld   -- 临时的
	systemctl start firewalld   
2.这种方式只是临时生效,当重启系统后,还是回归以前对服务的设置.
3.如果希望设置某个服务自启动或关闭永久生效,要使用systemctl [enable|disable] 服务名.
    firewalld.service
    systemctl enable firewalld    -- 设置开机启动
```



#### firewall指令

```markdown
**打开或者关闭指定端口**
	在真正的生产环境,往往需要将防火墙打开,但问题来了,如果我们把防护墙打开,那么外部请求数据包就不能跟服务器监听多扣通讯. 这时,需要打开指定的端口. 比如80,22,8080等.

** firewall 指令 **
打开端口: firewall-cmd --permanent --add-port=端口号/协议
关闭端口: firewall-cmd --permanent --remove-port=端口号/协议
重新载入,才能生效: firewall-cmd --reload
查询端口是否开放: firewall-cmd --query-port=端口/协议

-- 查询端口是那种协议
netstat -anp | more
-- 当前主机在监听哪些端口
netstat -tlunp

** 应用案例 **
1.启用防火墙,测试111端口是否能telnet
	systemctl start firewalld
	telnet 192.168.150.239 111
2.开放111端口
	-- 开放111端口
	firewall-cmd --permanent --add-port=111/tcp
	-- 重新载入
	firewall-cmd --reload
	-- 查询端口是否开放
	firewall-cmd --query-port=111/tcp
3.再次关闭111端口
	-- 关闭111端口
	firewall-cmd --permanent --remove-port=111/tcp
	-- 重新载入
	firewall-cmd --reload
	-- 查询端口是否开放
	firewall-cmd --query-port=111/tcp
```

#### 软件安装限制

- 操作系统对未知软件的安装有可能拒绝或者警告,我们需要禁用这个功能

- vi  /etc/selinux/config

  SELINUX=disabled

#### 文件传输

- yum install lrzsz -y

  ​	rz :  将文件从window上传到linux

  ​	sz : 将文件从Linux传输到Window

  ​	通用方式xftp软件

- Linux --> Linux

  - scp 源数据地址(source)  目标数据地址(target)
  - scp apache-tomat-7.0.16.tar.gz root@192.168.150.131:/opt
  - scp  hello.txt  root@192.168.150.130:/opt/  -- 将本机hello.txt文件发送至130机器的/opt目录下
  - scp  root@192.168.150.131:/root/hello.txt  /opt  -- 将131机器的hello.txt文件,下载到本机/opt下
  - scp -r base root@192.168.150.130:/opt/

#### 文件大小

- 分区信息
  - df  -h
- 指定文件目录大小
  - du  -h  --max-depth=1  apache-tomcat-7.0.61
- swap
  - 一个特殊分区, 以硬盘代替内存
  - 当内存使用满的时候, 可以将一部分数据写出到swap分区

#### 主机间相互免密钥

如果直接130 访问131 

可以把130的公钥发送给131, 在130上直接 ssh 192.168.150.131 可以直接登录, 不要密码



- 可以通过ssh免密钥登录其它机器

- 如果是第一次建立连接, 需要输入yes

  - 在 ~/.ssh/konwn_hosts 文件记录了以前访问地址
  - 在访问地址的时候如果没有收录到known_hosts文件中,就需要输入yes
  - 如果以前收录到known_hosts中, 直接输入密码即可

- 需要输入密码

  - 生产密钥
    - ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
  - 发送密钥
    - ssh-copy-id -i ~/.ssh/id_rsa.pub root@192.168.150.131
  - 再次登录
    - ssh 192.168.150.131   [不在需要输入密码,本机130]

- 主机名与Host校验

  - cd .ssh/

  - vim  known_host

  - 经常会弹出需要输入yes的地方

  - 方法:

    - 修改/etc/ssh/ssh_config文件配置, 以后不会出现此问题

    - 最后添加:

    - StrictHostKeyChecking no

      UserKnownHostsFile /dev/null

  - 最后输入ssh xxx.131 或者 ssh centos01  都不会再输入yes确认

#### 动态监控top

```markdown
** 介绍 **
top与ps命令很相似.它们都用来显示正在执行的进程.Top与ps最大不同之处,在于top在执行一段时间可以更新正在运行的进程.

** 基本语法 **
top [选项]

选项说明
-d秒数     指定top命令每隔几秒更新.默认是3秒
-i        使top不显示任何闲置或者僵死进程.
-p        通过指定监控进程ID来仅仅监控某个进程的状态

交互操作说明
P         以CPU使用率排序,默认就是此项
M         以内存的使用率排序
N         以PID排序
q         退出top

u         监视特定用户
k         终止进程
** 应用实例 **
案例1. 监视特定用户
top: 输入此命令,按回车键,查看执行的进程
u: 然后输入"u" 回车, 再输入用户名,即可

案例2: 终止指定的进程
top: 输入此命令,按回车键,查看执行的进程
k:   然后输入"k"回车, 再输入要结束的进程ID号

案例3: 指定系统状态更新的时间(每个10秒自动更新),默认是3秒.
top -d 10
```

#### 查看网络情况

```markdown
** netstat **
基本语法
netstat [选项]

选项说明
-an    按一定顺序排列输出
-p     显示哪个进程在调用

应用案例
请查看服务名为sshd的服务的信息
	netstat -anp | grep sshd

检查主机连接命令ping
是一种网络检测工具,它主要是用检测远程主机是否正常,或是两部主机间的网线或网卡故障.
如: ping 对方ip地址
```



### Linux三剑客

#### 普通三剑客

- cut
  - 用指定的规则来切分文本
  - cut -d ':' -f1,2,3 passwd | grep root
    - passwd文件中用":"作为切分条件, 展示出3列
- sort
  - sort lucky
    - 对文本中的进行排序, 按字母顺序
  - sort -t ':' -k3 passwd
    - 将文本passwd按照":"进行切分,按照第三列进行排序
  - sort -t ':' -k3 -r passwd
    - 逆序排序
  - sort -t ':' -k3 -n passwd
    - 按照数值大小进行排序,如果有字母,字母在前
- wc
  - 统计单词的数量
  - wc  hello.txt
  - 45   91 2342 passwd
    - -l  line
    - -w  word 以空格来分割单词
    - -c  char

#### 剑客一号:grep

- 可以对文本进行搜索
- 同时搜索多个文件
  - 从文档中查询指定的数据
  - grep  hello  hello.txt
  - grep  hello  hello1.txt  hello2.txt
- 显示匹配的行号
  - grep -n hello hello.txt
- 显示不匹配的忽略大小写
  - grep -nvi  root  passwd  --color=auto
- 使用正则表达式匹配
  - grep  -E  "[1-9]+" passwd  --color=auto 

#### 剑客二号

- sed是Stream Editor(字符流编辑器)的缩写, 简称编辑器
- Sed软件从文件或管道中读取一行,处理一行, 输出一行;再读取一行,在处理一行,在输出一行.
- 一次一行的设计使得sed软件性能很高.
- vi命令打开文件是一次性将文件加载到内存





### RPM与YUM

#### RPM

```markdown
** rpm包管理 **
介绍
rpm用于互联网下载包的打包及安装工具,它包含在某写Linux分发版中.它生成具有. RPM扩展名的文件.
RPM是RedHat Package Manager (RedHat软件包管理工具)的缩写,类似windows的setup.exe,
这以文件格式虽然打上了RedHat的标志,但理念是通用的.

Linux的分发版都有采用(suse,redhat,centos等等),可以算是公认的行业标准了.

** rpm包的简单查询指令 **
查询已安装的rpm列表 
rpm -qa|grep xxx
	rpm -qa | grep mysql   -- 查看MySQL
	rpm -qa |grep firefox  -- 查看是否安装了火狐
	[firefox-68.5.0-2.el7.centos.x86_64]

rpm -q 软件包名: 查询软件包是否安装
	rpm -q firefox

rpm -qi 软件包名 : 查询软件包信息
	rpm -qi firefox
rpm -ql 软件包名 : 查询软件包中的文件
 	rpm -ql firefox

rpm -qf 文件全路径名 : 查询文件所属的软件包
	rpm -qf /etc/passwd
	rpm -qf /root/install.log

** 卸载RPM包 **
rpm -e RPM包名称
	rpm -e firefox
注意: 如果其他软件包依赖于要卸载的软件包,卸载则会产生错误信息.
比如: rpm -e foo  -- 会产生错误信息
强制删除 rpm -e --nodeps foo  -- 一般不推荐

** 安装RPM包 **
	rpm -ivh RPM包全路径名称
	查找安装位置
	find / -name java  或者 whereis java

参数说明
i = install 安装
v = verbose 提示
h = hash   进度条

案例演示:
rpm -ivh /home/firefox-68.2.2-1.el7.centos.X86-64.rpm
```

#### YUM

```markdown
** 介绍 **
Yum是一个Shell前端软件包管理器.基于RPM包管理,能够从指定的服务器自动下载RPM包并且安装,可以自动处理依赖关系,并且一次安装所有依赖的软件包

** yum的基本指令 **
	yum search ifconfig   在线查询命令或软件
	yum info net-tools    在线查询已安装的软件信息
	yum list | grep java  在线查询安装的rpm包,或者只查询某一周

	yum search ifconfig  -- 查找安装包
	[root@centos02 home]# yum search ifconfig
    [已加载插件：fastestmirror, langpacks
       Repodata is over 2 weeks old. Install yum-cron? Or run: yum makecache fast
       Determining fastest mirrors
       * base: mirrors.nju.edu.cn
       * extras: mirrors.aliyun.com
       * updates: mirrors.aliyun.com
       =======匹配：ifconfig ==================
       net-tools.x86_64 : Basic networking tools]

	yum install net-tools -y  -- 安装ifconfig包 -y 自动安装依赖

** 案例 **
请用yum的方式安装firefox
	rpm -e firefox  -- 卸载firefox
	rpm list | grep firefox  -- 查询yun服务器适合安装的firefox
	[firefox.i686     78.10.0-1.el7.centos       updates ] 
    [firefox.x86_64   78.10.0-1.el7.centos       updates]
	rpm install firefox.x86_64   -- 安装firefox

更换yum源
安装wget
	yum install wget -y
修改配置 
	cd /etc/yum.repos.d/
备份配置	
	mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
下载配置
	wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
清空以前yum源
	yum clean all
获取阿里云的缓存
	yum makecache
```



### 安装JDK

```xml
** 安装步骤 **
1.	mkdir opt/jdk
2.	通过xftp上传到 /opt/jdk下
3.	cd /opt/jdk
4.	解压tar -zxvf jdk1.8.0_261 /usr/loacl/java
5.	mkdir /usr/local/java
6.	mv /opt/jdk/jdk1.8.0_261 /usr/local/java
7.	配置环境变量的配置文件vim /etc/profile
8.	export JAVA_HOME=/usr/local/java/jdk1.8.0_261
9.	export PATH=$JAVA_HOME/bin:$PATH
10.	source /etc/profile [让文件生效]
11. 测试效果
	vim Hello.java
	javac.Hello.java
	java Hello
```



### 安装Tomcat

```xml
** 安装步骤 **
1. 上传安装文件, 并解压到/opt/tomcat
2. 进入解压目录/bin, 启动tomcat  ./startup.sh
3. 开放端口8080
	-- 查询端口是否开放
	firewall-cmd --query-port=8080/tcp  
	-- 开放8080端口
	firewall-cmd --permanent --add-port=8080/tcp
	-- 重新加载, 使设置生效
	firewall-cmd --reload
```



### 安装idea

```xml
1. 下载地址 https://www.jetbrains.com/idea/download/download-thanks.html?platform=linux
2. 解压到/opt/idea
3. 启动idea bin目录下 ./idea.sh, 配饰jdk
4. 编写Hello World 程序测试
```



### 安装MySQL

```markdown
下载mysql
解压mysql
删除自带数据库
	-- 查询自带数据库
	rpm -qa | grep mari
	-- 删除自带数据库
	rpm -e --nodeps mariadb-libs
	rpm -e --nodeps marisa

安装依赖perl 和 net-tools
	yum install perl net-tools -y
安装Mysql
	tar -xvf mysql-8.0.18-1.e17.x86_64.rpm-bundle.tar
	
	rpm -ivh mysql-community-common-8.0.18-1.e17.x86_64.rpm
	rpm -ivh mysql-community-libs-8.0.18-1.e17.x86_64.rpm
	rpm -ivh mysql-community-client-8.0.18-1.e17.x86_64.rpm
	rpm -ivh mysql-community-server-8.0.18-1.e17.x86_64.rpm

运行启动mysql
	systemctl start mysqld   

设置root密码 ,查看密码
	cat /var/log/mysqld.log | grep password 

进入mysql 
	mysql -u root -p

设置root密码,Mysql8.0版本
	set global validate_password.policy=LOW;  -- 设置密码策略
	set global validate_password.length=6
	alter user 'root'@'localhost' identified by '123456' password expire naver;
	alter user 'root'@'localhost' identified with mysql_native_password by '123456';
	flush privileges;  刷新权限
	
设置root密码,Mysql5.7版本
	set global validate_password_policy=LOW;
	set global validate_password_length=6;
	alter user root@localhost identified by '123456';

修改链接地址
	use mysql;
	update user set host='%' where user = 'root';
	commit;

重启
	systemctl restart mysql
```



### Shell编程

```markdown
** Shell是什么 **
	Shell是一个命令行解释器, 它为用户提供了一个向Linux内核发送请求以便运行程序的界面系统级程序,用户可以用Shell来启动,挂起,停止甚至是编写一些程序.

** 脚本格式要求 **
1.脚本以#!/bin/bash 开头
2.脚本需要有可执行权限

** 编写一个脚本 **
常见一个Shell脚本, 输出hello world!
	-- 创建hello.sh
	vim hello.sh
	-- 输入
	#!/bin/bash
	echo "hello,world!"	
	-- 赋予x权限
	chmod u+x hello.sh
	-- 执行
	./hello.sh
	
	如果不给权限
	则执行 sh hello.sh

** 常用脚本执行方式 **
方式1: (输入脚本的绝对路径或相对路径)
  说明: 首先要赋予helloworld.sh脚本的x权限,再执行脚本
方式2: (sh+脚本)
  说明: 不用赋予脚本x权限, 直接执行即可.
```



#### Shell变量

```markdown
** Shell变量 **
1.Linux Shell中的变量分为, 系统变量和用户自定义变量.
2.系统变量: $HOME,$PWD,$SHELL,$USER等等, 比如: echo $HOME等等
3.显示当前shell中所有变量: set

** shell变量的定义 **
1.定义变量: 变量=值
2.撤销变量: unset 变量
3.声明静态变量: readonly 变量, 注意,不能unset

1.变量名称可以由字母,数字,下划线组成,不能以数字开头
2.等号两侧不能有空格
3.变量名称一般习惯为大写

将命令的返回值赋给变量
1.A=`date` 反引号,运行里面的命令,并把结果返回给变量A
2.A=$(date) 等价于反引号
    C=`date`
    echo c=$C
    D=$(date)
    echo D=$D


** 快速入门 **
案例1: 定义变量A
案例2: 撤销变量A
案例3: 声明静态的变量B=2, 不能unset
案例4: 可把变量提升为全局环境变量,可供其他shell程序使用
	#!/bin/bash
    #案例1: 定义变量A
    A=100
    #输出变量需要加上$
    echo "A=$A"
    echo A=$A 
    #案例2: 撤销变量A
    unset A
    #案例3: 声明静态的变量B=2, 不能unset
    readonly B=2
    echo B=$B
```

#### 设置环境变量

```markdown
** 基本语法 **
1.export 变量名=变量值 (将shell变量输出为环境变量/全局变量)
2.source 配置文件  (让修改后的配置信息立即生效)
3.echo $变量名     (查询环境变量的值)

** 快速入门 **
1.在/etc/profile 文件中定义TOMCAT_HOME环境变量
	vim /etc/profile
	##tomcat
	export TOMCAT_HOME=/opt/module/tomcat/apache-tomcat-8.5.59
	source /etc/profile
2.查看环境变量TOMCAT_HOME的值
	echo $TOMCAT_HOME
3.在另外一个shell程序中使用TOMCAT_HOME
	E=$TOMCAT_HOME
	echo E=$E
注意: 在输出TOMCAT_HOME环境变量前,需要让其生效 source /etc/profile
```



#### 位置参数变量

```markdown
** 介绍 **
当我们执行一个shell脚本时,如果想获取参数信息,就可以使用位置参数变量.

** 基本语法 **
$n (n为数字,$0代表命令本身,$1-$9代表第一到第九个参数,十以上的参数,用大括号表示,${10})
$* (获取所有参数,把所有参数看成一个整体) *
$@ (获取所有参数,把每个参数区分对待)
$# (获取参数个数)

** 例子 **
	vim position.sh
	输入
	#!/bin/bash
    echo "$0 $1 $2"
    echo "所有参数 : $*"
    echo "$@"
    echo "参数个数: $#"
    执行
    sh position.sh 100 200
    得到
[position.sh 100 200]
[所有参数 : 100 200]
[100 200]
[参数个数: 2]
```

#### 预定义变量

```markdown
** 介绍 **
就是shell设计者事先定义好的变量,可以直接在shell脚本中使用

** 基本语法 **
$$ (获取当前进程的进程号PID)
$! (后台运行的最后一个进程的进程号PID)
$? (最后一次执行的命令的返回状态, 返回0,说明上一个命令执行成功,非0,则执行错误)

** 例子 **
	vim preVar.sh
	输入
    #!/bin/bash
    echo "当前执行的进程id: $$"
    #以后台方式运行一个脚本,并获取它的进程号
    sh /home/position.sh 100 200 &
    echo "最后一个后台方式运行的进程id: $!"
    echo "上一个命令是否成功: $?"
	执行
	sh preVar.sh
	结果
[当前执行的进程id: 2390]
[最后一个后台方式运行的进程id: 2391]
[上一个命令是否成功: 0]
[[root@hadoop01 home]# /home/position.sh 100 200]
[所有参数 : 100 200]
[100 200]
[参数个数: 2]
```

#### 运算符

```markdown
** 基本介绍 **
学习如何在shell中进行各种运算操作

** 基本语法 **
1.$((运算式)) 或者 $[运算式] 或者 expr m + n (注意空格)
2.expr要有空格,如果希望将expr的结果赋给某个变量,使用``
3.expr m - n
4.expr \*,/,%  乘,除,取余

** 例子 **
例子1 : 计算 (2+3)*4
	vim jisuan.sh
	输入
    #!/bin/bash
    #方式一
    echo "(2+3)*4 = $(((2+3)*4))"
    #方式二
    echo "(2+3)*4 = $[(2+3)*4]"
    #方式三
    temp=`expr 2 + 3`
    res4=`expr $temp \* 4`
    echo "tmep=$temp"
    echo "res4=$res4"
	执行
	sh jisuan.sh
	结果
[(2+3)*4 = 20]
[(2+3)*4 = 20]
[tmep=5]
[res4=20]	

例子2: 计算两个参数的和
	#计算连个参数的和
	echo "参数和: $[$1+$2]"
	执行
	sh jisuan.sh 1 1
	结果
[参数和: 2]	
```



#### 条件判断

```markdown
** 判断语句 **
[ condition ] (注意: condition前后要有空格)
'#'非空返回true,可以用$?验证(0为true, >1为false)

** 应用实例 **
[ tom ]       返回true
[ ]           返回false
[ condition ] && echo OK || echo notok    满足条件,执行后面的语句
```

```markdown
** 判断条件 **
=  字符串比较
-lt 小于
-le 小于等于
-eq 等于
-gt 大于
-ge 大于等于
-ne 不等于

按照文件权限进行判断
-r 有读的权限
-w 有写的权限
-x 有执行的权限

按照文件类型进行判断
-f 文件存在并且是一个常规的文件
-e 文件存在
-d 文件存在并且是一个目录
```

```markdown
** 案例 **
    #!/bin/bash
    #案例1: "ok"是否等于"ok"
    if [ "ok" = "ok" ]
    then
            echo "相等"
    fi
    #案例2: 23是否大于等于22
    if [ 23 -ge 22 ]
    then
            echo "大于"
    fi
    #案例3: /home/position.sh 目录中文件是否存在
    if [ -f /home/position.sh ]
    then
            echo "存在"
    fi
结果
[相等]
[大于]
[存在]
```



#### 流程控制if

```markdown
** if判断 **
if [条件判断式]  -- 注意两边空格
then 
	代码
fi

或者
if [条件判断式]   -- 注意两边空格
then 
	代码
elif [条件判断式]
then
	代码
fi	

注意: [条件判断式] 两边有空格

** 案例 **
    #!/bin/bash
    if [ $1 -ge 60 ]
    then
            echo "及格了"
    elif [ $1 -lt 60 ]
    then
            echo "不及格"
    fi
```

#### 流程控制case

```markdown
** case语句 **
case $变量名 in
  "值1")
如果变量的值等于值1,则执行程序1
;;
"值2")
如果变量的值等于值2,则执行程序2
;;
...省略其他分支
*)
如果变量的值都不是以上的值,则执行此程序
;;
esac

** 案例 **
当命令行参数是1时,输出"周一",是2时,就输出"周二",其它输出"other".
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
```



#### for循环

```markdown
** 语法 **
for 变量 in 值1 值2 值3...
do
程序/代码
done

** 案例 **
案例1 : 打印命令行输入的参数[可以看出$@和$*区别]
	vim testFor1.sh
	输入
	#!/bin/bash
    for i in "$*"
    do
            echo "num is $i"
    done

    echo "---------------------------"

    for j in $@
    do
            echo "num is $j"
    done
    执行
    sh testFor1.sh 1 2 3 4 5
    结果
[num is 1 2 3 4 5]
[---------------------------]
[num is 1]
[num is 2]
[num is 3]
[num is 4]
[num is 5]   

** 语法2 **
for (( 初始值;循环控制条件;变量变化 ))
do
程序
done

** 案例 **
从1加到100的值输出显示
	vim testFor2.sh
	输入
	#!/bin/bash
    sum=0
    for (( i=1; i <= 100;i++ ))
    do
            sum=$[$sum+$i]
    done
    echo "sum = $sum"
    结果
[sum = 5050]   
    修改
	#!/bin/bash
    sum=0
    for (( i=1; i <= $1; i++ ))
    do
            sum=$[$sum+$i]
    done
    echo "sum = $sum"
	执行
	sh testFor2.sh 50
	结果
[sum = 1275]	
```

#### while循环

```markdown
** 基本语法 **
while [ 条件判断式 ]  (注意:三处空格)
do
程序
done

** 案例 **
从命令行输入一个数n,统计从1+...+ n的值是多少
	vim testWhile.sh
	输入
	#!/bin/bash
    sum=0
    i=0
    while [ $i -le $1 ]
    do
            sum=$[$sum+$i]
            i=$[$i+1]
    done
    echo "执行结果: $sum"
	执行
	sh testWhile.sh 100
	结果
[执行结果: 5050]	
```

#### read读取输入

```markdown
** 基本语法 **
read (选项) (参数)
选项:
 -p : 指定读取值时的提示符
 -t : 指定读取值时等待时间(秒),如果没有在指定的时间内输入,就不再等待了.
参数:
 变量:指定读取值得变量名

** 案例 **
读取控制台输入一个num值.
读取控制台输入一个num值, 在10秒内输入.
	vim testRead.sh
	输入
	#!/bin/bash
    #案例1: 读取控制台输入一个num1值
    read -p "请输入一个数num1=" num1
    echo "num1=$num1"
    #案例2: 读取控制台输入一个num2值,在10秒内输入
    read -t 10 -p "请输入一个数num2=" num2
    echo "num2=$num2"
	执行
	sh testRead.sh
```



#### 函数

```markdown
** 函数 **
shell编程和其它编程语言一样,有系统函数,也可以自定义函数.系统函数中,我们这里就介绍两个.

** 系统函数 **
basename 基本语法
功能: 返回完整路径最后/的部分,常用于获取文件名.
basename [path] [suffix] : 返回文件名,不带路径
选项:
	suffix为后缀,如果suffix被指定,则返回的文件名不带后缀

dirname 基本语法
功能: 返回完整路径最后/的前面部分,常用语返回路径部分
dirname 文件绝对路径  -- 返回路径, 不含文件名

** 案例 **
请返回/home/var.sh 的"var.sh"部分
	basename /home/var.sh  -- 返回 var.sh
	basename /home/var.sh .sh  -- 返回var

请返回/home/song/abc.txt 的 "/home/song" 部分
	dirname /home/song/abc.txt    -- 返回 /home/song
```

#### 自定义函数

```markdown
** 自定义函数 **
[ function ] funname[()]
{
	Action;
	[return int;]
}
直接写函数名调用: funname [值]

** 案例 **
计算两个参数的和,getSum
	vim testFun.sh
	输入
	#!/bin/bash
    #定义函数getSum
    function getSum(){
            sum=$[$n1+$n2]
            echo "和是=$sum"
    }
    #输入连个数
    read -p "请输入一个数n1=" n1
    read -p "请输入一个数n2=" n2
    #调用函数
    getSum $n1 $n2
    执行
    sh testFun.sh
```

#### 定时备份数据库

```markdown
** 需求分析 **
1.每天凌晨2:30备份数据库sqltest到 /data/backup/db
2.备份开始备份结束能够给出相应的提示信息
3.备份后的文件要求以备份时间为文件名,并打包成.tar.gz的形式,比如:2021-03-01_230201.tar.gz
4.在备份的同时,检查是否有10天前备份的数据库文件,如果有就将其删除.
```

```shell
vim mysql_db_backup.sh
编辑文件------------------------------------
#获取当前时间
DATETIME=$(date +%Y-%m-%d_%H%M%S)
echo $DATETIME

#数据库地址,用户名,密码,数据库名
HOST=localhost
DB_USER=root
DB_PW=admin
DATABASE=sqltest

#创建备份目录,如果不存在,就创建
[ ! -d "${BACKUP}/${DATETIME}" ] && mkdir -p "${BACKUP}/${DATETIME}"

#备份数据库
mysqldump -u${DB_USER} -p${DB_PW} --host=${HOST} -q -R --datebases ${DATABASE} | gzip > ${BACKUP}/${DATETIME}/$DATETIME.sql.gz

#将文件处理成 tar.gz
cd ${BACKUP}
tar -zcvf $DATETIME.tar.gz ${DATETIME}
#删除对应的备份目录
rm -rf ${BACKUP}/${DATETIME}

#删除十天前的备份文件
find ${BACKUP} -atime +10 -name "*.tar.gz" -exec rm -rf {} \;
echo "备份数据库${DATABASE} 成功"
编辑结束--------------------------------------------------------------------
添加定时任务
crontab -e
30 2 * * * sh /usr/sbin/mysql_db_backup.sh
```



### 日志

#### 基本介绍

```markdown
1.日志文件是重要的系统系统信息文件,其中记录了许多重要的系统事件,包括用户的登录信息,系统的启动信息,系统的安全信息,邮件信息,各种服务相关信息等.
2.日志对于安全来说也很重要,它记录了系统每年发生的各种事情,通过日志来检查错误发生的原因,或者受到攻击时攻击者留下的痕迹.
3.可以这样理解 日志是用来记录重大事件的工具.

/var/log/ 目录存放系统日志文件
```

```markdown
** 系统常用日志 **
/var/log/boot.log      系统启动日志
/var/log/cron          记录系统定时任务相关的日志
/var/log/cups          记录打印信息的日志
/var/log/dmesg         记录了系统在开机内核自检的信息
/var/log/btmp          错误登录日志.二进制文件,用lastb命令查看
/var/log/lasllog       记录所有用户最后一次登录时间,二进制文件
/var/log/mailog        记录邮件信息的日志
/var/log/message       记录重要消息的日志.比如系统出问题
/var/log/secure        记录验证和授权
/var/log/wtmp          永久记录所有用户的登录,注销信息,同时记录系统的活动,重启,关机事件.二进制
/var/log/ulmp          记录当前已经登录用户的信息.用w,who,users 等命令查看.
```

```markdown
 ** 应用案例 **
 使用root用户通过xshell登录,第一次使用错误的密码,第二次使用准确的密码登录成功,看看再日志文件/var/log/secure 里有没有记录相关信息
 
 清空原secure文件
 	echo '' > secure
```

#### 日志管理服务

```markdown
CentOS7.6日志服务是rsyslogd, CentOS6.x日志服务是syslogd. rsyslogd功能更强大.
rsyslogd的使用,日志文件的格式,和syslogd服务兼容的.

查询Linux中的rsyslogd服务是否启动
ps -aux |  grep "rsyslog" | grep -v "grep"
查询rsylogd服务的自启动状态
systemctl list-unit-files | grep rsylog

rsyslogd 服务在 cat /etc/rsyslog.conf文件中配置
```

```
配置文件: /etc/rsyslog.conf
编辑文件时的格式为: *.*  存放日志文件
其中第一个*代表日志类型,第二个*代表日志级别
1. 日志类型分为:
auth                ##pam产生的日志
authpriv            ##ssh,ftp等登陆信息的验证信息
corn                ##时间任务相关
kern                ##内核
lpr                 ##打印
mail                ##邮件
mark(syslog)-rsylog   ##服务内部的信息,时间标识
news                ##新闻组
user                ##用户程序产生的相关信息
uucp                ##unix to nuix copy主机之间相关的通信
local 1-7           ##自定义的日志设备

2.日志级别
debug               ##有调试信息的,日志通信最多
info                ##一般信息日志,最常用
notice              ##最具有重要性的普通条件的信息
warning             ##警告级别
err                 ##错误级别
crit                ##严重级别
alert               ##需要立即修改的信息
emerg               ##内核崩溃等重要信息
none                ##什么都不记录
注意: 从上到下,级别从低到高,记录信息越来越少.

** 案例 **
	修改日志文件
	vim /etc/rsyslog.conf
	加入
	*.*              /var/log/song.log
	新建song.log
	> /var/log/song.log
	重启系统
	reboot
	查看song.log
	cat song.log | grep sshd
```

#### 日志轮替

```markdown
日志轮替就是把旧的日志文件移动并改名,同时建立新的空日志文件,当旧日志文件超出保存范围之后.
就会进行删除.

日志轮替文件命名
1.CentOS7使用logrotate进行日志轮替管理,要想改变日志轮替文件名字,通过/etc/logrotate.conf配置文件中"dateext"参数:
2.如果配置文件中有"dateext"参数,那么日志会用日期来作为日志文件的后缀,例如"secure-20210501".这样日志文件名不会重叠,也就不需要日志文件的改名,只需要指定保存日志个数,删除多余的日志文件即可.
3.如果配置文件中没有"dateext"参数,日志文件就需要进行改名了.当第一次进行日志轮替时,当前的"secure"日志会自动改名为"secure.1",然后新建"secure"日志,用来保存新的日志.当第二次进行日志轮替时,"secure.1"会自动改名为"secure.2",当前的"secure"日志会自动改名为"secure.1",然后也会新建"secure"日志,用来保存新的日志,一次类推.
```

```markdown
logrotate配置文件
/etc/logrotate.conf为 logrotate的全局配置文件.
#rotate log files weekly. 每周对日志文件进行一次轮替
weekly
#keep 4 weeks worth of backlogs. 共保存4分日志文件,当建立新的日志文件时,旧的将会被删除
rotate 4
#创建新的空日志文件,在日志轮替后
create
#使用日期作为日志轮替文件的后缀
dateext
#日志文件是否压缩.如果取消注释,则日志会在转储的同事进行压缩

include /etc/logrotate.d
#目录中所有的子配置文件.也就是说会把这个目录所有子配置文件读取进来

/etc/logrotate.conf 或 /etc/logrotate.d 可以指定轮替规则

/var/log/wtmp{
	monthly 	# 每月对日志进行一次轮替
    create 0664 root utmp 	# 建立新的日志文件,权限0664,所有者是root,所属组是utmp组
    minsize 1M   # 日志文件最小轮替大小是1MB. 也就是日志一定要超过1MB才会轮替
    rotate 1    # 仅保留一个日志备份.也就是只有wtmp和wtmp.1 日志保留而已
}
/var/log/btmp{
	missingok  # 如果日志不存在,则忽略该日志的警告信息
	monthly
	create 0600 root utmp
	rotate 1
}
```

```markdown
给song.log单独指定轮替规则
方法一:
	直接在/etc/logrotate.conf 配置文件中写入该日志的轮替策略
方法二:
	在/etc/logrotate.d/ 目录中建立该日志的轮替文件,在该轮替文件中写入准确的轮替规则,因为该目录中的文件都会被"include"到主配置文件中,所以也可以把日志加入轮替.

推荐使用第二种方法,利于文件管理
	vim /etc/logrotate.d/songlog
	输入
	/var/log/song.log
    {
        missingok
        daily
        copytruncate
        rotate 7
        notifempty
    }
```

```markdown
** 日志轮替机制原理 **
日志轮替之所以可以再指定时间备份,是依赖系统定时任务. 在/etc/crom.daily/ 目录,就会发现有logrotate文件(可执行),logrotate通过这个文件依赖定时任务执行的.
```

#### 内存日志

```markdown
 journalctl 可以查看内存日志
 
 journalctl        ## 查看全部
 journalctl -n 3   ## 查看最新3条
 journalctl --since 19:00 --until 19:10:10  ## 查看起始时间到结束时间的日志可加日期
 journalctl -p err  ## 报错日志
 journalctl -o verbose  ## 日志详细内容
 journalctl -PID=1235 _COMM=sshd  ## 查看包含这些参数的日志 (在详细日志查看)
 
 或者journalctl | grep sshd
 
 注意: journalctl 查看的是内存日志,重启清空
 
 案例
 使用journalctl | grep sshd 来看看用户登录清空, 重启系统,再次查询,看看日志有什么变化没有.
```



### 备份和恢复

```markdown
备份和恢复两种方式
1.把需要的文件(或分区)用TAR打包就行,下次需要恢复的时候,再解压开覆盖即可
2.使用dump和restore命令

安装dump和restore
yum -y install dump
yum -y install restore
```

#### dump备份

```markdown
dump支持分卷和增量备份(所谓增量备份是指备份上次备份后, 修改/增加过的文件,也称差异备份).

dump [-cu] [-123456789] [-f <备份文件名>] [-T <日期>] [目录或文件系统]
dump [] -wW
-c : 创建新的归档文件,并将由一个或多个文件参数所指定的内容写入归档文件的开头.
-0123456789 : 备份的层级,0位最完整备份,会备份所有文件.若指定0以上的层级,则备份至上一次备份以来修改或新增的文件,到9后,可以再次轮替.
-f <备份后文件名> : 指定备份后文件名
-j : 调用bzlib 库压缩备份文件, 也就是将备份后的文件压缩成bz2格式,让文件更小
-T <日期> : 指定开始备份的时间和日期
-u : 备份完毕后,在/etc/dumpdares中记录备份的文件系统,层级,日期与时间等.
-t : 指定文件名, 若该文件已存在备份文件中,则列出名称
-W : 显示需要备份的文件及其最后一次备份的层级,时间,日期
-w : 与-W类似,但仅显示需要备份的文件.
```

```markdown
将/boot 分区所有内容备份到/opt/boot.bak0.bz2 文件中,备份层级为"0"
dump -0uj -f /opt/boot.bak0.bz2 /boot
```

#### restore恢复

```markdown
restore命令用来恢复已备份的文件,可以从dump生成的备份文件中恢复原文件

restore [模式选项] [选项]
下面四个模式,不能混用,在一次命令中,只能指定一种.
-C : 使用对比模式,将备份的文件与已存在的文件相互对比.
-i : 使用交互模式,在进行还原操作时,restore指令将依序询问用户
-r : 进行还原模式
-t : 查看模式,看备份文件有哪些文件

-f <备份设备> : 从指定的文件中读取备份数据, 进行还原操作

restore -C -f boot.bak1.bz2 
```



### Webmin安装



### bt宝塔

```
安装: yum install -y wget && wget -O install.sh http://download.bt.cn/install/install_6.0.sh && sh install.sh

bt default -- 显示安装信息, 网址, 用户名 密码等


```



### 面试题

```markdown
http://192.168.200.10/index1.html
http://192.168.200.10/index2.html
http://192.168.200.20/index1.html
http://192.168.200.30/index1.html
http://192.168.200.40/index1.html
http://192.168.200.30/order.html
http://192.168.200.10/order.html
-- 取ip并统计排序
cat t.txt | cut -d '/' -f 3 | sort | uniq -c | sort -nr

     3 192.168.200.10
     2 192.168.200.30
     1 192.168.200.40
     1 192.168.200.20

```



### 其他命令

```markdown
rz : 上传文件
```



### 常用命令

```shell 
ssh 192.168.150.131   -- 远程登录
pwd                   -- 查看目录位置
ifconfig              -- 查看网络信息
free -h               -- 查看内存信息
df -lh                -- 磁盘信息
du -h --max-depth=1   -- 查看当前目录的占用空间信息
uname -a              -- 查看系统版本

which  'java'         -- 查看安装位置
find / -name hello.txt  -- 查找文件/目录位置

jobs                  -- 查询后台任务
ps -ef| grep java     -- 查询java进程
netstat -ntlp         -- 查看占用端口
top                   -- 查看资源占用情况
cul                   -- 访问相应端口

sz /home/hello.txt    -- 下载文件到本地
rz                    -- 上传文件到Linux
```





