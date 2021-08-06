---
title: React Axios
date: 2021-08-06 18:52:08
tags:
---

1. #### 导入axios以及qs文字包(否则中文会有乱码现象)

   ```javascript
   import axios from "axios";
   import qs from 'qs';
   ```

2. #### 创建自己的ajax函数,参数依次为(get/post,地址,数据,成功回调函数,头文件含默认)

   ```javascript
   let ajax=function (method,url,data,cb,head= "application/x-www-form-urlencoded") {
   
   }
   ```

3. #### 判断是否为form表单传输数据

   ```javascript
   //判断content type 是什么类型,若是form-data 则不转换成字符串 
   if(head!=="multipart/form-data"){
           data = qs.stringify(data)
    }
   ```

4. #### 调用axios方法,假设使用本地8000端口(会有跨域问题,需要自己设置)

   ```javascript
   axios({
           method:method,
           url:"http://localhost:8000/"+url,
           headers:{
               "Content-Type":head
           },
       	//超时设置单位为ms
           timeout: 10000,
           //上传文件不需要解析data,而普通需要解析变量
           data:data
       }).then((response)=>{
       	//成功会调用回调方法
           cb(response.data)
       }).catch((error)=>{
       	//报错会直接打印
           console.log(error)
       })
   ```

5. #### 总体代码如下

   ```javascript
   import axios from "axios";
   import qs from 'qs';
   let ajax=function (method,url,data,cb,head= "application/x-www-form-urlencoded") {
       //判断content type 是什么类型,若是form-data 则不转换成字符串
       if(head!=="multipart/form-data"){
           data = qs.stringify(data)
       }
       axios({
           method:method,
           url:"http://localhost:8000/"+url+token,
           headers:{
               "Content-Type":head
           },
           timeout: 10000,
           //上传文件不需要解析data,而普通需要解析变量
           data:data
       }).then((response)=>{
       	cb(response.data)
       }).catch((error)=>{
           console.log(error)
       })
   }
   export  default ajax
   ```

6. #### 下面为使用方法

   ```
   import React, {Component} from 'react'
   import ajax from "../AJAX";
   class Welcome extends Component{
       componentDidMount() {
           ajax('get','api/get_csrf_token','',(data)=>{
               console.log(data)
           })
       }
       render() {
           return (
               <div>
                   <div>
                       欢迎进入点餐团队下面是本次的安排
                   </div>
               </div>
           )
       }
   }
   export default Welcome
   ```

   
