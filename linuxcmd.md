# <center>Linux常用命令总结<center> #

<center>
**系统一切从根开始**  
**系统中数据一切皆文件**  
**命令操作完没有任何消息信息, 就是最好的消息**
</center>

  

## 一、关机命令(这个还真没怎用过)
### shutdown
推荐使用的安全的将系统关机的命令。
	
	[-H] 关闭机器
	[-r] 重启机器
	[-P] Power-off关闭电源
	[-h] 和-P一样，除非指定-H
	[-k] 不会关机，只是发送警告命令
	[-c] 取消正在执行的关机程序
	shutdown -r now	//立刻重启
	shutdown -h 10	//10分钟后关机
	
## 二、查看/更改命令别名
### alias

	[gjk@gjk projects]$ alias vv="echo Wo shi bieming"
	[gjk@gjk projects]$ vv
	Wo shi bieming
	[gjk@gjk projects]$ unalias vv
	[gjk@gjk projects]$ vv
	-bash: vv: command not found

## 三、创建文件或更新文件时间戳
### touch


## 四、查看文件内容命令
### 1、cat
从上到下显示整个文件。主要有有四大功能
#### 1）一次显示整个文件
	cat filename
#### 2）从键盘创建一个文件
	cat > filename
输入这个命令后命令行开始接收用户输入，输入的内容将保存到filename这个文件中。  
文件**不存在**则创建，存在则新清空源文件。
#### 3）将几个文件合并为一个
	cat file1 file2 > new file

#### cat的参数
	-n 输出的内容增加行号
	-b 和-n类似，但是空白行不编号
	-s 当遇到有连续两行空白行时，替换为一个空白行
	-v 显示非可见字符

### 2、more
一页一页的显示，方便读者阅读。最基本的指令是按**空格**显示下一页，**回车**下一行，**q**退出。  
**`more`在显示之前会加载整个文件**

	+n 从第n行开始显示
	-n 定义屏幕大小为n行
	-c 从顶部清屏开始显示
	-d 增加提示语，禁用响铃
	-l 忽略换页字符
常用的操纵
	
	Ctrl+F 向下翻一页，同空格
	Ctrl+B 向上翻一页
	= 显示当前行号
	:f 显示当前文件名和行号
	v 调用vi编辑
	! 调用shell命令

### 3、less
`less`很强大。`less`和`more`非常相似，但less在显示之前不会加载整个文件。  
常用参数
		
	-b <缓冲区大小> 设置缓冲区大小
	-e 当文件显示结束后自动离开
	-f 强制打开特殊文件，例如二进制文件、目录等
	-m 显示类似more命令的百分比
	-N 显示行号
	-o <filename> 将less输出的内容保存到filename中
	
命令
	
	/str	向下搜索str
	?str	向上搜索str
	n		重复前一个搜索
	N		反向重复前一个搜索
	b		向后翻一页
	d		向后翻半页
	h		帮助
	
### 4、head/tail
显示文件开始/结尾的内容

	-n <num> 显示开头/结尾的num行
### 5、od
od命令默认以八进制显示文件内容，用来查看二进制文件再合适不过了。  
例子:

1、以八进制显示内容 

	od dump.rdb
2、以十六进制显示内容
	
	od -x dump.rdb
3、显示ASCII码
	
	od -c dump.rdb
4、同时显示ASCII码和十六进制

	od -cx dump.rdb


## 五、文本处理工具
主要有文本处理的grep、awk、sed三大命令。  
grep主要用于文本查找，awk主要用于文本分析，sed主要用于文本编辑。另外还有一个sed的简化版本tr。
### 1、tr
tr命令是translate的缩写。tr可以看作是sed的一个及其简化的版本。  
	
	tr [options] set1 [set2]
常用格式:

	tr [-c -d -s] ["string1_to_translate_from"] ["string2_to_translate_to"] < input-file
常用选项：

	-c	使用指定字符集的补集
	-d	删除指定字符集中包含的内容
	-s	字符集中包含的内容连续出现时都只保留第一个

字符集可以使用的范围，必须是ASCII码，所有可用格式可查看man手册或者--help。

	[A-Z]/[a-z]	指定范围内的大/小写字符
	[0-9]		指定范围内的数字
	\r			回车
	\n			新行
	\b			退格
	[C*n]		字符C出现n次
