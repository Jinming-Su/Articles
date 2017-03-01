### 基础知识
* 异常和中断区别
    * 中断由外部设备产生而异常由CPU内部产生
    * 中断产生的原因和CPU当前执行的指令无关,而异常的产生就是由于CPU当前执行的指令出了问题
* 用户模式与特权模式
    * 操作系统可以在页表中设置每个内存页面的访问权限,有些页面不允许访问,有些页面只有在CPU处于特权模式时才允许访问,有些页面在用户模式和特权模式
都可以访问,访问权限又分为可读、可写和可执行三种.
* 用户空间和内核空间
    * 通常操作系统把虚拟地址空间划分为用户空间和内核空间,例如x86平台的Linux系统虚拟地址空间是0x00000000~0xffffffff,前3GB(0x00000000~0xbfffffff)是用户空间,后1GB(0xc0000000~0xffffffff)是内核空间.
* 编译阶段，指令中用到的符号地址都是相对地址，这个阶段已经生成符号表；链接阶段，其中的地址都改成加载时的内存地址
* 两个Segment必须加载到内存中两个不同的页面,因为MMU的权限保护机制是以页为单位的,一个页面只能设置一种权限
* 编译C语言程序
    * `gcc -S test.c`: 生成汇编代码
    * `gcc -c test.s`: 生成目标文件
    * `gcc test.o`: 生成可执行

### x86汇编程序基础
* 最简单的汇编程序
    * 实例
        
        ```
        #PURPOSE: Simple program that exits and returns a
        #status code back to the Linux kernel
        #
        #INPUT:none
        #
        #OUTPUT: returns a status code. This can be viewed
        #by typing
        #
        #echo $?
        #
        #after running the program
        #
        #VARIABLES:
        #%eax holds the system call number
        #%ebx holds the return status
        #
        .section .data
        .section .text
        .globl _start
        _start:
        movl $1, %eax # this is the linux kernel command
        # number (system call) for exiting
        # a program
        movl $4, %ebx # this is the status number we will
        # return to the operating system.
        # Change this around and it will
        # return different things to
        # echo $?
        int $0x80 # this wakes up the 
        ```
        * 在x86汇编中，以.开头的名称并不是指令的助记符，不会被翻译成机器指令，而是给汇编器一些特殊的指示，称为伪指令.
        * `.section .data`: `.section`把代码划分成若干个段，程序被操作系统加载时，每个段被加载到不同的地址，操作系统对不同的页面设置不同的读、写、执行权限.`.data`段保存程序的数据，是可读可写的，相当于C的全局变量.
        * `.section .text`: `.text`段保存代码，是只读和可执行的，后面的指令都属于`.text`段.
        * `.globl _start`: `.globl`告诉编译器`_start`这个符号要被链接器用到，所以要在目标文件的符号表中标记它是一个全局符号.`_start`类似C程序中的main函数，是整个程序的入口.
        * `movl $1, %eax`: 要求CPU内部产生一个数字1并保存到eax寄存器中.其中l表示long, 32位.
        * `int $0x80`: `int`指令称为软中断指令，可以用这条指令故意产生一个异常，异常的处理与中断类似，CPU从用户模式切换到特权模式，然后跳转到内核代码中执行异常处理程序.`int $0x80`这种异常称为系统调用.
* 寻址方式
    * 通用格式: `ADDRESS_OR_OFFSET(%BASE_OR_OFFSET,%INDEX,MULTIPLIER)`  
      所表示的地址的计算方式: `FINAL ADDRESS = ADDRESS_OR_OFFSET + BASE_OR_OFFSET + MULTIPLIER * INDEX`
    * 直接寻址: `movl ADDRESS, %eax`
    * 间接寻址: `movl (%eax), %ebx`
    * 变址寻址: `movl data_items(,%edi,4)`
    * 基址寻址: `movl 4(%eax), %ebx`
    * 立即数寻址，寄存器寻址
* ELF文件
    * ELF文件格式是一个开放标准，各种Unix的可执行文件都财政用ELF标准，它有三种不同的类型
        * 可重定位的目标文件
        * 可执行文件
        * 共享库
    * `objdump`工具
        * `objdump -C max.o`: 查看二进制文件
        * `objdump -d max.o`: 反汇编

