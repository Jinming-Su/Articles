#PHP进阶

##多维数组
实例
    
    ```
     <?php
        $sites = array
        (
            "runoob"=>array
            (
                "菜鸟教程",
                "http://www.runoob.com"
            ),
            "google"=>array
            (
                "Google 搜索",
                "http://www.google.com"
            ),
            "taobao"=>array
            (
                "淘宝",
                "http://www.taobao.com"
            )
        );
        print("<pre>"); // 格式化输出数组
        print_r($sites);
        print("</pre>");
    ?> 
    ```
    ```
    //***测试结果***
    Array
    (
        [runoob] => Array
            (
                [0] => 菜鸟教程
                [1] => http://www.runoob.com
            )
    
        [google] => Array
            (
                [0] => Google 搜索
                [1] => http://www.google.com
            )
    
        [taobao] => Array
            (
                [0] => 淘宝
                [1] => http://www.taobao.com
            )
    
    )   
    ```

##日期date()函数
* date() 函数用于格式化时间/日期
* 语法
    `string date ( string $format [, int $timestamp ] )`
* 实例
    
    ```
    <?php
        echo date('Y/m/d') . '<br>';  //输出2016/10/21
    ?>
    ```

##包含文件 include 和 require 语句
区别
* require 生成一个致命错误（E_COMPILE_ERROR），在错误发生后脚本会停止执行.
* include 生成一个警告（E_WARNING），在错误发生后脚本会继续执行.

##文件处理
* 打开文件
    `<?php $file=fopen("welcome.txt","r");?>`
* 关闭文件
    `<?php fclose($file);?>`
* 检测文件末尾
    `<?php if(feof($file)) echo '文件末尾';?>`
* 逐行读取文件
    
    ```
    <?php
        $file = fopen('test.txt', 'r');
        while(!feof($file)) {
            echo fgets($file) . '<br>';
        }
        fclose($file);
    ?>
    ```
* 逐字符读取文件fgetc()

##文件上传
* 创建上传脚本(在次之前需要有一个提交到此页的带有file的表单)
    
    ```
    <?php

        if($_FILES['file']['error'] > 0) {
            echo "error:" . $_FILES['file']['error'] . '<br>';
        } else {
            echo $_FILES['file']['name'] . '<br>';
            echo $_FILES['file']['type'] . '<br>';
            echo ($_FILES['file']['size']/1024)  . 'kB<br>';
            echo $_FILES['file']['tmp_name'] . '<br>';
        }
    ?>
    ```
    ```
    //***测试结果***
    ps.desktop
    application/x-desktop
    0.4501953125kB
    /opt/lampp/temp/phpjZOWV5
    ```
* 保存被上传的文件
    
    ```
    if(file_exists('upload/' . $_FILES['file']['name'])) {
        echo 'existed';
    } else {
        move_uploaded_file($_FILES['file']['tmp_name'], 'upload/' . $_FILES['file']['name']);
    }
    ```

##Cookie
* 创建cookie语法
    `setcookie(name, value, expire, path, domain);`
    * name（ Cookie名）可以通过$_COOKIE[‘name’] 进行访问
    * value（Cookie的值）
    * expire（过期时间）Unix时间戳格式，默认为0，表示浏览器关闭即失效
    * path（有效路径）如果路径设置为’/’，则整个网站都有效
    * domain（有效域）默认整个域名都有效，如果设置了’www.sjming.com’,则只在www子域中有效 
    * 注意：setcookie() 函数必须位于 <html> 标签之前，否则无效
* 删除cookie
    `setcookie('test','', time()-1); `

##Session
* 开始 PHP Session
    `<?php session_start(); ?>`
    * session_start() 函数必须位于 <html> 标签之前
* 存储session变量
    `$_SESSION['test'] = 1;`
* 销毁session
    * unset() 函数用于释放指定的 session 变量 `unset($_SESSION['views']);`
    *  session_destroy() 函数彻底销毁 session `session_destroy();`

##E-Mail
* 语法
    `mail(to,subject,message,headers,parameters`
* 具体测试参照:https://github.com/su526664687/Articles/blob/master/2016/11/17.1.phpmailer.md

##错误处理
* die()
    
    ```
    <?php
        if(!file_exists('welcome.txt')) {
            die('文件不存在');
        } else {
           $file = fopen('welcome.txt', 'r');
        }
    ?>
    ```
    die(status),如果 status是字符串，则该函数会在退出前输出字符串,如果 status是整数，这个值会被用作退出状态。退出状态的值在 0 至 254 之间。
* 自定义错误处理器
    * 语法
    `error_function(error_level,error_message,error_file,error_line,error_context)`
    * 使用set_error_handler('xxx')让错误处理器处理所有的错误,第二个参数控制错误级别
    
        ```
        <?php
            function customError($errno, $errstr) {
                echo "<b>Error:</b> [$errno] $errstr";
            }
            set_error_handler("customError");
            // 触发错误
            echo($test);
        ?>
        ```
        ```
        //测试结果
        Error: [8] Undefined variable: test
        ```
    * 触发错误
        
        ```
        <?php
            function customError($errno, $errstr)
            {
                echo "<b>Error:</b> [$errno] $errstr<br>";
                echo "脚本结束";
                die();
            }
            set_error_handler("customError",E_USER_WARNING);
            $test=2;
            if ($test>1)
            {
                trigger_error("变量值必须小于等于 1",E_USER_WARNING);
            }
        ?>
        ```
        ```
        //测试结果
        Error: [512] 变量值必须小于等于 1
        脚本结束
        ```
##异常处理
##过滤器
//这部分希望在以后用到的时候进行补充
##JSON
