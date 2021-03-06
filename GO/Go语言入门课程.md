---
title: Go语言入门课程
date: 2017-03-02 00:02:01
categories:
- GO
tags:
- go
description: ...
---

# Go语言入门课程
`非淡泊无以明志，非宁静无以致远`
> 近期要看一下D-judge(NEUOJ评测后端)的源代码，需要学习一些golang.<font color="red">搞计算机的童鞋们学东西一定要边学边实践，否则容易和之前学习内容混乱，似是而非，还不如不学.</font>

## 概述
* 特点：
  * 静态类型、编译型的开源语言
      * 静态类型：指在编写go时需要明确变量的类型，有两种方法实现（1）声明时携带（2）简介推导出变量类型.
      * 编译型：运行前需要编译成机器代码，比如C.
  * 脚本化语法，支持多种编程范式
      * 脚本化语法保证易编写
      * 多种编程范式：包括函数式编程和面向对象编程
  * <b>原生并发编程支持</b>
      * 区别于其他语言通过第三方函数库支持
* 缺点
  * 语法糖没有python那么多
      * 语法糖：可以理解为--为了简单完成某种功能而提供的一个函数或者方法等.
  * 运行速度不及C，但已经赶超了C++和Java
  * 第三方库比较少
## 基本规则
* 工作区
  * 通过GOPATH设置的地方
  * 结构
    
        ```
        src  //存放源码，以代码包为组织形式
        pkg  //存放归档文件（以.a为后缀的文件），以代码包为组织形式
        bin  //存放当前工作区的go程序的可执行文件
        ```

* 源码文件分类
  * 命令源码文件（带有main函数）、库源码文件（都是go语言程序）
  * 测试源码文件（辅助源码文件）
* 常用语法
  * go中`=`表示赋值，`:=`表示声明并赋值
  * Go语言中并没有继承的概念
  * Go语言中的任何类型都控接口它的实现类型
  
## 命令基础
* go run命令
  * 用于运行命令源码文件，只能接受一个源码文件，其他源码文件作为文件参数
  * 运行过程：先进行编译，生成文件放在临时文件夹中。之后进行运行。
  * 常用标记
      * -a, 强制编译相关代码
      * -n, 打印编译过程中所需运行的命令，但不真正执行它们
      * -p n, 并行编译，n为并行数量
      * -v, 列出被编译的代码包名称
      * -work, 显示编译时创建的临时工作目录的路径，并不删除，默认是删除的
      * -x, 打印编译过程中所需运行的命令，并真正执行
* go build命令
  * 编译源码文件或代码包，编译非命令文件不产生任何结果。如果不添加参数，默认对当前目录作为代码包进行编译。可接受多个命令源码文件
* go install命令
  * 编译并安装代码包或源码文件；安装代码包会在pkg/<平台相关目录如linux_amd64>下生成归档文件；安装命令源码文件会在当前工作区的bin目录或$GOBIN目录下生成可执行文件
* go get命令
  * 从远程代码仓库（如github）上下载并安装代码包
  * 受支持的代码版本控制系统有: git, Mercurial(hg),svn,Bazaar
  * 常用标记
      * -d: 只下载，不安装
      * -fix: 下载后进行修正，之后在进行编译和安装
      * -u: 利用网络来更新已有的代码包及其依赖包

## 基本数据类型
* 变量、常量、函数、结构体和接口被统称为“程序实体”，而它们的名字被统称为“标识符”.任何Go语言源码文件都由若干个程序实体组成的.
* 名字首字母为大写的程序实体可以被任何代码包中的代码访问到.而名字首字母为小写的程序实体则只能被同一个代码包中的代码所访问.
* 关键字
    
    |用途|关键字|
    |---|---|
    |程序声明|import,package|
    |程序实体声明和定义|chan, const, func, interface, map, struct, type, var|
    |程序流程控制|go, select, break, case, continue, default, defer, else, fallthrough, for, goto, if, range, return, switch|
* 变量(var)和常量(const)
* 整数类型命名和宽度
  * int8/uint8, int16/uint16, int32/uint32, int64/uint64, int/uint(这两个长度与计算机架构相关)
* 浮点数类型
  * float32,float64
  * 浮点数只能用10进制表示法，不能由8进制或16进制表示
