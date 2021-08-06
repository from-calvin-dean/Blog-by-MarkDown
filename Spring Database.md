---
title: Spring Database
date: 2021-08-06 18:50:36
tags:
---

1. #### 用idea创建spring boot项目选择mybatis,

   ### spring web
   
2. #### 设置application.properties

   ```properties
   #配置数据库的项目信息(MySQL数据库)
   #配置数据库的驱动
   spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
   
   # 旧版使用的驱动
   #spring.datasource.driver-class-name=com.mysql.jdbc.Driver
   
   
   spring.datasource.url=jdbc:mysql://127.0.0.1:3306/cms?characterEncoding=utf8&serverTimezone=UTC
   
   #配置数据库额度用户名
   spring.datasource.username=root
   
   #配置数据库的密码
   spring.datasource.password=123456
   
   # 配置服务器的相关信息(可选)
   # 配置服务器的端口
   server.port=80
   ```

3. ####  pom.xml 配置maven依赖 

   ```xml
    <!-- MYSQL -->
   <dependency>
       <groupId>mysql</groupId>
       <artifactId>mysql-connector-java</artifactId>
   </dependency>
   <!-- Spring Boot JDBC -->
   <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-jdbc</artifactId>
   </dependency>
   ```

   

4. #### 定义实体类

   ```java
   package org.springboot.sample.entity;
   
   public class Student {
       
       private int id;
       private String name;
       private String sex;
   
       //get set 方法省略
   }
   ```

   

5. #### 定义Service类

   ```java
   package com.example.chat.service;
   
   import com.example.chat.entity.Students;
   import org.springframework.jdbc.core.JdbcTemplate;
   import org.springframework.jdbc.core.RowMapper;
   import org.springframework.stereotype.Service;
   
   import java.sql.ResultSet;
   import java.sql.SQLException;
   import java.util.List;
   
   @Service
   public class StudentService {
       private final JdbcTemplate jdbcTemplate;
   
       public StudentService(JdbcTemplate jdbcTemplate) {
           this.jdbcTemplate = jdbcTemplate;
       }
   
       public  List<Students> getList(){
           String sql = "SELECT * FROM users";
           return (List<Students>)jdbcTemplate.query(sql, new RowMapper<Students>(){
               @Override
               public Students mapRow(ResultSet resultSet, int rowNum) throws SQLException {
                   Students stu = new Students();
                   stu.setName(resultSet.getString("name"));
                   stu.setSex(resultSet.getString("sex"));
                   return stu;
               }
           });
       }
   }
   
   ```

   

6. #### 定义Controller

   ```java
   package com.example.chat.controller;
   
   import com.example.chat.entity.Students;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.RestController;
   import com.example.chat.service.StudentService;
   
   import java.util.List;
   
   @RestController
   public class StudentController {
   
       private final StudentService studentService;
   
       public StudentController(StudentService studentService) {
           this.studentService = studentService;
       }
   	//或者通过ioc
       //@Autowired
       //private StudentService studentService;
       
       @RequestMapping("/stu")
       public List<Students>getStu(){
           return studentService.getList();
       }
   }
   ```

7. 最后就成功啦![]({{ site.url }}\assets\img\SpringBoot_jdbc.png)
