---
title: PHP SMTP E-mail
date: 2021-08-06 18:50:36
tags:
---

1. #### 登录网易邮箱/qq邮箱使用smtp服务

2. 下载phpmailer官方sdk并上传extend目录

3. 引入头

   ```php
   use phpmailer\Smtp;
   use phpmailer\Phpmailer;
   ```

4. 封装发送邮件的函数

   ```php
   //$config	发送需要的配置
   //$toEmail	接受人的邮件地址
   //$data		包含标题和正文
   function sendEmail($config,$toEmail,$data)
   {
       $mail = new  Phpmailer;
       $mail->isSMTP();// 使用SMTP服务
       $mail->CharSet = "utf8";// 编码格式为utf8，不设置编码的话，中文会出现乱码
       $mail->Host = 'smtp.163.com';// 发送方的SMTP服务器地址
       $mail->SMTPAuth = true;// 是否使用身份验证
       $mail->Username = $config['email'];/// 发送方的163邮箱用户名，就是你申请163的SMTP服务使用的163邮箱
       $mail->Password = $config['email_code'];// 发送方的邮箱密码，注意用163邮箱这里填写的是“客户端授权密码”而不是邮箱的登录密码！
       $mail->SMTPSecure = "ssl";// 使用ssl协议方式
       $mail->Port = 994;// 163邮箱的ssl协议方式端口号是465/994
       try {
           $mail->setFrom($config['email'], $config['cname']);
       } catch (\PHPMailer\phpmailerException $e) {
           return false;
       }// 设置发件人信息，如邮件格式说明中的发件人，这里会显示为Mailer(xxxx@163.com），Mailer是当做名字显示
       $mail->addAddress($toEmail, 'Wang');// 设置收件人信息，如邮件格式说明中的收件人，这里会显示为Liang(yyyy@163.com)
       $mail->addReplyTo($config['email'], "Reply");// 设置回复人信息，指的是收件人收到邮件后，如果要回复，回复邮件将发送到的邮箱地址
       $mail->addCC($config['cemail']);// 设置邮件抄送人，可以只写地址，上述的设置也可以只写地址(这个人也能收到邮件)
       //$mail->addBCC("xxx@163.com");// 设置秘密抄送人(这个人也能收到邮件)
       //$mail->addAttachment("bug0.jpg");// 添加附件
   
       $mail->SMTPOptions = array(
           'ssl' => array(
               'verify_peer' => false,
               'verify_peer_name' => false,
               'allow_self_signed' => true
           )
       );
       $mail->Subject = $data['title'];// 邮件标题
       $mail->Body = $data['suggestions'];// 邮件正文
       if (!$mail->send()) {
           return false;
       } else {
           return true;
       }
   }
   ```

   
