### 目录
* <a href="#conf_file" target="_self">配置文件</a>
* <a href="#user" target="_self">系统管理员对用户和用户组管理文件</a>
* <a href="#file_dir" target="_self">文件目录操作</a>
* <a href="#file_right" target="_self">文件权限设置</a>
* <a href="#file_find" target="_self">文件查找</a>
* <a href="#file_tar" target="_self">文件与文件系统的压缩与打包</a>
* <a href="#file_and_disk" target="_self">文件系统和磁盘</a>
* 性能监控与优化

# <a id="conf_file" href="#">配置文件</a>
* .bashrc/.zshrc
保存个人的一些个性化设置，如命令别名、路径等。一般会在.bash_profile文件中显式调用.bashrc。登陆linux启动bash时首先会去读取~/.bash_profile文件，这样~/.bashrc也就得到执行了，你的个性化设置也就生效了

# <a id="user" href="#">系统管理员对用户和用户组管理文件</a>
* /etc/passwd
* /etc/shadow
* /etc/group 
* /etc/gshadow

# <a id="file_dir" href="#">文件目录操作</a>
***
### cat
* 三大主要功能
  * 1.一次显示整个文件:cat filename
  * 2.从键盘创建一个文件:cat > filename 只能创建新文件,不能编辑已有文件.
  * 3.将几个文件合并为一个文件:cat file1 file2 > file
* 参数
  * -b, --number-nonblank    对非空输出行编号
  * -n, --number     对输出的所有行编号,由1开始对所有输出的行数编号
	
    	```
    	cat -n log2012.log > log2013.log 
    	```
* tac log.txt 反向列示

### head/tail
* 实例
  * head -n 5 log2014.log 显示文件的前n行
  * head -c 20 log2014.log 显示文件前n个字节
  * head -c -32 log2014.log 文件的除了最后n个字节以外的内容
  * head -n -6 log2014.log 输出文件除了最后n行的全部内容

### more / less
* 实例
  * more +3 log2012.log 显示文件中从第3行起的内容
  * more +/day3 log2012.log 从文件中查找第一个出现"day3"字符串的行，并从该处前两行开始显示输出 
  * more -5 log2012.log 设定每屏显示行数
  * ls -l  | more -5

### mv
* 实例
  * -f ：force 强制的意思，如果目标文件已经存在，不会询问而直接覆盖；
  * -t  ： --target-directory=DIRECTORY move all SOURCE arguments into DIRECTORY，即指定mv的目标目录，该选项适用于移动多个源文件到一个目录的情况，此时目标目录在前，源文件在后。

### nl
* nl命令在linux系统中用来计算文件中行号。nl 可以将输出的文件内容自动的加上行号！其默认的结果与 cat -n 有点不太一样， nl 可以将行号做比较多的显示设计，包括位数与是否自动补齐 0 等等的功能

### rm
* 实例：自定义回收站
 打开.bashrc或者.zshrc文件，在后边添加如下命令：  
	
 ```
 myrm(){ D=/tmp/$(date +%Y%m%d%H%M%S); mkdir -p $D; 	mv "$@" $D && echo "moved to $D ok"; }
 alias myrm='myrm'
 ```
source .zshrc进行生效，就可以使用myrm命令了

### touch
* Linux中每个文件的三个时间
    * modification time(mtime): 文件内容数据改变的时间
    * status time(stime): 比如权限或属性被改变的时间
    * access time(atime): 该文件被取用的时间
* 参数
    * -r 　把指定文档或目录的日期时间，统统设成和参考文档或目录的日期时间相同
	
    	```
    	touch -r log.log log2012.log
    	```
    * -d 类似-t，但是时间格式更加自由
        
        ```
        touch -d "2 days ago abc.txt"
        ```
    * -t 　使用指定的日期时间，而非现在的时间,time的格式[[CC]YY]MMDDhhmm[.SS]

    	```
    	touch -t 201211142234.50 log.log 
    	```

