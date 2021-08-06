---
title: React Route
date: 2021-08-06 18:51:34
tags:
---

1. #### 首先我们要安装路由组件

   ```
   npm install react-route-dom
   ```

2. #### 在需要路由的地方使用import导入

   ```javascript
   import React, {Component} from 'react'
   import {HashRouter,Switch,Route} from 'react-router-dom'
   ```

3. #### render()下返回组件

   ```javascript
   import User from '../User'
   import Welcome from '../Welcome'
   class App extends Component{
       render(){
           return(
               <HashRouter>
                   <Switch>
                       <Route exact path={"/"}><Welcome/></Route>
                       <Route path={"/user"}><User/></Route>
                   </Switch>
               </HashRouter>
           )
       }
   }
   export default App
   ```

   其中的switch表示选择只加载其中之一

4. #### 使用Link链接测试即可

   ```javascript
   import React, {Component} from 'react'
   import {Link} from "react-router-dom"
   class Welcome extends Component{
       render() {
           return (
               <div>
                   <div>
                       欢迎您^v^
                   </div>
                   <div>
                       <Link to={'/user'}>点击进入</Link>
                   </div>
               </div>
           )
       }
   }
   export default Welcome
   ```

   
