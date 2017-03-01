#使用phpmailer发邮件
---
##准备
* 需要在github上的phpmailer项目中下载class.phpmailer.php和class.smtp.php.

##实例

  ```
  <?PHP
    require_once("class.phpmailer.php");
    require_once("class.smtp.php");
    $mail = new PHPMailer();

    $mail->SMTPDebug = 1;

    $mail->isSMTP();
    $mail->SMTPAuth= true;
    $mail->Host = 'smtp.163.com';
    $mail->SMTPSecure = false;
    $mail->Port = 25;
    //$mail->Helo = 'Hello smtp.163.com Serve';
    //$mail->Hostname = '0.0.0.0';
    $mail->CharSet = 'UTF-8';
    $mail->FromName = 'Sjming';
    $mail->Username ='sujinming0125@163.com';
    $mail->Password = 'xxxx';
    $mail->From = 'sujinming0125@163.com';
    $mail->isHTML(false);
    $mail->addAddress('526664687@qq.com','Sjming');
    //添加该邮件的主题
    $mail->Subject = '测试307835526';
    $mail->Body = "123456";

    $status = $mail->send();

    //简单的判断与提示信息
    if($status) {
        echo '发送邮件成功';
    }else{
        echo '发送邮件失败，错误信息未：'.$mail->ErrorInfo;
    }
  ?>
  ```

* 以上实例中的每个部分都比较容易理解，不过多解释
* 测试时间:2016.11.17日
* 缺陷： 经过测试,在163,qq和sina邮箱是可用的，gmail应该也可以，outlook和hotmail失败
