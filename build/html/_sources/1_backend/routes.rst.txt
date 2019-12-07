路由管理
================

传统的 Web 项目，页面的数量等于页面文件的数量。在 Laravel 中，前端属于单页应用，其本质只存在一个页面文件，即 \
``index.php`` 。然而，Web 项目不可能只有一个页面，因此，需要借助路由（Route）进行页面管理。直观地讲，路由规定\
了每个页面的URL，并并告知后台使用相应的控制器。

**CCLAnnotator** 项目的路由文件路径为 ``/CCLAnnotator/routes/web.php`` 。每一条路由的基本格式为：

.. code-block:: php

   <?php 

   Route::{method}('{path}', '{controller}@{function}')->name('{custom-name}');

   ?>

其中， ``{method}`` 表示数据传输方式，比如 ``post`` 、 ``get`` 、 ``delete`` 等。此外，Laravel 框架还提供了 \
``resource`` 方法，作为各种数据传输方式的合集，具体参考 `Laravel 文档 <https://learnku.com/docs/laravel/5.5/routing/1293>`_，\
此处不再赘述。

``path`` 表示网页的路径，由开发者自己定义； ``controller`` 则表示控制相关页面的控制器，其名称同样由开发者自定义, \
当新建路由时，假若控制器亦存在新建的需要，则命名该控制器后，在项目根文件夹，输入以下命令新建控制器文件：

.. code-block:: bash

   $ php artisan make:controller {controller}

``function`` 表示控制器内的方法，用于接收和发送网页数据； ``{custom-name}`` 表示对该路由的命名，之后开发者可以\
在页面中使用 ``route({custom-name})`` 函数使用这一路由。
