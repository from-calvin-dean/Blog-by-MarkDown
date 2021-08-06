---
title: Mac Homebrew Installer
date: 2021-08-06 18:50:36
tags:
---

1. #### 执行命令

   ```shell
   /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
   ```

2. 如果有问题就是ip混淆了,需要寻找 raw.githubusercontent.com 的真实地址,更新到

   ```
   sudo vi /etc/hosts
   ```

   $$
   ##
   # Host Database
   #
   # localhost is used to configure the loopback interface
   # when the system is booting.  Do not change this entry.
   ##
   127.0.0.1       localhost
   255.255.255.255 broadcasthost
   ::1             localhost
   
   #追加这一条
   199.232.96.133 raw.githubusercontent.com
   $$

3. 如果下载很慢的话,直接使用control + c 停止更换链接地址为国内

   ```
   /bin/zsh -c "$(curl -fsSL https://gitee.com/cunkai/HomebrewCN/raw/master/Homebrew.sh)"
   ```

   
