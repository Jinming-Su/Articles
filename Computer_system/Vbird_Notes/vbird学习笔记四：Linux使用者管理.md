***
### Unit 14 Linux账号管理与ACL权限设置
* 用户标识符: UID与GID
    * Linux主机并不会直接认识“账号”，仅认识一组号码ID，之后通过`/etc/passwd`进行对应.
    * 在`/etc/passwd`文件中，UID列是用户标识符，UID对应的数值有分三个段:
        * 0(系统管理员)，当UID是0时，表示系统管理员.
        * 1-499(系统账号)，白留给系统使用的ID.由于系统上面启动的服务希望使用较小的权限去运行，因此不希望使用root的身份去执行这些服务，所以提供这些运行中程序的所有者账户.这些系统账户通常是不可登录的，所有才有/sbin/nologin这个特殊的shell存在.通常1-99由linux发行版自行系统账号，100-499用户可使用.
        * 500之后(可登录账户)，给一般用户使用.
    * `/etc/shadow`字段的意义
        * 账户
        * 密码
        * 最近更改密码的日期(从1970.1.1算起)
        * 密码不可被更改的天数
        * 密码需要重新更改的天数
        * 密码需要更改期限前的警告天数
        * 密码过期后的账号宽限天数
        * 账号失效日期
        * 保留
* 有效与初始用户组: groups，newgrp
    * 初始用户组: 在`/etc/passwd`中每个用户对应的GID就是他的初始用户组，当用户登陆系统的时候，就立即拥有这个用户组的相关权限.
    * 在`/etc/group`中记录了每个组的相关信息，和这个组包含的账号.一个用户可能包含在多个组中，当新建一个文件的时候要给出GID，这个就需要检查当前的有效用户组了.
    * 有效与支持用户组的查看: groups
        * `groups`: 查看当前用户支持的用户组，输出的第一个用户组为有效用户组.通常有效用户组的作用是新建文件.
        * `newgrp xxx`: 有效用户组的切换.
* 账号管理
    * 新增与删除用户
        * `useradd [-u UID] [-g 初始用户组] [-G 次用户组] [-mM] [-c 说明栏] [-d 主文件夹绝对路径] [-s shell] 用户账户名`
            * -m：强制创建用户主文件夹(一般账号默认值)
            * -M: 强制不要创建用户主文件夹(系统账号默认值)
            * `useradd -D`: 查看useradd的默认值
        * `passwd username`: 设置密码，passwd还有许多别的参数
        * `chage`: 密码参数显示
        * `usermod`: 微调useradd中的用户参数
        * `userdel [-r] username`
            * -r: 连同用户的主文件夹一起删除
    * 用户功能
        * `finger`: 查阅很多用户相关信息.
        * `chfn`: change finger
        * `chsh`: change shell
        * `id`: 查询某人或自己的相关UID/GID等信息
    * 新增与删除用户组
        * `groupadd`
        * `groupmod`
        * `groupdel`
        * `gpasswd`: 用户组管理员功能
* 主机的具体权限规划：ACL(Access Control List)的使用
* 用户身份切换
    * `su [-lm] [-c 命令] [username]`
        * su可进行任意身份的切换，但是需要对应身份的密码
        * -/-l: 使用login-shell的变量文件读取方式登录系统
        * -m/-p: 使用目前的环境变量
        * -c: 仅进行一次命令
        * 单纯使用su，则切换到root身份，读取变量的设置方式为non-login shell方式，这种方式下很多原本的变量不会被改变.
    * `sudo [-b] [-u 新用户账号]`
        * 只有在/etc/sudoers中的用户可执行sudo命令，仅需要输入自己的密码即可.
        * -b: 将后续的命令让系统自行执行，而不与目前的shell产生影响.
        * -u: 后面可接欲切换的用户
    * `visudo`
        * 如果需要设置其他用户执行root权限命令，需要去修改/etc/sudoers文件，但是此文件的修改需要按照一定的语法因此无法直接修改，需要使用visudo命令.
        * 可以通过添加用户和用户组的形式授予权限.也可以设置免密码输入.
