---
title: Node.js Socket
date: 2021-08-06 18:52:59
tags:
---

### Server端:

1. #### 导入net模块

   ```javascript
   const net = require('net')
   let client =[]//保存链接的用户
   let exits//用来更新退出的用户
   ```

2. #### 创建一个socket,并且监听10086端口

   ```js
   const server = net.createServer((socket) => {
       
   })
   server.listen({
       port:10086,
   })
   ```

3. #### 从socket种获取信息,判断是否为新用户,是否为单体发送,是否为退出,总体代码如下

   ```javascript
   const net = require('net')
   let clients=[]
   let exits
   const server = net.createServer((socket) => {
       socket.on('data',function(data){
           //获得客户端信息,判断
           var res = data.toString()
           var result = celints(res)
           //判断是用户名还是信息
           if(result){
               //存入对象
               user_obj(res,socket)
           }else{
             //  发送信息,单发或群发
             send_users(res)
           }
       })
       //当退出时,删除这个对象的链接
       socket.on('end',function(end){
           console.log(`${this.clientName}正在`+'exit')
           for (var i=0;i<clients.length;i++){
               if (clients[i].clientName==this.clientName){
                   clients.splice(i,1)
               }
           }
           exits = this.clientName+'退出成功'
           console.log(clients.length)
           console.log(exits)
       })
       //报错时所发的信息
       socket.on('error',function(error){
           socket.end()
       });
   })
   server.listen({
       port:10086,
   })
   //获得客户端信息
   function celints(res) {
       var result = res.substr(0,5)=='login'?true:false
       return result
   }
   //存入客户端对象
   function user_obj(res,socket){
       socket.clientName=res.split(':')[1]
       clients.push(socket)
       for (var i=0;i<clients.length;i++) {
           clients[i].write(res.split(':')[1]+'上线了')
       }
       console.log(res)
   }
   //群体发消息
   function send_users(res) {
       //判断是否为@语句
       var preg = /.*:@.*/
       var res2 = preg.test(res)
       if(res2){
           //捕捉@的人
           var username = res.split('@')[1].split(' ')[0]
           console.log(username)
       }
       //全屏发送消息
       for (var i=0;i<clients.length;i++){
           //判断正则匹配是否有@符存在
           if (res2 && clients[i].clientName==username){
               clients[i].write(res)
           }else if(!res2){
               clients[i].write(res)
           }
       }
   }
   ```

## Client端:

1. #### 导入net模块(上第一行同)

   ```javascript
   const net = require('net')
   var port = 10086;//链接的端口
   var host = '192.168.1.102';//链接的地址
   var socket= new net.Socket();
   let name = ''//用来保存nickname
   ```

2. #### 因为客户端需要在发送消息的同时接受消息所以要异步输入输出

   ```javascript
   var readline = require('readline')//导入readline模块
   const re = readline.createInterface({
       input:process.stdin,
       output:process.stdout
   })//建立异步输入输出接口
   ```

3. #### 下面就简单了,根据server的规定写socket即可

   ```javascript
   socket.setEncoding('utf-8');
   // 连接到服务端
   socket.connect(port,host,function(){
       //获得名字
       re.question('请输入你的名字？', (answer) => {
           name = answer
           socket.write(`login:${answer}`)
           console.log('登陆成功')
       });
       //输入流
       re.on('line',function(line) {
           var tokens = line
           socket.write(`${name}:${tokens}`)
           if(tokens=='bye'){
               socket.end(name)
           }
       })
   });
   //输出流
   socket.on('data',function(data){
       console.log(data.toString());
   })
   socket.on('close',function(){
       console.log('Connection closed')
   })
   socket.on('error',function(error){
       console.log('error:'+error);
   });
   socket.on('end', ()=>{
       console.log('ended');
   });
   ```

   
