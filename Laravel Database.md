---
title: Laravel Database
date: 2021-08-06 18:52:59
tags:
---

#### NO.1: 传统模式(与原生sql无太大区别)

- 查询操作

  ```sql
  $data = DB::select("select * from user");
  //返回一个二维数组 $data
  ```

  

- 插入操作

  ```sql
  $data = DB::insert("insert into user(name,sex,age) values(?,?,?,?)",
  ['小明','女',670])
  //成功会返回true
  ```

- 更新操作

  ```sql
  //参数一为sql语句,参二为前面对应的字段的值
  $data = DB::update("update `user` set `age`='18' where ID = 1",[18,1]);
  //成功会返回true
  ```

- 删除操作

  ```php
  //参数一为sql语句,参数二为对应的值字段值,一定要有where语句,否则会清空数据库!!!
  $data = DB::delete("delete from user where ID = 1",[1])
  //成功会返回非0的数
  ```

  

#### NO.2 采用laravel的内置DB操作类( 经典DB模式 )

- 查询操作

  ```sql
  //get()会返回所有数据相当于 *
  $data = DB::table('user')->get();
  //first()会返回单条数据
  $data = DB::table('user')->first();//返回第一条数据
  //id倒序第一条
  $data = DB::table('user')->orderBy('ID','desc')->->first();
  //查找id大于2的所有数据
  $data = DB::table('user')->where('ID','>',2)->get();
  //指定字段,后面无需get()
  $data = DB::table('user')->pluck('name');
  //list()中可将指定字段作为下标
  $data = DB::table('user')->list('name','id');
  // select()指定某个字段
  $data=DB::table("user")->select('name','ID')->get();
  // chunk()每次查n条
  $data = DB::table("user ")->chunk(2,function($user){  //每次查2条
      var_dump($user);
      if(.......) return false;  //在满足某个条件下使用return就不会再往下查了
  });
  #使用聚合函数
  //count()统计记录条数
  $num = DB::table("user")->count();
  // max()某个字段的最大值,同理min是最小值
  $max = DB::table("user")->max("age");
  echo $max;
  // avg()某个字段的平均值
  $avg = DB::table("user")->avg("age");
  echo $avg;
  // sum()某个字段的和
  $sum = DB::table("user")->sum("age");
  echo $sum;
  ```

- 插入操作

  ```sql
  $data = DB::table("user")->insert(['name'=>'小花','sex'=>'女','age'=>20]);
  //返回的为bool值
  $id = DB::table("user")->insertGetId(['name'=>'小花','sex'=>'女','age'=>20]);
  //返回新增id
  
  #插入多条
  $bool=DB::table("user")->insert([
          ['name'=>'小明','sex'=>'女','age'=>20],
          ['name'=>'小红','sex'=>'男','age'=>21],
  ]);//返回bool值
  ```

- 更新操作

  ```sql
  //更新操作的所有返回值皆为bool值
  $bool=DB::table("user")->where('ID',6)->update(['age'=>30]);
  #自增
  $bool=DB::table("user")->where('ID',6)->increment("age");//年龄加1
  $bool=DB::table("user")->where('ID',6)->increment("age",3);// 年龄加3
  #自减
  $bool=DB::table("user")->where('ID',6)->decrement("age");// 年龄减1
  $bool=DB::table("user")->where('ID',6)->decrement("age",3);// 年龄减3
  #自增(减)同时进行其他操作
  $bool=DB::table("user")->where('ID',6)->increment("age",3,['name'=>'小美']);
  // 年龄加3 名字改为小强
  ```