* 用户的特殊shell和PAM模块
    * 特殊的shell--/sbin/nologin
        * 系统账户的shell使用/etc/nologin.
        * 系统账户是无法登录的，指的是这个用户无法使用某种shell登录系统，并不是说这个用户无法使用其他的系统资源.
    * PAM模块
        * Pluggage Authentication Modules，嵌入式验证模块
        * PAM可以说是一套应用程序编程接口.它提供一连串的验证机制，只要用户验证阶段的需求告知PAM后，PAM就能够汇报用户验证的结果(成功或者失败).
* Linux主机上的用户信息传递
    * 主要用户同一主机上，不同用户的信息传递
    * 查询用户: w, who, last, lastlog
        * w: 显示目前登录系统的用户信息
        * who: 类似w
        * last：列出当月登录这信息
        * lastlog: 输出/var/log/lastlog文件，每个账户的最近登录时间
    * 用户对谈: write, mesg, wall
    * 用户邮件信箱: mail

### Unit 15 磁盘配额与高级文件系统管理
* 磁盘配额Quota
* 软件磁盘陈列(Software RAID)
    * RAID(Redundant Array of Inexpensive Disks), 廉价磁盘冗余阵列: 通过软件或者硬件的技术把多个较小的磁盘整合成一个较大的磁盘设备；而这个较大的磁盘不仅具有存储能力，还具有数据保护的功能.RAID由于选择的等级不同，是的整合后的磁盘具有不同的功能.
        * RAID 0(等量模式，stripe)，性能最佳: 将磁盘切成等量的区块，并在不同的磁盘上交错存放数据.
        * RAID 1(影像模式，mirror)，完整备份: 同一数据保存两份到不同的磁盘.
        * RAID 0+1, RAND 1+0
        * RAID 5， 性能与数据备份的均衡考虑: 使用三块以上的磁盘，写入类似RAID-0方法，每个循环中一次在某块磁盘加入一个同位检出数据，当有一块磁盘损毁时，可修复.
        * Spare Disk，预备磁盘
    * software RAID
        * 利用mdadm实现软件磁盘阵列
* 逻辑卷管理器(LVM)
    * 作用: 可以弹性调整文件系统的容量.具体说，LVM将几个物理的分区或磁盘通过软件组合成为一块看起来是独立的大磁盘(VG)，然后将这块大磁盘在经过分成为可使用分区(LV)，最终就能挂载使用了.

### Unit 16 例行性工作(crontab)
* 关于例行性工作
    * 工作调度种类
        * 突发性的的at: at是个可以处理仅执行一次就结束调度的命令，需要atd服务的支持.
        * 例行性的crontab: crontab这个命令所执行的工作会循环进行下去.除了使用命令执行外，也可以编辑/etc/crontaba来支持.
    * Linux上常见的例行性工作
        * 进行日志文件的轮替(log rotate):定期挪动日志文件，把旧的和新的分开存放.
        * 日志文件分析logwatch
        * 新建locate的数据库
        * 新建whatis数据库，这个食欲man page有关的查询命令
        * 新建RPM软件日志文件
        * 删除临时文件
        * 与网络服务有关的分析行为
* 仅执行一次的工作调度
    * 首先需要启动atd服务`/etc/init.d/atd restart`
    * at的工作方式: 使用at命令来生成所要运行的工作，并将这个工作以文本文件的方式写入/var/spool/at目录内，给工作便能等待atd服务的取用和执行.
    * at的使用限制:
        * 先寻找/etc/at.allow，写入这个文件的用户允许使用at，未写入的不允许.
        * 如果/etc/at.allow不存在，就查看/etc/at.deny，写入的不允许，未写入的允许.
        * 如果两个文件都不存在，则只有root允许使用at命令.
    * `at [-mldv time]`或`at -c 工作号码`
        * -m: 当at完成后，以email通知用户完成
        * -l: 相当于atq，列出当前系统上的所有该用户的at调度
        * -d: 相当于atrm，可以取消一个在at调度中的工作
        * -v: 可以使用较明显的时间格式列出at调度中的任务列表
        * -c: 可以列出后面接的该项工作的实际命令内容
    * `atrm 工作号码`
        * 删除一项任务
    * batch命令控制系统有空时才进行后台任务