* 复数类型
  * complex64(float32实数部分+float32虚数部分i), complex128(float64实数部分+float64虚数部分i)
* byte和rune
  * byte是uint8的别名,rune是int32的别名
  * 一个rune类型的值表示一个Unicode字符
* 字符串类型
  * 采用UTF-8编码，用反引号表示字符串值本身，双引号程序编译期间被转义.

## 高级数据类型
* 数组类型
  * 例如`type MyNumbers [3]int`
* 切片类型
  * 例如`type MySlice []int`
  * 切片通常是在数组对象上操作得到的一个对象,例如`var slice1 = numbers3[1:4]`
  * 切片值的长度总是为元素上界索引与元素下界索引的差值len()
  * 切片值的容量即为它的第一个元素值在其底层数组中的索引值与该数组长度的差值的绝对值cap()
  * 切片类型属于引用类型，它的零值为nil
* 字典类型
  * 例如`map[int]string`
  * 字典的键类型必须是可比较的，否则会引起错误.也就是说，它不能是切片、字典或函数类型.
  * 对于字典值来说，如果其中不存在索引表达式欲取出的键值对，那么就以它的值类型的空值（或称默认值）作为该索引表达式的求值结果.由于字符串类型的空值为""，因此，在有些情况下，无法判断是空字符串还是不存在，go语言提供的字典的索引表达式有两个求职结果,如`e, ok := mm[5]`
  * 删除, 如`delete(mapname, key)`
  * map属于引用类型，零值为nil
* 通道类型
  * 用于在不同Goroutine之间传递类型化的数据，并且是并发安全的
  * 例如`chan T`,初始化需要使用make函数,`make(chan int, 5)`
  * 向通道中发送数据`ch1 <- "value1"  `
  * 从通道中接受数据`value := <- ch1`
  * 与字典类似有`value, ok := <- ch1 `
  * 关闭通道`close(ch1) `
  * 通道属于引用类型，零值为nil
* 函数
  * 函数类型的零值是nil
* 结构体
  * 结构体类型属于值类型,零值为其中字段的值均为相应类型的零值的值.
* 接口  
* 指针
## 流程控制
* if
    
    ```
    if number := 4; 100 > number {
        number += 3
    } else if 100 < number {
        number -= 2
    } else {
        fmt.Println("OK!")
    }   
    ```
* switch
    
    ```
    v := 11
    switch i := interface{}(v).(type) {
    case int, int8, int16, int32, int64:
        fmt.Printf("A signed integer: %d. The type is %T. \n", i, i)
    case uint, uint8, uint16, uint32, uint64:
        fmt.Printf("A unsigned integer: %d. The type is %T. \n", i, i)
    default:
        fmt.Println("Unknown!")
    }
    ```
* if
    
    ```
    for i := 0; i < 10; i++ {
        fmt.Print(i, " ")
    }  
    for i, v := range "Go语言" {
        fmt.Printf("%d: %c\n", i, v)
    } 
    ```
* select 
  * select语句属于条件分支流程控制方法，不过它只能用于通道.select语句中的case关键字只能后跟用于通道的发送操作的表达式以及接收操作的表达式或语句.
* defer
  * defer语句并不会被立即执行，它的确切的执行时机是在其所属的函数的执行即将结束的那个时刻
* error
  * 借口error
  * 标准库代码包errors
  * errors.New是一个很常用的函数
* panic
  * 内建函数panic可以让我们人为地产生一个运行时恐慌。不过，这种致命错误是可以被恢复的.在Go语言中，内建函数recover就可以做到这一点.
  * recover函数必须要在defer语句中调用才有效.因为一旦有运行时恐慌发生，当前函数以及在调用栈上的所有代码都是失去对流程的控制权.只有defer语句携带的函数中的代码才可能在运行时恐慌迅速向调用栈上层蔓延时“拦截到”它.
* go
  * go语句和通道类型是Go语言的并发编程理念的最终体现
  * go语句仅由一个关键字go和一条表达式语句构成
  * 在go语句被执行时，其携带的函数（也被称为go函数）以及要传给它的若干参数（如果有的话）会被封装成一个实体（即Goroutine），并被放入到相应的待运行队列中.Go语言的运行时系统会适时的从队列中取出待运行的Goroutine并执行相应的函数调用操作.
  * go函数的执行时间不确定
