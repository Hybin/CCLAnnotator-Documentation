文件引用
================

借助RequireJS，我们可以很方便地引用不同文件的 ``JavaScript`` 代码。比如，在 ``/assets/js/components/`` 文件夹\
下，包含了 ``editor.js`` 和 ``dashboard.js`` 分别保存相关界面的 ``JavaScript`` 代码。其内部结构很简单：

.. code-block:: JavaScript

   // editor.js
   export const control = () => {
       // code...
   }

要想在 ``/assets/js/app.js`` 中使用上述代码，只需在文件中加入：

.. code-block:: JavaScript
   
   // app.js
   const editor = require('./components/editor');

   editor.control();

而后进行编译打包即可。