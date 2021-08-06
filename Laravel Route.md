---
title: Laravel Command
date: 2021-08-06 18:50:36
tags:
---

1. #### 创建项目

   ```
   laravel new blog || composer create-project --prefer-dist laravel/laravel blog
   ```

2. 安装组件

   ```
   composer install
   ```

3. 刷新组件

   ```
   composer update
   ```

4. 删除组件

   ```
   composer remove chensuilong/toastr 
   composer dump-autoload
   ```

5. 查看artisan命令

   ```
   php artisan
   php artisan list
   ```

6. 启动PHP的Web服务

   ```
   php artisan serve
   ```

7. 查看某个帮助命令

   ```
   php artisan help make:model
   ```

8. 创建模型并创建新迁移

   ```
   php artisan make:model User --migration  
   ```

9. 查看laravel版本

   ```
   php artisan --version
   ```

10. 使用 PHP 内置的开发服务器启动应用

    ```
    php artisan serve
    ```

11. 生成一个随机的 key，并自动更新到 app/config/app.php 的 key 键值对（刚安装好需要做这一步）

    ```
    php artisan key:generate
    ```

12. 开启Auth用户功能（开启后需要执行迁移才生效）

    ```
    php artisan make:auth
    ```

13. 开启维护模式和关闭维护模式（显示503）

    ```
    php artisan down
    php artisan up
    ```

14. 进入tinker工具

    ```
    php artisan tinker
    ```

15. 列出所有的路由

    ```
    php artisan route:list
    ```

    

16. 生成路由缓存以及移除缓存路由文件

    ```
    php artisan route:cache
    php artisan route:clear
    ```

17. 重新生成签名

    ```
    php artisan passport:install
    ```

18. 自动生成Laravel密钥

    ```
    php artisan key:generate
    ```

19. Auth 系统

    ```
    php artisan make:auth
    ```

    

20. 创建控制器

    ```
    php artisan make:controller StudentController
    ```

    

21. 创建Rest风格资源控制器（带有index、create、store、edit、update、destroy、show方法）

    ```
    1. php artisan make:controller PhotoController --resource
    ```

    

22. 创建模型

    ```
    php artisan make:model User --migration  //创建模型并创建新迁移
    ```

    

23. 创建新建表的迁移和修改表的迁移

    ```
    php artisan make:migration create_users_table --create=students //创建students表
    php artisan make:migration add_votes_to_users_table --table=students//给students表增加votes字段
    ```

    

24. 执行迁移

    ```
    php artisan migrate
    php artisan migrate:rollback   //回滚最新一次迁移
    ```

    

25. 创建模型的时候同时生成新建表的迁移

    ```
    php artisan make:model Student -m
    ```

    

26. 回滚上一次的迁移

    ```
    php artisan migrate:rollback
    ```

    

27. 回滚所有迁移

    ```
    php artisan migrate:reset
    php artisan migrate:refresh  //更新表结构
    ```

    

28. 创建填充

    ```
    php artisan make:seeder StudentTableSeeder
    ```

    

29. 执行单个填充

    ```
    php artisan db:seed --class=StudentTableSeeder
    ```

    

30. 执行所有填充

    ```
    php artisan db:seed
    ```

    

31. 创建中间件（app/Http/Middleware 下）

    ```
    php artisan make:middleware Activity
    ```

    

32. 创建队列（数据库）的表迁移（需要执行迁移才生效）

    ```
    php artisan queue:table
    ```

    

33. 创建队列类（app/jobs下）：

    ```
    php artisan make:job SendEmail
    ```

    

34. 创建请求类（app/Http/Requests下）

    ```
    php artisan make:request CreateArticleRequest
    ```

    