* 循环执行的例行性工作调度
    * crontab的使用限制
        * 使用/etc/cron.allow和/erc/cron.deny，工作被记录在/var/spool/cron里面，cron执行的每一项工作会被记录在/var/log/cron这个日志文件中.
    * `crontab [-u username] [-l|-e|-r]`
        * -u: 只有root可以使用这个参数，帮助其他用户新建/删除crontab工作调度.
        * -e: 编辑crontab的工作内容
        * -l: 查阅crontab的工作内容
        * -r: 删除所有的crontab的工作内容，如果要仅删除一项，需要-e去b编辑.
    * 系统的配置文件: /etc/crontab
        * crontab -e是针对用户的cron来设计的，如果执行系统的例行性任务，可以直接通过编辑/etc/crontab即可.cron会每分钟读取一次/etc/crontab和/var/spool/cron里的数据内容.
        * crontab的完整路径是/usr/bin/crontab
* 可唤醒停机期间的工作任务
    * anacron和/etc/anacrontab

### 程序管理与SELinux初探
* 进程
    * 执行一个命令或程序可以出发一个时间二取得一个PID，产生一个进程.
    * 常驻进程被成为服务(daemon)或守护进程
    * 多用户环境是因为每个人登录后取得的shell的PID不同，多任务行为是因为CPU调度.
* 工作管理
    * 直接将命令丢到后台中执行: `&`
    * 将目前的工作丢到后台中暂停: `ctrl-z`
    * 查看目前的后台工作状态: `jobs [-lrs]`
        * -l: 除了列出job number与命令串之外，同事列出PID的号码
        * -r: 仅列出正在后台run的工作
        * -s: 仅列出正在后台当中暂停的工作
    * 将后台工作拿到前台来处理: `fg`
    * 让工作在后台下的状态变成运行中: `bg`
    * 管理后台当中的工作: `kill -signal %jobnumber`或`kill -l`
        * -l： 列出目前kill能够使用的信号(signal)
        * signal可取值
            * -1: 重新读取一次参数的配置文件(reload)
            * -2: 同ctrl-c
            * -9: 立即强制删除一个工作
            * -15: 以正常的程序方式终止一项任务.
    * <font color="red">在工作管理中提出的后台指的是在终端机模式下可以避免ctrl-c终端的一个情景，并不是放到系统的后台去.如果关闭终端，工作不会继续.</font>
    * 脱机管理
        * 使用`nohup`命令保证在脱机或注销系统后继续执行工作，`nohup cmd`在终端机前台工作，`nohup cmd &`在终端机后台工作.
