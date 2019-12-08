文件管理
============

**CCLAnnotator** 构式语料标注平台最重要的模块之一就是语料文件管理。对于文件系统，Laravel 提供了完善的 ``File`` 模块\
和 ``Storage`` 模块，开发者能够方便地调用相关函数进行文件管理。使用 ``File`` 模块，需要提供文件的绝对路径；而 \
``Storage`` 模块则可以直接定位到存储文件，开发者仅需提供相对路径，比如：

.. code-block::
   
   Storage::disk('local') => /storage/app/

依照 Laravel 默认的文件系统配置，**CCLAnnotator** 平台选择服务器磁盘作为存储空间，因此，语料文件存储路径为：

.. code-block::

   /CCLAnnotator/storage/app/public/

在该文件夹下，我们创建了 ``/constructions/materials/`` 文件夹和 ``/constructions/corpora/`` 文件夹，分别用于\
存储生语料和标注语料。当标注任务进入存档阶段，语料文件会从 ``/constructions/materials/`` 移入 ``/constructions/corpora/`` 。\

.. note::
   标注完毕的语料文件将以其构式形式在现代汉语构式知识库中的ID为文件名进行保存。

因此，对于生语料，我们期待的文件命名方式是 ``日期_构式形式_构式ID.xml`` ，既保证该文件在 ``/constructions/materials/`` 文件夹\
中的独一性，避免数据覆盖；也能使同一构式的不同实例语料能够正确地存档。

**CCLAnnotator** 平台对于文件的操作包括上传文件、更新文件、删除文件以及语料标注过程的保存操作，其路由设计如下：

.. code-block:: php

   <?php
   Route::post('files/upload', 'UploadedFilesController@store')->name('files.store');
   Route::post('files/update', 'UploadedFilesController@update')->name('files.update');
   Route::post('files/verify', 'UploadedFilesController@verify')->name('files.verify');
   Route::get('files/download/{file}', 'UploadedFilesController@download')->name('files.download');
   Route::post('files/commit', 'UploadedFilesController@commit')->name('files.commit');
   Route::post('files/destroy', 'UploadedFilesController@destroy')->name('files.destroy');
   
   Route::resource('corpora', 'StoreFilesController');
   ?>

由上可知，文件管理系统涉及到两个控制器： ``UploadedFilesController`` 和 ``StoreFilesController`` 。下面分别介绍其相关方法：

.. function:: store(Request $request)
   :module: UploadedFilesController

   上传文件，保存文件到``/constructions/materials/`` 文件夹，并更新数据库信息，返回后台操作视图

   :param Request: $request 收到客户端发来的HTTP请求

.. function:: update(Request $request)
   :module: UploadedFilesController

   更新文件，并更新数据库信息，返回后台操作视图

   :param Request: $request 收到客户端发来的HTTP请求

.. function:: verify(Request $request)
   :module: UploadedFilesController

   文件下载确认，返回确认信息

   :param Request: $request 收到客户端发来的HTTP请求
   :rtype: json

.. function:: download($file)
   :module: UploadedFilesController

   下载文件

   :param String: $file 待下载文件名

.. function:: commit(Request $request)
   :module: UploadedFilesController

   提交文件，更新任务状态，更新文件内容，返回提交处理信息

   :param Request: $request 收到客户端发来的HTTP请求
   :rtype: json

.. function:: destroy(Request $request)
   :module: UploadedFilesController

   删除文件，返回删除处理信息

   :param Request: $request 收到客户端发来的HTTP请求
   :rtype: json

.. function:: clear($path)
   :module: UploadedFilesController

   当标注任务状态改变时，与之对应的文件应清除标注标记信息('annotated')，并更新文件

   :param String: $path 文件路径

显然，控制器 ``UploadedFilesController`` 是对文件本身的操作，与之相对，控制器 ``StoreFilesController`` 对文件流\
内容进行操作：

.. function:: store(Request $request)
   :module: StoreFilesController

   保存标注内容到文件，并返回保存状态信息

   :param Request: $request 收到客户端发来的HTTP请求
   :rtype: json

.. function:: loadData($path, $node)
   :module: StoreFilesController

   静态函数，加载指定节点的XML文件内容并返回

   :param String: $path ``.xml`` 文件路径
   :param String: $node ``.xml`` 文件节点
   :rtype: Array<XMLElement>

.. function:: mergeXML($old, $new, $node, $construction)
   :module: StoreFilesController

   静态函数，将新的标注内容添加到已存档的标注语料文件

   :param String: $old 已存档的标注语料文件路径
   :param String: $new 待加入的标注语料文件路径
   :param String: 句子节点，默认为 ``sentence``

.. function:: sort($path, $task)
   :module: StoreFilesController

   静态函数，对标注文件内容进行分类排序，收拢具备相同的构式内容的句子，并按构式频次进行升序排列

   :param String: $path 待排序文件路径
   :param String: $task 待排序文件名称   