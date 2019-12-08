后台管理
=============

正如每一位注册用户都需要一个个人页面，对于平台管理员而言，亦需要一个后台页面，来管理标注任务和人员授权。对于 **CCLAnnotator** \
平台，后台控制器 ``DashboardController`` 主要涉及后台数据传输与标注菜单管理。首先看路由设计：

.. code-block:: php

   <?php
   Route::get('dashboard', 'DashboardController@open')->name('dashboard.open');
   Route::post('dashboard/menu-setting/push', 'DashboardController@push')->name('dashboard.push');
   Route::post('dashboard/menu-setting/pop', 'DashboardController@pop')->name('dashboard.pop');
   ?>

由路由可知，控制器 ``DashboardController`` 并未包含太多函数，大致如下：

.. function:: open()
   :module: DashboardController

   返回Dashboard视图

.. function:: push(Request $request)
   :module: DashboardController

   新增标注菜单选项，更新配置文件，并返回视图。

   :param Request: $request 收到客户端发来的HTTP请求

.. function:: pop(Request $request)
   :module: DashboardController

   删除标注菜单选项，更新配置文件，并返回处理结果

   :param Request: $request
   :rtype: json

.. function:: update($content)
   :module: DashboardController

   更新配置文件 ``config/constructions.php``

   :param Array: $content 更新的```config('constructions')`` 数组

.. note::
   借助 ``config()`` 函数，Laravel 能够自动修改数组，但其存在计算机内存中，并不能反馈到配置文件中，需要构造更新函数。

