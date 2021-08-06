---
title: PHP Ali Message
date: 2021-08-06 18:50:36
tags:
---

1. function sendMsg($config,$mobile,$code)
   {
       $accessKeyId = $config['accessKeyId'];  //阿里云短信获取的accessKeyId

       $accessKeySecret = $config['accessKeySecret'];    //阿里云短信获取的accessKeySecret
       
       //这个个是审核过的模板内容中的变量赋值，记住数组中字符串code要和模板内容中的保持一致
       //比如我们模板中的内容为：你的验证码为：${code}，该验证码5分钟内有效，请勿泄漏！
       
       $signName = $config['signName']; //这个是短信签名，要审核通过
       
       $templateCode = $config['templateCode'];   //短信模板ID，记得要审核通过的
       
       AlibabaCloud::accessKeyClient($accessKeyId,$accessKeySecret)
           ->regionId('cn-hangzhou')
           ->asDefaultClient();
       
       try {
           $result = AlibabaCloud::rpc()
               ->product('Dysmsapi')
               // ->scheme('https') // https | http
               ->version('2017-05-25')
               ->action('SendSms')
               ->method('POST')
               ->host('dysmsapi.aliyuncs.com')
               ->options([
                   'query' => [
                       'RegionId' => "cn-hangzhou",
                       'PhoneNumbers' => $mobile,
                       'SignName' => $signName,
                       'TemplateCode' => $templateCode,
                       'TemplateParam' => json_encode(['code'=>$code]),
                   ],
               ])
               ->request();
           return $result->toArray();
       } catch (ClientException $e) {
           echo $e->getErrorMessage() . PHP_EOL;
       } catch (ServerException $e) {
           echo $e->getErrorMessage() . PHP_EOL;
       }
   }
