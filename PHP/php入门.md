# PHP
## 开发环境
* xampp
* phpstorm
* ubuntu 14.04

## 第一个程序
> 注意一个比较坑的地方，开始的时候，我把自己的工程放在/opt/lampp/apache2/htdocs（这个地方也是大家在使用apache2服务器时常用的地方），试了好久都不能执行，开始的时候还以为是权限的问题，试了一下也没有解决.最后发现应该把项目放在/opt/lanmpp/htdocs.

* 第一个php程序
    
    ```
    <!DOCTYPE html>
    <html>
    <body>
        <?php
            echo "Hello sjming";
        ?>
    </body>
    </html>
    ```

## 基础知识
* php脚本在服务器上执行，把纯html结果发回给浏览器
* 变量是用来存储数据的容器
* ”var_dump”函数可以打印变量的数据类型和值,本身具有换行，可以输出数组，注意和print_r及echo的区别：var_dump() 能打印出类型和值,print_r() 只能打出值，echo不能打印数组，前两个可以
* PHP_EOL 为换行符

## 语法
* 变量
    * 命名规则
        * 变量必须首字母为$,以字母或下划线 “_”开头
        * 只能由字母、数字、以及“_”组成，还能包含汉字
        * 区分大小写
    * 作用域
        * local和global
        * static
        * parameter
    * 测试local和global
    > 注意的一点:在所有函数外部定义的变量，拥有全局作用域。除了函数外，全局变量可以被脚本中的任何部分访问，要在一个函数中访问一个全局变量，需要使用 global 关键字.
            
        ```
        <?php
        $x=5; // 全局变量
        
        function myTest()
        {
            $y=10; // 局部变量
            echo "<p>测试函数内变量:<p>";
            echo "变量 x 为: $x";
            global $x;
            echo "变量 x(+global) 为: $x";
            echo "<br>";
            echo "变量 y 为: $y";
        }
        
        myTest();
        
        echo "<p>测试函数外变量:<p>";
        echo "变量 x 为: $x";
        echo "<br>";
        echo "变量 y 为: $y";
        ?>
        ```
    
        ```
        //测试结果
        测试函数内变量:
    
        Notice: Undefined variable: x in /opt/lampp/htdocs/test-php/index.php on line 8
        变量 x 为: 变量 x(+global) 为: 5
        变量 y 为: 10
        
        测试函数外变量:
        
        变量 x 为: 5
        
        Notice: Undefined variable: y in /opt/lampp/htdocs/test-php/index.php on line 20
        变量 y 为:
        ```
    > 还有一种global变量的使用方法:PHP 将所有全局变量存储在一个名为 $GLOBALS[index] 的数组中，可以直接使用.
        
        ```
        <?php
        $x=5;
        $y=10;
        
        function myTest()
        {
            $GLOBALS['y']=$GLOBALS['x']+$GLOBALS['y'];
        }
        
        myTest();
        echo $y;
        ?> 
        ```
    * 测试static
    > 想象一种场景:当一个函数完成时，它的所有变量通常都会被删除。然而，有时候您希望某个局部变量不要被删除。这种情况下就可以借助static关键字,在变量的第一次声明时使用static.
    
        ```
        <?php
    
        function myTest()
        {
        static $x=0;
        echo $x;
        $x++;
        }
        
        myTest();
        myTest();
        myTest();
        
        ?> 
        ```
        ```
        //测试结果
        012
        ```
* 超级全局变量
    * 列表
        * $GLOBALS
        * $_SERVER
        * $_REQUEST
        * $_POST
        * $_GET
        * $_FILES
        * $_ENV
        * $_COOKIE
        * $_SESSION
    * $_SERVER 是一个包含了诸如头信息(header)、路径(path)、以及脚本位置(script locations)等等信息的数组。这个数组中的项目由 Web 服务器创建.具体查阅http://www.runoob.com/php/php-superglobals.html
    * $_REQUEST 用于收集HTML表单提交的数据
    * $_POST 被广泛应用于收集表单数据，在HTML form标签的指定该属性："method="post"
    * 剩余的在后文介绍
* 数据类型
    * 分类
        * String
        * Integer
        * Float
        * Boolean
        * Array
        * Object
        * NULL
        > 注意：Boolean：当我们用”echo”指令输出布尔类型时，如果是“true”则输出的是“1”，“false”则什么也不输出
    * 测试Array
        
        ```
        <?php
            $cars=array("Volvo","BMW","Toyota");
            echo 'var_dump:<br>';
            var_dump($cars);
            echo '<br>print_r:<br>';
            print_r($cars);
        ?>
        ```
        ```
        //测试结果
        var_dump:
        array(3) { [0]=> string(5) "Volvo" [1]=> string(3) "BMW" [2]=> string(6) "Toyota" }
        print_r:
        Array ( [0] => Volvo [1] => BMW [2] => Toyota ) 
        ```
    * php对象就是php面向对象编程中的类的实例