常用例子：  
1、将字符abc替换为xyz
	
	cat a | tr "abc" "xyz"
2、转换为大写
	
	cat a | tr [a-z] [A-Z]
	cat a | tr [:lower:] [:upper:]
3、删除换行
	
	cat a | tr -d "\n"
4、删除windows造成的^M
	
	cat a | tr "\r" "\n"

### 2、grep
Linux系统中grep命令是一种强大的文本搜索工具，它能使用正则表达式搜索文本，并把匹 配的行打印出来。grep全称是`Global Regular Expression Print`，表示全局正则表达式版本，它的使用权限是所有用户。  
grep家族包括grep、egrep、fgrep。egrep和fgrep都是grep的扩展，这里只说grep。  
grep命令格式:

	grep [OPTIONS] PATTERN [FILE...]
    grep [OPTIONS] [-e PATTERN | -f FILE] [FILE...]
常用选项

	-i		不区分大小写
	-n		显示行号
	-c		只显示检索到的行的数目
	-l		只显示匹配到的文件名
	-w		精确匹配
	-A		打印出后面的行
	-B		打印出前面的行
	-C		打印前后的行

例子：  
1、查找test.txt中有test的行，-i表示不区分大小写，-n显示行号

	grep -i -n 'test' test.txt
2、不仅打印出匹配行，而且打印出该行后面的2行
	
	grep -A2 'test' test.txt

3、找出能精确匹配test的行

	grep -w 'test' test.txt

4、找到不包含test的行

	grep -v 'test' test.txt

5、包含abc或者包含test的行

	grep -e 'abc' -e 'test' test.txt
再详细一些的就查看man手册。

### 3、awk
awk把文件逐行读入，以空格为默认分隔符将每行切片，切开的部分再进行分析处理。

	awk '{pattern action}' {filename}
pattern表示要查找匹配的内容，正则表达式，用双斜杠括起来。  
选项：  
	
	-F		指定分隔符

内置变量：  

	ARGC            命令行参数个数
	ARGV            命令行参数排列
	ENVIRON         支持队列中系统环境变量的使用
	FILENAME		awk命令浏览的文件名
	FNR             浏览文件的记录数
	FS              设置输入域分隔符，等价于命令行 -F选项
	NF				浏览记录的域的个数
	NR				已读的记录数
	OFS             输出域分隔符
	ORS             输出记录分隔符
	RS              控制记录分隔符
	$0表示整个行内容，$1表示分割后第一个域的内容，$2表示第二个依次类推，$NF表示最后一个
例子：  
1、搜索/etc/passwd有root关键字的行
	
	awk '/root/' /etc/passwd
2、搜索/etc/passwd有root关键字的行，显示对应的shell字段
	
	awk -F: '/root/ {print $7}' /etc/passwd
3、对/etc/passwd进行统计，输出文件名，行号，列数，行内容。

	awk -F: '{print "filename:" FILENAME ", linenum:" NR ", columns:" NF ", content:" $0}' /etc/passwd
	awk -F: '{printf("filename:%s, linenum:%03d, columns:%2d, content:%s\n", FILENAME, NR, NF, $0)}' /etc/passwd
4、打印/etc/passwd第二行信息

	awk 'NR==2 {printf("filename: %s, %s\n",FILENAME, $0)}' /etc/passwd
5、输出ll命令的每行的域个数

	ll | awk '{print NF}'
6、过滤输出ll命令输出中包含指定字符 `gjk` 的行

	ll | awk '/gjk/'
7、查询/etc/passwd的每行的第一列和最后一列

	awk -F: '{print $1 ", " $NF}' /etc/passwd
8、查询输出/etc/passwd的第2-4行的第一列

	awk -F: '{if(NR>=2 && NR<=4) print $1}' /etc/passwd
9、添加BEGIN和END增加开头行和结尾行
	
	awk -F: 'BEGIN{print "====begin=====\nname, shell"} {print $1 ", " $NF} END{print "====over===="}' /etc/passwd
10、查看最近登录的用户名和IP

	last | awk '{print $1,$3}'

另外awk还可以编程，这里先不写了。

### 4、sed
`sed`命令采用的是流编辑模式，`sed`处理数据之前，需要预先提供一组规则，`sed`会按照此规则来编辑数据。  
`sed`根据脚本命令来处理文本文件中的数据，这些命令要么从命令行中输入，要么存储在一个文本文件中。  
`sed`执行顺序：  

