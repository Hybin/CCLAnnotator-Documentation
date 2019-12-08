用户管理
=================

针对用户管理系统，其涉及到三个方面：数据库、数据验证和页面呈现。三者刚好对应MVC架构：以模型（Model）操作数据库；\
以控制器（Controller）管理数据验证；以视图（View）渲染页面。在 Laravel 框架中，用户模型的路径为 ``/CCLAnnotator/app/Models/User.php`` 。\
``User.php`` 设定了数据表名称和相关字段名称。与之相关的数据迁移操作，参看 `文档 <https://learnku.com/docs/laravel/5.5/migrations/1329>`_，\
此处不作赘述。

用户模型的存在，使开发者能够方便地操作数据库，参数化查询的模式，直接规避了数据库注入和跨站脚本攻击的风险。接下来，看\
用户注册模块和用户登录模块的设计。

用户注册
-----------------

对于用户注册模块，其路由设计如下：

.. code-block:: php

   <?php
   Route::get('signup', 'UsersController@create')->name('signup');
   Route::resource('users', 'UsersController'); 
   ?>

对于 ``signup`` ，其为用户注册页面设计相应的URL路径，即 ``/signup`` ，并由控制器 ``UsersController`` 向浏览器\
响应视图。控制器 ``UsersController`` 的路径为 ``CCLAnnotator/app/Http/UsersController.php`` ； ``resource`` \
方法相当于多条路由的合集，比如：

+-----------------------------+------------------------+--------------------+------------------+
| **Method**                  | **URI**                | **Route**          | **Name**         |
+-----------------------------+------------------------+--------------------+------------------+
| GET	                      | /users	               | index	            | users.index      |
+-----------------------------+------------------------+--------------------+------------------+
| GET	                      | /users/create          | create             | users.create     |
+-----------------------------+------------------------+--------------------+------------------+
| POST	                      | /users	               | store	            | users.store      |
+-----------------------------+------------------------+--------------------+------------------+
| GET	                      | /users/{user}	       | show	            | users.show       |
+-----------------------------+------------------------+--------------------+------------------+
| GET	                      | /users/{user}/edit     | edit	            | users.edit       |
+-----------------------------+------------------------+--------------------+------------------+
| PUT/PATCH                   | /users/{user}          | update             | users.update     |
+-----------------------------+------------------------+--------------------+------------------+
| DELETE                      | /users/{user}          | destroy            | users.destroy    |
+-----------------------------+------------------------+--------------------+------------------+

当然，在 **CCLAnnotator** 项目中，用户管理系统并未用到全部路由，具体如下：

.. function:: create()
   :module: UsersController

   返回用户注册视图

.. function:: store(Request $request)
   :module: UsersController

   保存用户注册数据，并直接登录

   :param Request: $request 收到客户端发来的HTTP请求
   :rtype: view

.. function:: index()
   :module: UsersController

   返回注册用户列表视图

.. function:: update(Request $request, User $user)
   :module: UsersController

   授权新注册用户成为标注人员

   :param Request: $request 收到客户端发来的HTTP请求
   :param User: $user 待授权用户实例

.. function:: show(User $user)
   :module: UsersController

   返回个人页面

   :param User: $user 特定用户实例


用户登录
-----------------

对于用户登录模块，其路由设计如下：

.. code-block:: php

   <?php 
   Route::get('login', 'SessionsController@create')->name('login');
   Route::post('login', 'SessionsController@store')->name('login');
   Route::delete('logout', 'SessionsController@destroy')->name('logout');
   ?>

同样，对于控制器 ``SessionsController`` ，其路径为 ``CCLAnnotator/app/Http/SessionsController.php`` ，内部\
函数介绍如下：

.. function:: create()
   :module: SessionsController

   返回用户登录视图

.. function:: store(Request $request)
   :module: SessionsController

   验证用户登录信息，并返回相应视图

   :param Request: $request 收到客户端发来的HTTP请求

.. function:: destroy()
   :module: SessionsController

   清除用户登录信息，并返回相应视图