### od
* 查阅非文本文件，例如二进制文件.
* 参数
  * -t 后面可添加各种类型type的输出，如
        * a: 默认字符
        * c: ascii字符
        * d[size]: 十进制(decimal), 每个整数占用size bytes
        * f[size]: 浮点数(floating)
        * o[size]: 八进制(ocatal)
        * x[size]: 十六进制(hexadeciaml)
* 实例  
	* 执行`od -t oCc test.o`
![1](https://cloud.githubusercontent.com/assets/16068384/20956807/344dbad8-bc87-11e6-8752-cbadc066cc6d.png)

### 查看文件类型: file
* 如果知道某个文件的基本文件类型，例如属于ASCII或者data或者binary，以及文件中使用的动态函数库信息等.
* 实例
    
    ```
    ➜  c++ file test.cpp 
    test.cpp: C source, ASCII text
    ➜  c++ file test
    test: ELF 64-bit LSB  executable, x86-64, version 1 (SYSV), dynamically linked (uses shared libs), for GNU/Linux 2.6.24, BuildID[sha1]=88744b1fe53bb47b28cb75b83d7572ddecb7d6e7, not stripped
    ```

# <a id="file_right" href="#">文件权限设置</a>
### chgrp
* 使用chgrp指令去变更文件与目录所属群组
* 群组和群组识别码可以去/etc/group文件中查看
* 参数
  * -v 运行时显示详细的处理信息
  * --dereference 作用于符号链接的指向，而不是符号链接本身
* 实例	

	```
	改变文件的群组属性 
	chgrp -v bin log2012.log
	根据指定文件改变文件的群组属性 
	chgrp --reference=log2012.log log2013.log
	改变指定目录以及其子目录下的所有文件的群组属性 
	chgrp -R bin test6
	通过群组识别码改变文件群组属性
	chgrp -R 100 test6
	```

### chown命令
* 通过chown改变文件的拥有者和群组。在更改文件的所有者或所属群组时，可以使用用户名称和用户识别码设置。普通用户不能将自己的文件改变成其他的拥有者。其操作权限一般为管理员。
* 参数
  * -R 处理指定目录以及其子目录下的所有文件
  * -v 显示详细的处理信息
* 实例
 
	```
	改变拥有者和群组
	chown mail:mail log2012.log 
	改变文件拥有者和群组
	chown root: log2012.log
	改变文件群组
	chown :mail log2012.log
	改变指定目录以及其子目录下的所有文件的拥有者和群组 
	chown -R -v root:mail test6
	```

### 文件默认权限: umask
* 指定用户在新建文件或目录时候的默认权限值(反码)
* 默认情况下(不设置umask的时候)，新建文件的权限是`-rw-rw-rw-`，新建目录的权限是`drwxrwxrwx`.
* 设置umask之后，新建文件的权限是默认权限减去umask的值
* 设置umask `umask xxx`

### 文件隐藏属性: chattr,lsattr
* chattr [+-=] [ASacdistu] 文件或目录名称
* chattr参数
    * +: 增加某个参数,类似的有-/=
    * a: 设置后，改文件只能增加数据，不能删除和修改数据
    * i: 设置后，使文件不能被删除、改名、设置链接，也无法写入或添加数据.
* lsatttr [-adR] 文件或目录名称

### 文件特殊权限: SUID,SGID,SBIT
* Set UID
    * s权限放在所有者的x权限的位置上，例如`/usr/bin/passwd` 
    * 功能
        * SUID权限仅对二进制程序有效
        * 执行者对于改程序具有x的权限
        * 本权限仅在执行改程序的过程中有效
        * 执行者将具有改程序所有者的权限
    * 比如使用passwd可以修改本用户密码(实际是修改/etc/shadow)，但是通过cat命令是无法查看/etc/shadow的，原因是/usr/bin/passwd具有s权限
    * SUID对目录无效
* Set GID
    * s权限放在用户组的x权限的位置上，例如`/usr/bin/locate`
    * SGID对目录和文件都有效
* Sticky Bit
    * t权限放在其他用户组的x权限的位置上，例如`/tmp`
    * SBIT只堆目录有效，t
* 特殊权限设置
    * 加一个数字在普通权限之前
    * SUID--4
    * SGID--2
    * SBIT--1
* S,T表示空的SUID/SGID/SBIT权限，即没有这个权限

# <a id="file_find" href="#">文件查找</a>
***
### which
* which命令的作用是，在PATH变量指定的路径中，搜索某个系统命令的位置，并且返回第一个搜索结果 
* 参数：-a 将所有知道找到的命令均列出

### locate 
* locate 让使用者可以很快速的搜寻档案系统内是否有指定的档案。其方法是先建立一个包括系统内所有档案名称及路径的数据库`/var/lib/mlocate`，之后当寻找时就只需查询这个数据库，而不必实际深入档案系统之中了。在一般的 distribution 之中，数据库的建立都被放在 crontab 中自动执行
<font color="red">PS: 有时候电脑上可能没有locate命令，这个时候需要先安装mlocate,然后使用updatedb进行更新就可以了</font>
* `/var/lib/mloacate`下的数据库每天更新一次，手动更新的方法是命令`updatedb`.
* 参数
    * -i 忽略大小写的差异
    * -r 后面可接正则表达式的显示方法

### whereis
* whereis同样也是查找数据库
* whereis命令是定位可执行文件、源代码文件、帮助文件在文件系统中的位置
* 参数
    * -b   定位可执行文件，二进制文件
    * -m   定位帮助文件,即manual陆金霞的
    * -s   定位源代码文件
    * -u 查找不在上述三个选项中的其他特殊文件

### find 
* find [PATH] [option] [action]
* 参数
    * 与时间有关的参数
        * -atime/mtime/stime n/+n/-n
        * -newer filename   
    * 与用户或用户组有关的参数
    * 与文件权限及名称有关的参数
    * 其他操作
* find . -size +1000c -print 查找当前目录大于1K的文件 

# <a id="file_tar" href="#">文件与文件系统的压缩与打包</a>
* 区分打包和压缩
    * 打包是指将一大堆文件或目录变成一个总的文件；压缩则是将一个大的文件通过一些压缩算法变成一个小文件。Linux中很多压缩程序只能针对一个文件进行压缩，这样当你想要压缩一大堆文件时，你得先将这一大堆文件先打成一个包（tar命令），然后再用压缩程序进行压缩（gzip bzip2命令）。

### 用SecureCRT来上传和下载文件

### tar
* 用来压缩和解压文件。tar本身不具有压缩功能。他是调用压缩功能实现的 
* 参数
  * -x 从压缩的文件中提取文件
  * -z 支持gzip解压文件
  * -c 建立新的压缩文件
  * -v 显示操作过程
  * -f 指定压缩文件
  * -d 记录文件的差别
* 实例	

	``` 
	tar
	解包：tar xvf FileName.tar
	打包：tar cvf FileName.tar DirName
	```
	```
	.gz
	解压1：gunzip FileName.gz
	解压2：gzip -d FileName.gz
	压缩：gzip FileName
	```
	```
	.tar.gz 和 .tgz
	解压：tar zxvf FileName.tar.gz
	压缩：tar zcvf FileName.tar.gz DirName
	```
	```
	.bz2
	解压1：bzip2 -d FileName.bz2
	解压2：bunzip2 FileName.bz2
	压缩： bzip2 -z FileName
	```
	```
	.tar.bz2
	解压：tar jxvf FileName.tar.bz2
	压缩：tar jcvf FileName.tar.bz2 DirName
	```
	```
	.bz
	解压1：bzip2 -d FileName.bz
	解压2：bunzip2 FileName.bz
	压缩：未知
	```
	```
	.tar.bz
	解压：tar jxvf FileName.tar.bz
	压缩：未知
	```
	```
	.Z
	解压：uncompress FileName.Z
	压缩：compress FileName
	```
	```
	.tar.Z
	解压：tar Zxvf FileName.tar.Z
	压缩：tar Zcvf FileName.tar.Z DirName
	```
	```
	.zip
	解压：unzip FileName.zip
	压缩：zip FileName.zip DirName
	```
	```
	.rar
	解压：rar x FileName.rar
	压缩：rar a FileName.rar DirName 
	```

### 进阶命令
* 完整备份工具: dump, restore
* 光盘写入工具
    * 刻录iso镜像: mkisofs
    * 将iso刻录成光盘或DVD: cdrecord
* 其他常见压缩与备份工具
    * dd
    * cpio

# <a name="file_and_disk" id="file_and_disk">文件系统和磁盘</a>
### df
* 列出文件系统的整体磁盘使用量
* 必要参数
	* -a 全部文件系统列表
	* <b>-h 方便阅读方式显示</b>
	* -H 等于“-h”，但是计算式，1K=1000，而不是1K=1024
	* <b>-i 显示inode信息</b>
	* -k 区块为1024字节
	* -l 只显示本地文件系统
	* -m 区块为1048576字节
	* --no-sync 忽略 sync 命令
	* -P 输出格式为POSIX
	* --sync 在取得磁盘信息前，先执行sync命令
	* -T 文件系统类型
* 选择参数
	* --block-size=<区块大小> 指定区块大小
	* -t<文件系统类型> 只显示选定文件系统的磁盘信息
	* -x<文件系统类型> 不显示选定文件系统的磁盘信息
	* --help 显示帮助信息
	* --version 显示版本信息
 
### du
	暂时使用的较少，不详述

### ln
* Linux文件系统中，有所谓的链接(link)，我们可以将其视为档案的别名，而链接又可分为两种 : 硬链接(hard link)与软链接(symbolic link)，硬链接的意思是一个档案可以有多个名称，而软链接的方式则是产生一个特殊的档案，该档案的内容是指向另一个档案的位置。硬链接是存在同一个文件系统中，而软链接却可以跨越不同的文件系统。
* 软链接(symbolic)
    * 1.软链接，以路径的形式存在。类似于Windows操作系统中的快捷方式
    * 2.软链接可以 跨文件系统 ，硬链接不可以
    * 3.软链接可以对一个不存在的文件名进行链接
    * 4.软链接可以对目录进行链接
    * 5.软链接就是`ln –s 源文件 目标文件`，它只会在你选定的位置上生成一个文件的镜像，不会占用磁盘空间
* 硬链接(通过文件系统的inode连接来产生新文件名，而不是新文件)
    * 1.硬链接，以文件副本的形式存在。但不占用实际空间。
    * 2.不允许给目录创建硬链接
    * 3.硬链接只有在同一个文件系统中才能创建
    * 4.硬链接 `ln 源文件 目标文件`，没有参数-s， 它会在你选定的位置上生成一个和源文件大小相同的文件，无论是软链接还是硬链接，文件都保持同步变化.
* 必要参数
	* -b 删除，覆盖以前建立的链接
	* -d 允许超级用户制作目录的硬链接
	* -f 强制执行
	* -i 交互模式，文件存在则提示用户是否覆盖
	* -n 把符号链接视为一般目录
	* -s 软链接(符号链接)
	* -v 显示详细的处理过程  
* 选择参数
	* -S “-S<字尾备份字符串> ”或 “--suffix=<字尾备份字符串>”
	* -V “-V<备份方式>”或“--version-control=<备份方式>”
	* --help 显示帮助信息
	* --version 显示版本信息 
* 实例
	* 源文件被删除后，并没有影响硬链接文件；软链接文件在centos系统下不断的闪烁，提示源文件已经不存在
	* 关于目录链接的说明
		* 目录只能创建软链接
		* 目录创建链接必须用绝对路径，相对路径创建会不成功，会提示：符号连接的层数过多 这样的错误
		* 在链接目标目录中修改文件都会在源文件目录中同步变化

### 进阶命令
* 磁盘分区: fdisk
* 如果遇到对高于2TB的磁盘进行分区，使用: parted
* 磁盘格式化: mkfs
* 磁盘检验: fsck, badblocks
* 磁盘挂载与卸载: mount, umount
* 磁盘参数修改: mknod, e2label, tune2fs
* 设置开机挂载: /etc/fstab, /etc/mtab

# 性能监控与优化
### top
* 显示当前系统正在执行的进程的相关信息，包括进程ID、内存占用率、CPU占用率等
* 常用参数
  * -b 批处理
  * -c 显示完整的治命令
  *  -I 忽略失效过程
  * -s 保密模式
  * -S 累积模式
  * -i<时间> 设置间隔时间
  * -u<用户名> 指定用户名
  * -p<进程号> 指定进程
  * -n<次数> 循环显示的次数 
* 举例
    ![1](https://cloud.githubusercontent.com/assets/16068384/20957189/3787e3ac-bc89-11e6-8cc7-a6e7fa33908f.png)

  * 第一行：当前系统时间；up xxx 运行了多久；当前1个用户登录；load average后面的三个数分别是1分钟、5分钟、15分钟的负载情况。如果这个数除以逻辑CPU的数量，结果高于5的时候就表明系统在超负荷运转了
  * 第二行：总的进程数量，zombie是僵死的
  * PID行：进程ID；进程所有者；进程优先级（PR）;nice值，负值表示高优先级，正值表示低优先级；VIRT，进程使用的虚拟内存总量，单位kb。VIRT=SWAP+RES；RES,进程使用的、未被换出的物理内存大小，单位kb。RES=CODE+DATA; 

### free
* free 命令显示系统使用和空闲的内存情况，包括物理内存、交互区内存(swap)和内核缓冲区内存。共享内存将被忽略
* 命令参数
	* -b 　以Byte为单位显示内存使用情况。 
	* -k 　以KB为单位显示内存使用情况。 
	* -m 　以MB为单位显示内存使用情况。
	* -g   以GB为单位显示内存使用情况。 
	* -o 　不显示缓冲区调节列。 
	* -s<间隔秒数> 　持续观察内存使用状况。 
	* -t 　显示内存总和列。 
	* -V 　显示版本信息。   
* 内存额定值文件：cat /proc/meminfo
* 实例
	
	```
	free
	free -g
	free -m
	free -s 1 //每秒执行一次
	```

### vmstat/iostat命令/lsof命令
还没接触到，先不做解释
http://www.cnblogs.com/peida/archive/2012/12/25/2833108.html
# 网络命令
### ifconfig
* 用ifconfig命令配置的网卡信息，在网卡重启后机器重启后，配置就不存在。要想将上述的配置信息永远的存的电脑里，那就要修改网卡的配置文件了。
* 命令参数
  * up 启动指定网络设备/网卡。
  * down 关闭指定网络设备/网卡。
  * arp 设置指定网卡是否支持ARP协议
* 实例

	```
	启动和关闭网卡
	ifconfig eth0 up
	ifconfig eth0 down
	为网卡配置ipv6地址
	ifconfig eth0 add 33ffe:3240:800:1005::2/64
	用ifconfig修改MAC地址
	ifconfig eth0 hw ether 00:AA:BB:CC:DD:EE
	配置IP地址
	ifconfig eth0 192.168.120.56 netmask 255.255.255.0 broadcast 192.168.120.255
	启用和关闭ARP协议
	ifconfig eth0 arp
	ifconfig eth0 -arp
	设置最大传输单元
	ifconfig eth0 mtu 1500
	```
### rcp(remote file copy)
* rcp命令用在远端复制文件或目录，如同时指定两个以上的文件或目录，且最后的目的地是一个已经存在的目录，则它会把前面指定的所有文件或目录复制到该目录中。
* 命令参数
  * -r 递归地把源目录中的所有内容拷贝到目的目录中。要使用这个选项，目的必须是一个目录。
  * -p 试图保留源文件的修改时间和模式，忽略umask。
  * -k 请求rcp获得在指定区域内的远程主机的Kerberos 许可，而不是获得由krb_relmofhost⑶确定的远程主机区域内的远程主机的Kerberos许可。
  * -x 为传送的所有数据打开DES加密。这会影响响应时间和CPU利用率，但是可以提高安全性。如果在文件名中指定的路径不是完整的路径名，那么这个路径被解释为相对远程机上同名用户的主目录。如果没有给出远程用户名，就使用当前用户名。如果远程机上的路径包含特殊shell字符，需要用反斜线（\\）、双引号（”）或单引号（’）括起来，使所有的shell元字符都能被远程地解释。需要说明的是，rcp不提示输入口令，它通过rsh命令来执行拷贝。
  * directory 每个文件或目录参数既可以是远程文件名也可以是本地文件名。远程文件名具有如下形式：rname@rhost：path，其中rname是远程用户名，rhost是远程计算机名，path是这个文件的路径 
* 实例
	
	```
	rcp local_file remote_hostname:remote_fileEnter
	rcp remote_hostname:remote_file local_fileEnter
	```

### scp(secure copy)
* scp传输是加密的
* 命令格式： scp [参数] [原路径] [目标路径] 
* 命令参数
  * -1  强制scp命令使用协议ssh1  
  * -2  强制scp命令使用协议ssh2  
  * -4  强制scp命令只使用IPv4寻址  
  * -6  强制scp命令只使用IPv6寻址  
  * -B  使用批处理模式（传输过程中不询问传输口令或短语）  
  * -C  允许压缩。（将-C标志传递给ssh，从而打开压缩功能）  
  * -p 保留原文件的修改时间，访问时间和访问权限。  
  * -q  不显示传输进度条。  
  * -r  递归复制整个目录。  
	* -v 详细方式显示输出。scp和ssh(1)会显示出整个过程的调试信息。这些信息用于调试连接，验证和配置问题。   
	* -c cipher  以cipher将数据传输进行加密，这个选项将直接传递给ssh。   
	* -F ssh_config  指定一个替代的ssh配置文件，此参数直接传递给ssh。  
	* -i identity_file  从指定文件中读取传输时使用的密钥文件，此参数直接传递给ssh。    
	* -l limit  限定用户所能使用的带宽，以Kbit/s为单位。     
	* -o ssh_option  如果习惯于使用ssh_config(5)中的参数传递方式，  
	* -P port  注意是大写的P, port是指定数据传输用到的端口号   
	* -S program  指定加密传输时所使用的程序。此程序必须能够理解ssh(1)的选项。 
* 实例
	* 从本地服务器复制到远程服务器
		
		```
		scp local_file remote_username@remote_ip:remote_folder/file
		scp local_file remote_ip:remote_folder/file 
		```
	* 从远程服务器复制到本地服务器
		
		```
		scp root@192.168.120.204:/opt/soft/nginx-0.5.38.tar.gz /opt/soft/
		```
### route/traceroute/netstat/ss

# 其他常用命令
### diff
* diff命令能比较单个文件或者目录内容。如果指定比较的是文件，则只有当输入为文本文件时才有效。以逐行的方式，比较文本文件的异同处。如果指定比较的是目录的的时候，diff 命令会比较两个目录下名字相同的文本文件。列出不同的二进制文件、公共子目录和只在一个目录出现的文件。
* 实例

	```
	diff a.txt b.txt
	diff a/ b/
	```
	
### cal
* 命令参数
	* -1 显示一个月的月历
	* -3 显示系统前一个月，当前月，下一个月的月历
	* -s  显示星期天为一个星期的第一天，默认的格式
	* -m 显示星期一为一个星期的第一天
	* -j  显示在当年中的第几天（一年日期按天算，从1月1号算起，默认显示当前月在一年中的天数）
	* -y  显示当前年份的日历  
* 实例
	
	```
	cal 9 2012
	cal -y 2013 
	```

### crontab
* 相关文件
	* /var/spool/cron/ 所有用户crontab文件存放的目录,以用户名命名
	* /etc/cron.deny 该文件中所列用户不允许使用crontab命令
	* /etc/cron.allow 该文件中所列用户允许使用crontab命令

### watch/at

