---
title: Laravel Validator
date: 2021-08-06 18:50:36
tags:
---

1. #### 创建一个验证的控制器

   ```powershell
   php artisan make:controller getValidator
   ```

2. #### 创建一个统一的方法去返回验证结果

   ```php
    /**
        * @var 验证是否有误
        */
       protected  $validator;
       protected  function vd(){
           if ($this->validator->fails()){
               return false;
           }
           return true;
       }
   ```

3. #### 为需要验证的数据创建方法

   ```php
    public function showFriend(Request $request){
        //调用方法时会触发Validator的make方法
        //数组中为传入的值的验证
           $this->validator = Validator::make($request->all(),[
               'id'=>'required|max:11',
           ]);
           return $this->vd();
       }
   ```

   验证的详情见laravel的validation

4. #### 使用验证类

   ```php
   class FriendController extends Controller
   {
       protected $validator;
   
       public function __construct()
       {
           //验证类实例
           $this->validator = new getValidator;
       }
       
   	public function friend(Request $request)
       {
           if (!$this->validator->showFriend($request)) {
              echo "未设置有效id";
           }
       }
   }
   ```

   
