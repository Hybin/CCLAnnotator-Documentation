前端维护
=================

**CCLAnnotator** 项目前端主要采用 `jQuery <https://jquery.com/>`_ 框架，用于处理相对复杂的标注操作，同时，\
考虑到模块化需求，方便对代码进行管理，采用 **CommonJS** 进行模块化，同时，使用 **Webpack** 进行打包。

对于HTML文件的渲染，我们直接采用了 Laravel 框架提供的 ``Blade`` 语法。前面我们比较细致地介绍了MVC架构中\
的Model和Controller，接下来，我们介绍一下View。一般来说，View 文件的路径为  ``/CCLAnnotator/resources/views/`` ，\
我们可以在文件夹下创建自己的文件夹，并在文件夹下创建 ``blade.php`` 文件。在控制器中，调用视图的函数为 \
``view('{custom_dir}.{custom_blade}')`` 。基于路由变化，控制器返回指定的视图。网站页面不再依赖于多个页面文件。\
具体的 ``Blade`` 语法参看文档，此处不再作赘述。

Laravel 的前端文件存储在 ``/CCLAnnotator/resources/assets/`` 下，包括JavaScript文件和SCSS文件。后者是CSS的扩展\
集，比之CSS更为灵活和强大，具体语言特性请开发者自行学习。考虑到模块化需求，我们使用 **Webpack** 对JS和SCSS资源进行\
打包，最终输出到 ``/CCLAnnotator/public/`` 文件夹下。开发者修改完毕相关代码，需要运行下述命令进行打包：

.. code-block:: bash
   
   $ npm run dev
   $ #or
   $ npm run watch-poll

.. toctree::
   :maxdepth: 2
   :numbered: 2

   require
   ajax
   render