1. 每次仅读取一行内容。
2. 根据提供的规则命令匹配并修改数据。注意，`sed`默认不会直接修改源文件数据，而是会将数据复制到缓冲区中，修改也仅限于缓冲区中的数据。
3. 将执行结果输出。

当一行数据匹配完成后，它会继续读取下一行数据，并重复这个过程，直到将文件中所有数据处理完毕。  
`sed`命令格式如下：

	sed [选项] [脚本命令] 文件名
选项：

	-e		将其后跟的脚本命令添加到已有命令中
	-f		将其后跟的文件中的脚本命令添加到已有命令中
	-n		屏蔽启动输出
	-i		直接修改源文件，慎用
1、sed s 替换命令
	
	sed [address]s/pattern/replacement/flags
address 表示指定要操作的具体行，pattern 指的是需要替换的内容，replacement 指的是要替换的新内容, flags是一些功能代号。  
常用的flags：

	n		1~512 之间的数字，指定要替换的字符串出现第几次时才进行替换
	g		对所有匹配到的内容进行替换，默认只替换第一个出现的
	p		打印与替换命令中指定的模式匹配的行，常与-n选项一起使用
	w file	将缓冲区中的内容写到file文件中
	&		使用正则表达式匹配的内容进行替换
	\n		匹配第 n 个子串，该子串之前在 pattern 中用 \(\) 指定。
	\		转义
例子：  
将test.txt包含abc的行中，abc第2次出现时替换为gjk

	sed 's/abc/gjk/2' test.txt
替换所有的abc为gjk

	sed 's/abc/gjk/g' test.txt
只输出被替换了的行

	sed -n 's/abc/gjk/p' test.txt
将替换的内容输出到文件中
	
	sed -n 's/abc/gjk/w data.txt' test.txt
将/bin/bash替换为/bin/csh
	
	sed -n 's/\/bin\/bash/\/bin\/csh/p' /etc/passwd
2、sed d替换命令

	sed [address]d
删除文件中的指定行，如果没有指定行的话，会删掉所有内容，小心使用。  
例子：  
删除指定文件中的2、3行。

	sed '2,3d' data6.txt
删除第2行到最后一行

	sed '2,$d' data6.txt
3、sed a和i命令
a表示在指定行后面插入一行，i表示在指定行前面插入一行。

	sed [address]a或者i\新内容
例子：
在第二行后面插入一行 "New Line"

	sed '2a\New Line' data6.txt
插入多行

	[gjk@ecs-sn3-medium-2-linux-20191117153425 ~]$ sed '2a\
	> New Line1\
	> New Line2\' data6.txt