* 进程管理
    * 进程查看(利用静态的ps或者动态的top，还能以pstree来查阅程序树之间的关系)
        * ps: 将某个时间点的进程运行情况选取下来
            * `ps aux`: 查看系统所有的进程数据
            * `ps -lA`: 查看所有系统的数据
            * `ps axjf`: 连同部分进程树状态
            * -A: 所有进程均显示，同-e
            * -a: 不与terminal有关的所有进程
            * -u: 有效用户相关的进程
            * x: 通常与a一起使用列出完成信息
            * 输出格式规划:
                * l: 较长较详细列出每个PID的信息
                * j: 工作的格式(job format)
                * -f: 更为完整的输出
            * <b>主要记住两个命令`ps -l`(只查看自己bash相关进程)和`ps aux`</b>
        * 动态查看进程的变化`top [-d 数字]`或`top [-bnp]`
            * -d: 接秒数，表示整个进程界面更新的秒数，默认5s
            * -b: 以批次的方式执行top
            * -n: 与-b搭配，表示需要进行几次top的结果输出
            * -p: 制定某些PID来进行查看检测
        * `pstree [-A|U] [-up]`
            * -A: 各进程树之间的连接以ascii字符连接
            * -U: 各进程树之间的连接以utf8来连接
            * -p: 同时列出每个进程的PID
            * -u: 同时列出每个进程的所属账号名称.
    * 进程管理
        * 通过给予进程一个信号(signal)去告知该进程需要做什么.
        * 通过`kill -l`查看所有的signal
        ![1](https://cloud.githubusercontent.com/assets/16068384/21037747/38312de4-be0b-11e6-8a24-3ee31e1893e2.png) 
        * `kill - signalname PID/%jobnumber`: 将某个signal传送给某个工作或进程
        * `killall -signal 命令名称`
    * 进程执行顺序
        * PRI: 优先执行序
        * `PRI(new) = PRI(old) + nice`
        * 可以调节nice值
            * `nice [-n 数字] cmd`: 新执行的命令给予新的nice值
            * `renice [number] PID`: 调整nice值
    * 系统资源查看
        * free
        * uname
        * uptime
        * `netstat -[atunlp]`
            * -a: 将目前系统所有的连接、监听、socket数据都列出来
            * -t: 列出tcp网络数据包的数据
            * -u: udp
            * -n: 不列出进程的服务名称，以端口号显示
            * -l: 列出目前正在网络监听的服务
            * -p: 列出该网络服务的进程PID
        * dmesg
        * vmstat
* 特殊文件与程序
    * `/proc/*`
        * 内存当中的数据都写入到`/proc/*`这个目录下，可以通过`ll /proc`进行查看
    * 与使用中的文件相关的
        * `fuser`: 通过文件找到正在使用该文件的程序
        * `lsog`: 列出被进程所打开的文件名
        * `pidof`: 找出某个正在执行的进程的PID 

### Unit 18 认识系统服务(daemons)
* 概念
    * deamon的主要分类
        * 可以自行单独启动的stand_alone
            * stand_alone的deamon响应速度较快
            * 常见的有www的deamon(httpd)、FTP的daemon(vsftpd)
        * 通过一个super daemon来统一管理的服务
            * 这个super daemon是inetd或者xinetd
            * 常见的super daemon所管理的服务如telnet
            * 针对super daemon的处理模式有两种
                * 多线程
                * 单线程
    * deamon根据提供服务的工作状态类分类
        * signal-control，例如打印机服务(cupsd)
        * interval-control，例如atd与crond
    * 服务与端口的对应
        * `/etc/services`
    * deamon的启动脚本与启动方式
        * `/etc/init.d/*`: 各服务启动脚本放置处
        * `/etc/sysconfig/*`: 各服务的初始化环境配置文件
        * `/etc/xinetd.conf, /etc/xinetd.d/*`: super daemon配置文件
        * `/etc/*`: 各服务各自的配置文件
        * `/var/lib/*`: 各服务产生的数据库
        * `/var/run/*`: 各服务的程序的PID记录处
        * stand alone的启动
            * `/etc/init.d/xxx restart`
            * `service xxx restart`: 实质还是第一种方法，service的路径为/sbin/service
        * super daemon的启动
            * super daemon本身是一个stand alone
            * `/etc/xinet.d/xxx restart`
* 服务的防火墙管理xinetd, TCP wrappers
    * 任何以xinetd管理的服务都可以通过/etc/hosts.allo, /etc/hosts.deny来设置防火墙.关于防火墙，可以理解为针对源IP进行允许或拒绝的设置，以决定该连接能否成功实现连接的一种方式.
    * 其实/etc/hosts.allow和/etc/hosts.deny也是/usr/sbin/tcpd的配置文件，而/usr/sbin/tcpd则是用来分析进入系统的tcp数据包的一个软件.TCP数据包的头部主要记录来源与目的主机的IP和port，因此通过分析TCP数据包并结合/etc/hosts.{allow, deny}的规则比较，就可以决定该连接是否能够进入我们的主机.
    * TCP Wrappers
* 设置开机立即启动的服务
    * `chkconfig`: 管理系统服务默认开机启动与否
        * `chkconfig --list`: 将目前的各项服务状态栏显示出来
        * `chkconfig [--level [0123456]] [服务名称] [on|off]`: 设置某个服务在该level下启动或关闭

### Unit 19 日志文件
* Linux常用的日志文件
    * `/vat/log/cron`: 例行性工作调度日志
    * `/var/log/dmesg`: 记录系统在开机的时候内核检测过程所产生的各项信息
    * `/var/log/lastlog`: 记录系统上所有的账号最近一次登录系统是的相关信息，对应于lastlog命令
    * `/var/log/maillog`或`/var/log/mail/*`: 记录邮件往来信息
    * `/var/log/messages`: 记录系统发生的错误信息
    * `/var/log/secure`: 只要牵涉到需要输入帐密的软件，登录信息都会被记录在这里
    * `/var/log/wtmp, /var/log/faillog`: 记录正确和错误登录系统者的账户
    * `/var/log/httpd/*, /var/log/news/*, /varlog/samba/*`: 记录网络服务产生的各项信息
* syslogd: 记录日志文件的服务
    * syslog的配置文件: /etc/syslog.conf
* logrotate: 日志文件的轮替
* 分析日志文件
