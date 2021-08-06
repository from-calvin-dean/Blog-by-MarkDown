---
title: Laravel Cross
date: 2021-08-06 18:50:36
tags:
---

1. #### 1.建立中间件

   ```
   php artisan make:middleware CrossHttp
   ```

   

   #### 2.写如跨域头代码

   ```php
   <?php
   namespace App\Http\Middleware;
   use Closure;
   
   class CrossHttp
   {
   
       public function handle($request, Closure $next)
       {
           $response = $next($request);
           $response->header('Access-Control-Allow-Origin', '*');
           $response->header('Access-Control-Allow-Headers', 'Origin, Content-Type, Cookie, Accept');
           $response->header('Access-Control-Allow-Methods', 'GET, POST, PATCH, PUT, OPTIONS');
           return $response;
       }
   }
   ```

   

   #### 3. App/Http/Kernel 进行中间件加入(可选全局,指定路由)

   ```php
   //全局:
   	\App\Http\Middleware\CrossHttp::class
   //指定路由:
   	'auth.api' => \App\Http\Middleware\CrossHttp::class,
   ```

   运行如有错误,目录下执行composer dump-autoload即可
