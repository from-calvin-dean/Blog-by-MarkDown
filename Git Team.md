---
title: Git Team
date: 2021-08-06 18:50:36
tags:
---

1. #### 建立公钥

   ```powershell
   ssh-keygen -t rsa
   ```

2. #### 将公钥的内容设置到共用的github上

   使用下面命令验证一下如果遇到 yes/no 输入yes

   ```
   ssh -T git@github.com
   ```

   

3. #### 进行身份验证

   ```
   git config --global user.name "cokia-cinco"
   git config --global user.email "23177804@qq.com"
   ```

4. #### clone项目地址

   ```
   git clone https://github.com/cokia-cinco/myServer
   ```

5. #### 首先是单分支

   ```
   git add .
   git commit -m "Cin修改了xxx"
   git push origin master
   ```

   !!!这里是直接推送至主分支

6. #### 过几天增加一名美女cokia重复上述步骤

   这里面会这几到问题,当cokia进行提交以后,Cin再次push就会报错.

   解决方法是拉取master被变更过的内容

   ```
   git pull origin master
   ```

   这时会让你填写merge的原因,填写保存即可.

7. #### 现在执行push就可以了

   ```
   git push origin master
   ```

8. #### 下面是创建分支

   ```
    git checkout -b/git branch 分支名(假设为"cokia")
   ```

   这里面的git checkout -b也作为选择分支命令

   现在我们进行一部分提交操作

   ```
   git branch					# 查看当前的分支
   git add 功能A.txt	           # 将写好的功能A代码add
   git commit -m"功能A第一版"    # 将写好的功能A第一版代码commit
   git add 功能A.txt            # 将写好的功能A代码add
   git commit -m"功能A第二版"    # 将写好的功能A第二版代码commit
   git push origin 当前分支	 # 将内容推送到当前分支
   ```

   

9. #### 进行推送的时候需要进入master

   ```
   git checkout master                                     # 回到master分支
   git merge --no-ff -m "merge 功能A with --no-ff" 分支名   # 在master上执行merge操作
   git push origin master 	
   ```

     git merge 带上 --no-ff 参数会保存分支的历史信息。如果不加，merge后就看不到分支信息了。

10. #### Cin合并后cokia也进行了同样的操作发现可以进行merge，但是没法进行push

    原因和 第6步相同需要先pull一下

11. #### 查看历史版本

    ```
    git log --graph --pretty=oneline --abbrev-commit
    ```

12. #### 下面就可以自由穿越了

    ```
    git reset --hard commit 版本号
    ```

    ##### 												本文针对同公司共用一个账号,谨记!!!