* 输出方式:echo和print语句
    * 区别  
        echo - 可以输出一个或多个字符串,print - 只允许输出一个字符串，返回值总为1.echo 输出的速度比 print 快， echo 没有返回值.
    * echo
        * echo 是一个语言结构,可以说不是一个函数
        * 由于上一条，所以格式: echo 或 echo()
        * echo可以直接输出html标签，多个参数用逗号隔开
    * 测试echo
        
        ```
        <?php
            echo "<h2>PHP is fun!</h2>";
            echo "This", " string", " was", " made", " with multiple parameters.";
        ?>
        ```
    * print同样是一个语言结构，也有两种输出格式.除了上边所述区别外，完全相同.

## 常量
> 注意：常量名不需要加 $ 修饰符;常量在定义后，默认是全局变量

* 语法（必须使用define定义）
    `bool define ( string $name , mixed $value [, bool $case_insensitive = false ] )`
* 测试区分默认区分大小写
    
    ```
    <?php
        // 区分大小写的常量名
        define("GREETING", "欢迎访问 Runoob.com");
        echo GREETING;    // 输出 "欢迎访问 Runoob.com"
        echo '<br>';
        echo greeting;   // 输出 "greeting"
    ?>
    ```
* 魔术常量(也有叫'魔术变量'的)
    * 有八个魔术常量它们的值随着它们在代码中的位置改变而改变
    * 使用例子学习
        
        ```
        <?php
            namespace MyProject;
            //__LINE__
            echo '这是第 “ '  . __LINE__ . ' ” 行<br>';
            //__FILE__
            echo '该文件位于 “ '  . __FILE__ . ' ”<br>';
            //__DIR__
            echo '该文件位于 “ '  . __DIR__ . ' ” <br>'; //相当于dirname(__FILE__)
            //__FUNCTION__
            function test() {
                echo  '函数名为：' . __FUNCTION__ . '<br>';
            }
            test();
            //__CLASS__
            class test {
                function _print() {
                    echo '类名为：'  . __CLASS__ . "<br>";
                    echo  '函数名为：' . __FUNCTION__ .'<br>';
                }
            }
            $t = new test();
            $t->_print();
            //__TRAIT__
            class Base {
                public function sayHello() {
                    echo 'Hello ';
                }
            }
        
            trait SayWorld {
                public function sayHello() {
                    parent::sayHello();
                    echo 'World!';
                }
            }
        
            class MyHelloWorld extends Base {
                use SayWorld;
            }
        
            $o = new MyHelloWorld();
            $o->sayHello();
            //__METHOD__ 类似__FUNCTION__
            //__NAMESPACE__
        
            echo '命名空间为："', __NAMESPACE__, '"';
        ?>
        ```
        ```
        //***测试结果***
        这是第 “ 4 ” 行
        该文件位于 “ /opt/lampp/htdocs/test-php/index.php ”
        该文件位于 “ /opt/lampp/htdocs/test-php ”
        函数名为：MyProject\test
        类名为：MyProject\test
        函数名为：_print
        Hello World!命名空间为："MyProject"
        ```
    * 说明：Traits 是一种为类似 PHP 的单继承语言而准备的代码复用机制
## 字符串
* 并置运算符
    并置运算符 (.) 用于把两个字符串值连接起来
* strlen() 函数
* strpos() 函数
    用于在字符串内查找一个字符或一段指定的文本。如果在字符串中找到匹配，该函数会返回第一个匹配的字符位置。如果未找到匹配，则返回 FALSE。
* 测试strpos()函数
    
    ```
    <?php
        echo strpos("Hello world!","world");
    ?> 
    ```