更多的内容去查看man手册或者[教程](http://c.biancheng.net/view/4028.html)

## 六、文件查找工具
这里介绍find、locate、whereis、which和type几个命令。
### 1、find
find是最常用和最强大的工具，可以查找任何类型的文件。  

	find <指定目录> <指定条件> <指定动作>
	find pathname -options [-print -exec -ok]
默认在当前目录下查找，默认执行-print，不加任何过滤。  
选项：

	-name		按名字查找
	-user		按所属者查找
	-group		按所属组查找
	-perm		按文件权限来查找
	-prune		不在当前指定目录中查找
	-mtime -n+n	按照修改时间来找，-n表示n天以内，+n表示n天以前
	-type		按文件类型找(b：块设备文件；d：目录文件；c：字符设备文件；p：管道文件；l：链接文件；f：普通文件)

例子：  
1、在当前目录及其子目录下查找

	find -name "passwd"
	find . -name "passwd"

2、在/目录下查找
	
	find / -name "passwd"
### 2、locate
这个命令在centos7不是自带的，需要先安装一下`yum install mlocate`，安装好之后，要先updatedb一下才能用。  
locate其实是find -name的另一种写法，但是查找方式跟find不同，比find快的多。它不搜索具体目录，而是在一个数据库(`/var/lib/mlocate/mlocate.db`)中去查找，这个数据库由updatedb命令来更新，是由crontab定期更新的，所以找不到文件时可以先更新下数据库。

	locate	mlocate.db
这个命令感觉和windows上的everything很像了。
### 3、whereis
whereis命令只能用于搜索二进制文件(-b)、源代码文件(-s)、说明文件(-m)。如果省略参数则返回所有的信息。

	whereis crontab
	whereis -b crontab
	whereis -s crontab
	whereis -m crontab
不指定参数则都显示
### 4、which
which命令是在PATH变量指定的路径中搜索指定的系统命令的位置。  

	which crontab
### 5、type
type命令主要用于区分一个命令到底是shell自带的还是外部独立的二进制文件提供的。如果是shell自带的则会提示此命令为shell buildin,否则会列出命令的位置。
	
	[root@centos7 ~]# type cd
	cd is a shell builtin
	
	[root@centos7 ~]# type redis-server
	redis-server is /usr/local/bin/redis/redis-server
	
	[root@centos7 ~]# type cp
	cp is aliased to `cp -i'

## 七、进程管理工具
这里介绍ps、dstat、top、htop四个命令
### 1、ps
ps命令用于显示当前的进程状态

	ps [options]
选项有三种风格：

1. UNIX风格，必须在选项前面加“-”
2. BSD风格，选项前不能加“-”
3. GNU风格，选项前为两个“-”

常用格式：  
1、ps -aux

	a		所有与终端相关的进程
	x		所有与终端无关的进程
	u		以用户为中心组织进程状态信息显示
其中stat状态字段的含义：

	R：running 运行
	S：interruptable sleeping 可中断睡眠
	D：uninterruptable sleeping 不可中断睡眠
	T：Stopped 停止
	Z：zombie 僵死态
	+：前台进程
	l：多线程进程
	N：低优先级进程
	<：高优先级进程
	s：session leader  进程领导者

2、ps -ef

	e		显示所有进程
	f		显示完整格式的进程信息
可以看到当前进程和父进程的pid

3、ps -eFH

	F		显示完整格式的进程信息
	H		
PSR字段表示运行于哪个CPU上

4、ps -eo field1,field2... **OR**  ps -axo field1,field2...  
可以指定要显示的字段，常用的字段如下：
	
	user：用户
	pid：进程的pid号
	ni：nice值
	priority：优先级
	psr：运行在那颗cpu
	pcpu：cpu利用率
	ppid：父进程的id号
	rtprio：实时优先级
	stat：运行状态
	comm：命令行
	tty：终端

### 2、dstat
多功能资源统计工具。dstat可以查看所有的实时系统资源，全面、灵活、优雅、格式清晰，非常强大。  
需要安装：

	yum install dstat
这里只列举几种用法，其实只是`dstat`不带选项的命令就有很多东西了，带上选项内容更细致。

1、查看内存使用情况

	dstat -glms --top-mem  


2、查看CPU使用情况

	dstat -cyl --proc-count --top-cpu  


3、将实时信息输出到csv文件里

	dstat --output ~/test.csv  


这个命令只看了一个皮毛，感觉深入研究一下会很好玩

### 3、top
动态显示进程状态。很常用的一个命令了。  

top内功能：

	l：是否显示uptime命令信息，首部第一行
	t：切换task和cpu信息显示，首部第二三行
	m：切换内存显示，首部第四五行
	
	P：以占据CPU百分比排序
	M：以占据内存百分比排序
	T：累积占用CPU时间排序
	
	
	
	s：修改刷新间隔，默认3秒
	k：杀进程

	q：退出
	Esc：取消输入命令
运行选项：

	-d		指定刷新间隔
	-b		以批次方式显示
	-n		显示多少批次

### 4、htop
和top类似，比top要优雅强大许多，提供非常多的操作方式。  
这里只简单说几个选项，具体的使用方式还需要继续探索。  
选项：

	-d		指定延迟时间间隔
	-u		仅显示指定用户的进程
	-s		以指定字段进行排序
常用子命令：

	l：显示选定的进程打开的文件列表
	s：跟踪选定的进程的系统调用
	t：以层级关系显示各进程状态
	a：将选定的进程绑定至某指定的CPU核心
PS:在这个命令界面，按上下按钮可以切换选中的进程。

## 八、文件压缩命令
Linux中的压缩格式主要有以下几种：**.zip**、**.gz**、**.bz2**、**.tar.gz**、**.tar.bz2**。  

1. linux中后缀名不重要，但是正确的后缀名可以表现出压缩格式，帮助选择正确的解压缩命令。
2. 解压缩的方法和压缩的方法不一致的话，基本上都会出问题的。
### 1、zip
后缀名为.zip，zip命令不是自带的，需要安装 `yum install zip`。
格式如下：

	zip <压缩文件名> <源文件>
	zip -r <压缩文件名> <源目录>
	unzip <压缩文件名>
例如：

	zip data data1.txt data6.txt
	unzip data.zip
### 2、gzip
后缀名为.gz，不能打包目录。

	gzip <源文件>			#源文件会消失
	gzip -r <目录>			#压缩目录下的文件，但是不会压缩目录
	gunzip	<压缩文件名>		#压缩包会消失

例子：
	gzip test.txt
	gunzip test.txt.gz
### 3、bzip2
后缀名为bz2，不能打包目录

	bzip2 <源文件>			#源文件会消失
	bzip2 -k <源文件>		#保留源文件
	bzip2 -d <压缩文件>		#解压缩
	bunzip2 <压缩文件>		#压缩包会消失
例子：

	bzip2 -k test.txt
	bzip2 -kd test.txt.bz2
### 4、tar
gz和bz2都不能压缩目录，而**tar**通过先把目录中的文件打包成一个文件，然后再用gz后者bz2进行压缩。得到**.tar.gz** 或者 **.tar.bz2**来解决问题。这两种格式的压缩包是linux最常见的形式。  
打包命令：

	tar	-cvf <打包文件名> <源文件>
	tar -xvf <打包文件名>
	tar -tvf <打包文件名>
	-c		打包
	-x		解包
	-t		不解包，直接查看包内容
	-v		显示过程
	-f		指定打包文件名
例如：

	tar -cvf data.tar data*
	tar -xvf data.tar
**.tar.gz**是在tar包的基础上进行gzip压缩，可以直接执行一个命令完成两个动作：

	tar -zcvf	<打包文件名> <源文件>
	tar -zxvf 	<打包文件名>
	tar -ztvf 	<打包文件名>
例如：
	
	tar -zcvf data data*
	tar -zxvf data.tar.gz
**.tar.bz2**是在tar包的基础上进行bzip2压缩，可以直接执行一个命令完成两个动作：

	tar -jcvf	<打包文件名> <源文件>
	tar -jxvf 	<打包文件名>
	tar -jtvf 	<打包文件名>
例如：
	
	tar -jcvf data.tar.bz2 data1.txt data6.txt
	tar -jxvf data.tar.bz2
## 九、网络相关命令
### 1、hostname

	hostname 			没有选项，显示主机名字
	hostname new_name	设置临时主机名
	hostname –d 		显示机器所属域名
	hostname –f 		显示完整的主机名和域名
	hostname –i 		显示当前机器的ip地址（这个IP应该是内网IP）
### 2、ifconfig
查看用户网络配置，可以查看到所有网卡的信息。
### 3、nslookup
DNS查询，这个命令不是自带的，需要安装`yum install bind-utils`。
	
	nslookup domain-name		#解析域名
	nslookup ip					#通过ip查找主机名
### 4、traceroute
查看数据包在提交到远程系统或者网站时候所经过的路由器的 IP 地址、跳数和响应时间。  
需要安装`yum install traceroute`

	traceroute <hostname/ip>
### 5、finger
这个可以查看一些用户的信息，需要安装`yum install finger`。比`who -a`高级一点，并且可以查看远程机器的用户信息。

1、 查看当前用户列表

	finger [-l]
	
2、查看具体用户信息

	finger [-m] user
3、查看远程用户信息

	finger -m user@ip
### 6、telnet
通过 telnet 协议连接目标主机，如果 telnet 连接可以在任一端口上完成即代表着两台主机间的连接良好。通常用来测试主机是否在线或者网络是否正常。需要安装 `yum install telnet`。

	telnet hostname port
### 7、ethtool
查看和更改网卡的许多设置（不包括 Wi-Fi 网卡）。你可以管理许多高级设置，包括 tx/rx、校验及网络唤醒功能。

	ethtool [-idpS] ethI
	-i		#查看驱动信息
	-d		#查看注册信息
	-S		#查看收发包统计
	-p		让网卡亮灯
另外还有许多可以对网卡进行的设置，需要时再搜吧
### 8、ifup/ifdown
重启/关闭网络设备

	ifup eth0
	ifdown lo
### 9、route
### 10、netstat
Netstat是控制台命令,是一个监控TCP/IP网络的非常有用的工具，它可以显示路由表、实际的网络连接以及每一个网络接口设备的状态信息。Netstat用于显示与IP、TCP、UDP和ICMP协议相关的统计数据，一般用于检验本机各端口的网络连接情况。

常用选项：
	
	-a ：显示所有选项，默认是不显示LISTEN相关的
	-t : 指明显示TCP端口
	-u : 指明显示UDP端口
	-l : 仅显示监听套接字(所谓套接字就是使应用程序能够读写与收发通讯协议(protocol)与资料的程序)
	-p : 显示进程标识符和程序名称，每一个套接字/端口都属于一个程序。
	-n : 不进行DNS轮询，显示IP(可以加速操作)
例子：  
1、查看tcp端口

	netstat	-ntlp
2、查看udp端口

	netstat	-nulp

### 11、nc
需要安装 `yum install nc`，nc命令是netcat的缩写。  
常用选项：
	
	-v		显示出一些连接的日志/细节
	-p		创建tcp客户端连接的端口
	-l		创建tcp服务器监听的端口
例子：

	nc -v -l localhost 9981			
在本地开启一个tcp服务器，监听9981端口

	nc -v www.baidu.com 80
在本地开启一个tcp客户端，连接到baidu的80端口




### 12、ss
ss是Socket Statistics的缩写，用来获取socket统计信息。它可以显示和netstat类似的内容。ss的优势在于它能够显示更多更详细的有关TCP和连接状态的信息，而且比netstat更快速更高效。
格式：

	ss [options] [filter]
常用选项：
	
	-t:	tcp
	-u:	udp
  	-a:	all
	-o: 显示时间信息
   	-e: 显示socket详细信息
  	-l: listening      ss -l列出所有打开的网络连接端口
  	-s: summary        显示 Sockets 摘要】
	-p: progress
	-n: numeric        不解析服务名称】
	-r: resolve        解析服务名称】
	-m: memory        	显示内存情况】
例子：

1、显示所有tcp连接

	ss -ta
2、显示所有监听tcp端口的进程信息

	ss -tpl
3、显示所有udp连接

	ss -ua
4、显示所有状态为`established`的tcp连接，-o显示tcp连接的时间信息

	ss -to state 'established'
5、匹配本地/远程地址

	ss dst/src ip[:port]


## 杂：
### 1、nm
nm是names的缩写，目标文件中的符号清单，主要是一些全局变量和函数。对文本文件无效。<nm>
nm显示的内容包括三个部分

1. 在可执行程序中的text session的偏移量
2. 类型大写表示全局，小写表示local
3. 符号的名字

具体类型的意义见：[类型含义](https://www.cnblogs.com/LiuYanYGZ/p/5536607.html)

### 2、objdump
objdump命令是Linux下的反汇编目标文件或者可执行文件的命令，它以一种可阅读的格式让你更多地了解二进制文件可能带有的附加信息。

选项：


	-a 	显示档案库的成员信息,类似ls -l将lib*.a的信息列出
	-b 	指定目标码格式。这不是必须的，objdump能自动识别许多格式
	-C  将底层的符号名解码成用户级名字，除了去掉所开头的下划线之外，还使得C++函数名以可理解的方式显示出来。
	-g 	显示调试信息。企图解析保存在文件中的调试信息并以C语言的语法显示出来。仅仅支持某些类型的调试信息。有些其他的格式被readelf -w支持
	-e	类似-g选项，但是生成的信息是和ctags工具相兼容的格式 
	-d 	从objfile中反汇编那些特定指令机器码的section
	-D 	与 -d 类似，但反汇编所有section.
	-EB 
	-EL --endian={big|little}指定大小端
	-f 	显示objfile中每个文件的整体头部摘要信息。
	-h 	显示目标文件各个section的头部摘要信息。
	-i 	显示对于 -b 或者 -m 选项可用的架构和目标格式列表。
	-j name 仅仅显示指定名称为name的section的信息     
	-S 	尽可能反汇编出源代码，尤其当编译的时候指定了-g这种调试参数时，效果比较明显。隐含了-d参数。
例子：

1、反汇编

	objdump -d main
2、显示文件头部信息

	objdump -f main
3、显示指定section段的信息

	objdump -s -j .init hello		指定.init段