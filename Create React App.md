---
title: Create React App
date: 2021-08-06 18:50:36
tags:
---

1. #### 全局安装create-react-app(需要node环境)

   ```
   npm install -g create-react-app
   ```

   !!! 注意 : 这里面  -g   代表global也就是全局安装creat-react-app是react的脚手架

2. #### 使用脚手架创建react项目(命令行所在目录下直接创建)

   ```
   create-react-app 项目名称
   ```

3. #### 进入项目

   ```
   cd 项目名称
   ```

4. #### 运行项目

   ```
   npm start
   ```

5. #### 常用的依赖安装

   ```powershell
   npm install axios  //这是ajax组件
   npm install react-route-dom  //这是路由组件
   同理所有组件的安装方式都是如此
   ```

6. #### 安装方式npm install 、npm install --save 和 npm install --save-dev的区别

   ###### 相同点

   ​	三者都会本地安装包到项目的node_modules目录中

   ###### 区别

   ​	区别在于对项目package.json的修改，npm install不会修改package.json，而后两者会将依赖添加进package.json

   ###### `--save` 和`--save-dev`下载标签

   - 他们表面上的区别是--save 会把依赖包名称添加到 package.json 文件 dependencies 键下，--save-dev 则添加到 package.json 文件 devDependencies 键下.
   - dependencies是运行时依赖，devDependencies是开发时的依赖。即devDependencies 下列出的模块，是我们开发时用的