- 删除操作

  ```sql
  //所有的删除操作一定要注意where条件!!!
  $num=DB::table("user")->where('ID',6)->delete();// 删除1条
  $num=DB::table("user")->where('ID','>',4)->delete();// 删除多条
  echo $num;  //删除的行数
  $num=DB::table("user")->truncate();// 删除整表，不能恢复，谨慎使用
  ```

  #### NO.3 laravel之精华版--Eloquent ORM(为laravel操作数据库核心,主推)

  1. 模型的建立

     laravel所自带的Eloquent ORM 是一个ActiveRecord实现，用于数据库操作。每个数据表都有一个与之对应的模型，用于数据表交互。
      建立模型，在app目录下建立一个user模型，由于看习惯了mvc所以先建立一个Models文件夹在里面建立user.php，目录结构变成 app/Models/user.php

     ```php
     namespace App\Models;
     use Illuminate\Database\Eloquent\Model;
     class User extends Model
     {
        //表名的绑定
         protected $table = 'e_user';
         //绑定主键为'id',(默认绑定的主键为'id',如主键非id则必选)
         protected $primarykey = 'id';
         //开启时间戳,如不使用则设为false
         public $timestamps = true;
         
         if($timestamps){
             protected function getDateFormat(){
                 return time();
             } 
         }
         // 允许批量赋值的字段
         protected $fillable=['name','age','sex'];
         // 定义隐藏的字段
         protected $hidden = ['updated_at','created_at','deleted_at'];
     }
     ```

  2. 控制器对模型并使用ORM的DB操作

     ```php 
     //需要导入ORM所在的模型
     public function test(){
           // all()方法查询所有数据
           $users=User ::all();
           dd($users);
           //find()查询一条，依据主键查询。findOrFail()查找不存在的记录时会抛出异常
           $user=User ::find(5);  //主键为5的记录
           var_dump($user);
           //查询构造器的使用,省略了指定表名
           $user=User::get();  
           var_dump($user);
           //使用leftjoin 进行查询
           Banner::where($where)
           ->leftJoin("c_img_url", function ($join){
                // 右表和左表相对应的字段，可以有多个右表，直接把这一句复制改一下表名字段名就行了
                $join->on("c_img_url.id", "=", "c_banner.img_url_id"); 
           })->get(array('c_banner.*','c_img_url.address','c_img_url.name'))
         
     }
     ```

  3. 新增数据,自定义时间戳,批量赋值

     laravel会默认维护created_at,updated_at 两个字段，这两个字段都是存储时间戳，整型11位的，因此使用时需要在数据库添加这两个字段。如果不需要这个功能，只需要在模型里加一个属性：public $timestamps=false; 以及一个方法，可以将当前时间戳存到数据库

     ```php
     （1）使用save方法新增
     $student=new Student();
     //设定数据
     $student->name='xiaoming';
     $student->age='18';
     $student->sex='女';
     $bool=$student->save();  //保存
     echo $bool;
     
     （2）使用create方法新增时，需要在模型里增加：
      //允许批量赋值的字段
     protected $fillable=['name','age','sex'];  
     // 在控制器中使用
     Student::create(['name'=>'mmm','age'=>23,'sex'=>'女']);
     // 这样即可新增成功！
     
     （3）firstOrCreate()以属性查找记录，若没有则新增
     代码执行顺序：先查找表中是否有name=phphub-monkey的，如果有则不新增，没有就新增
     第一个参数用于确认 Eloquent 对象是否存在；
     第二个参数用于生成 Eloquent 对象时需要附加生成的属性。
     $user = User::firstOrCreate(
         ['name' => 'phphub-monkey'],
         [
             'email' => 'jin114001251@gmail.com',
             'age' => '23',
             'sex' => 'nv',
         ]
     ]);
     
     （4）firstOrNew()以属性查找记录，若没有则会创建新的实例。若需要保存，则自己调用save方法()
     和firstOrCreate类似只不过需要自己调用save()
     $user=User::firstOrNew(['name'=>'小强']);
     $user->save();
     echo $user;
     ```

  4. 更新数据

     ```php
     //通过模型更新数据
     $user=User::find(2); // 修改主键为2的数据
     $user->age=25;
     $user->save(); //返回bool值
     //通过查询构造器更新
     $num=User::where('ID','>',2)->update(['age'=>20]);
     echo $num;  //返回更新的行数
     
     updateOrCreate如果不存在就创建（和firstOrNew方法类似）
     // 名字为小明的行是否存在，不存在就创建
     User::updateOrCreate(
         ['name' => '小明'],
         [
             'age' => 20,
             'sex' => '女',
         ]);
     
     ```

  5. 删除数据

     ```php
     (1) 通过模型删除数据
     $user=User::find(11);
     $user->delete(); //返回bool值
     
     (2) 通过主键删除
      $num=User::destroy(10); //删除主键为10的一条记录
      echo $num; //返回删除的行数
      $num=User::destroy(10,5); //删除多条  或者$num=Student::destroy([10,5]);
      echo $num; //返回删除的行数
     ```

     
