---
title: Responsive Layout
date: 2021-08-06 18:46:56
tags:
---

1. #### 建立三个css文件用作不同的屏幕显示的效果

2. #### 用其中一个css为基础写完最初版本

3. #### 使用link引入

   ```html
   <!--这个是标准版本,以它为基础-->
   <link type="text/css" rel="stylesheet" href="../css/tencent.css"/>
   <!--这是当屏幕宽小于1300px的版本,注意media中的screen指的是电脑端屏幕-->
   <link type="text/css" rel="stylesheet" href="../css/min.css" media="screen and (max-width:1300px)" />
   <!--这是当屏幕宽小于1000px的版本-->
   <link type="text/css" rel="stylesheet" href="../css/mins.css" media="screen and (max-width:1000px)" />
   ```

   

##### !!!注意 : 基础版本是一直加载的,所以另外两个我们只需要写需要变更的stylesheet.
