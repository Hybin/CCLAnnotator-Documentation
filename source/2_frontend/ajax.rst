异步请求
==============

通常情况下，使用 ``<form>`` 标签设计表单可以想服务端发送数据请求，并获得响应，但这一过程是同步的，即发出请求后，\
需要等待响应，之后才能进行下一步动作。在某些场合下，同步响应往往会造成不好的用户体验。JavaScript 设计了异步响应\
功能，即数据发出后，可以先进行其他动作，等到响应数据到达，再进行响应的处理。我们在 ``/assets/js/utility.ts`` \
构造同样的数据请求函数：

.. code-block:: TypeScript
   
   // utility.ts
   export const post = (data: JSON, route: String) => {
       return $.ajax({
           type: 'POST',
           url: location.origin + route,
           data: data,
           dataType: 'json',
           async: true,  // 异步
       });
   }

在 ``editor.js`` 或 ``dashboard.js`` 中，假若用到数据请求函数，则可以：

.. code-block:: JavaScript

   // editor.js
   const utility = require('../utility')

   utility.post(data, route).then(data => {
       // code...
   }).catch((xhr, error, status) => {
       // code...
   });

当发出的数据收到成功响应之后，则 ``then()`` 处理相应的逻辑，响应失败，则 ``catch()`` 相应的错误信息。