* 与字符串相关函数的还有很多，可以查阅[PhpString参考手册](http://www.runoob.com/php/php-ref-string.html).其中比较常用的例如：
    * bin2hex()/hex2bin()
    * convert_uuencode()/convert_uudecode()
    * crc32()
    * crypt()
    * md5()/md5_file()

## 数组
* 分类
    * 索引数组
    * 关联数组
* 赋值方式
    
    ```
    <?php
        //索引数组1
        $cars=array("Volvo","BMW","Toyota");
        //索引数组2
        $arr[0]='苹果';
        $arr[1]='橘子';
        //关联数组1
        $age=array("Peter"=>"35","Ben"=>"37","Joe"=>"43");
        //关联数组2
        $age['Peter']="35";
        $age['Ben']="37";
        $age['Joe']="43";
    ?>
    ```
* 遍历
    
    ```
    //遍历数值数组
    <?php
        $cars=array("Volvo","BMW","Toyota");
        $arrlength=count($cars);
        
        for($x=0;$x<$arrlength;$x++)
        {
        echo $cars[$x];
        echo "<br>";
        }
    ?> 
    //遍历关联数组
    <?php
    $age=array("Peter"=>"35","Ben"=>"37","Joe"=>"43");
    
    foreach($age as $x=>$x_value)
    {
        echo "Key=" . $x . ", Value=" . $x_value;
        echo "<br>";
    }
    ?> 
    ```
* 排序
    * sort() - 对数组进行升序排列
    * rsort() - 对数组进行降序排列
    * asort() - 根据关联数组的值，对数组进行升序排列
    * ksort() - 根据关联数组的键，对数组进行升序排列
    * arsort() - 根据关联数组的值，对数组进行降序排列
    * krsort() - 根据关联数组的键，对数组进行降序排列

## 运算符
* 算术运算符
    php7新增整除运算符intdiv()，例如`intdiv(10, 3)`
* 赋值运算符
* 比较运算符
* 逻辑运算符
    * and, &&
    * or, ||
    * xor
    * !x
* 数组运算符(举例说明)
    
    ```
    <?php
        $x = array("a" => "red", "b" => "green");
        $y = array("c" => "blue", "d" => "yellow");
        $z = $x + $y; //数组合并
        var_dump($z);
        var_dump($x == $y); //相等
        var_dump($x === $y); //恒等
        var_dump($x != $y); //不等
        var_dump($x <> $y); //不等
        var_dump($x !== $y); //不恒等
    ?>
    ```
* 三元运算符
* 组合运算符(php7+)
    
    ```
    <?php
        // 整型
        echo 1 <=> 1; // 0
        echo 1 <=> 2; // -1
        echo 2 <=> 1; // 1
        
        // 浮点型
        echo 1.5 <=> 1.5; // 0
        echo 1.5 <=> 2.5; // -1
        echo 2.5 <=> 1.5; // 1
         
        // 字符串
        echo "a" <=> "a"; // 0
        echo "a" <=> "b"; // -1
        echo "b" <=> "a"; // 1
    ?>
    ```
## 结构语句
* if...else 
* switch
* while/do...while
* for/foreach

## 函数
> PHP 的真正威力源自于它的函数。在 PHP 中，提供了超过 1000 个内建的函数。

* 语法：
    
    ```
    function functionName()
    {
        要执行的代码;
    } 
    ```

## 命名空间
* 在声明命名空间之前唯一合法的代码是用于定义源文件编码方式的 declare 语句。所有非 PHP 代码包括空白符都不能出现在命名空间的声明之前.
    
    ```
    <?php
        declare(encoding='UTF-8'); //定义多个命名空间和不包含在命名空间中的代码
        namespace MyProject {
        
            const CONNECT_OK = 1;
            class Connection { /* ... */ }
            function connect() { /* ... */  }
        }
        
        namespace { // 全局代码
            session_start();
            $a = MyProject\connect();
        }
    ?>
    ```
* 子命名空间
    `namespace MyProject\Sub\Level;  //声明分层次的单个命名空间`
* 命名空间使用
    * 非限定名称，或不包含前缀的类名称`foo();`
    * 限定名称,或包含前缀的名称`subnamespace\foo();`
    * 完全限定名称，或包含了全局前缀操作符的名称`\Foo\Bar\foo();`
* 使用命名空间：别名/导入 
    * 注意PHP不支持导入函数或常量
    * 使用use操作符导入/使用别名
    * 一行中包含多个use语句 
    * 导入和动态名称
    * 导入和完全限定名称 

## 面向对象
* 语法
    
    ```
    <?php
        class phpClass {
          var $var1;
          var $var2 = "constant string";
          
          function myfunc ($arg1, $arg2) {
             [..]
          }
          [..]
        }
    ?>
    ```
    * 类的变量使用 var 来声明, 变量也可以初始化值
* php对象调用函数只能使用`->`, 例如`$runoob->getTitle(); `
* 构造函数
    `void __construct ([ mixed $args [, $... ]] )`
* 析构函数
    `void __destruct ( void ){}`
* 继承
    PHP 不支持多继承
* 调用父类构造函数
    PHP 不会在子类的构造方法中自动的调用父类的构造方法。要执行父类的构造方法，需要在子类的构造方法中调用 parent::__construct().
