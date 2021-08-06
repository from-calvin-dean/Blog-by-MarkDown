---
title: Laravel Create
date: 2021-08-06 18:50:36
tags:
---

1. #### composer安装laravel安装环境

   ```
   //换阿里源,国内使用composer官方源太慢
   composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/
composer global require laravel/installer
   ```

2. 把composer的composer/vendor/bin/加入系统环境变量

3. 现在就可以使用laravel各种new项目了

   ```
    laravel new LaravelBasic
   ```

4. 模拟启动为(默认localhost:8000端口)

   ```
   php artisan serve
   //其他端口
   php artisan serve --port=xxxx
   ```

