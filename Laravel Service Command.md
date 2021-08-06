---
title: Laravel Service Command
date: 2021-08-06 18:50:36
tags:
---

1. #### 新建command命令

   ```
   php artisan make:command AddService
   ```

   ###### 执行该命令，将会在app\Console目录下生成Commands目录，同时在 app\Console\Commands 目录下生成 AddService.php 文件。

2. #### 编辑services.stub并添加内容 将如下内容添加到services.stub文件中并保存。

   ```php
   <?php
   
   namespace DummyNamespace;
   
   class DummyClass 
   {
   
   }
   ```

3. #### 编辑AddService.php并添加内容

   ```php
   <?php
   
   namespace App\Console\Commands;
   
   use Illuminate\Console\GeneratorCommand;
   
   class AddServices extends GeneratorCommand
   {
       /**
        * 控制台命令名称
        *
        * @var string
        */
       protected $name = 'make:service';
       /**
        * 控制台命令描述
        *
        * @var string
        */
       protected $description = 'Create a new service  class';
       /**
        * 生成类的类型
        *
        * @var string
        */
       protected $type = 'Services';
       /**
        * 获取生成器的存根文件
        *
        * @return string
        */
       protected function getStub()
       {
           return __DIR__.'/Stubs/services.stub';
       }
   
       /**
        * 获取类的默认命名空间
        *
        * @param  string  $rootNamespace
        * @return string
        */
   
       protected function getDefaultNamespace($rootNamespace)
       {
           return $rootNamespace.'\Services';
       }
   }
   ```

4. #### 注册命令(将以下内容添加到Kernel.php文件的 `protected $commands = []` 数组)

   ```
   protected $commands = [
       //
       \App\Console\Commands\AddServices::class
   ];
   ```

5. #### 测试命令

   ```
   php artisan make:service UserService
   ```

   

