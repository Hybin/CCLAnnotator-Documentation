权限管理
================

Laravel 框架对于权限管理主要体现在中间件（Middleware）和隐私策略（Policy）上的设计。在 **CCLAnnotator** 平台上，\
某些页面，比如个人页面，标注页面，要求必需登录平台才能进行操作，如何进行限制呢？答案就是中间件。

中间件
----------------

在相关的控制器中，加入如下代码：

.. code-block:: php

   <?php
   public function __construct()
   {
       $this->middleware('auth', [
           'except' => ['create', 'store', 'show']
       ]);

       $this->middleware('guest', [
           'only' => ['create']
       ]);
   }
   ?>

``__construct`` 是 PHP 的构造器方法，当一个类对象被创建之前该方法将会被调用。我们在 ``__construct`` 方法中调用了 \
``middleware`` 方法，该方法接收两个参数，第一个为中间件的名称，第二个为要进行过滤的动作。我们通过 ``except`` 方法\
来设定 **指定动作** 不使用 **Auth** 中间件进行过滤，意为——除了此处指定的动作以外，所有其他动作都必须登录用户才能访问，\
类似于黑名单的过滤机制。相反的还有 ``only`` 白名单方法，将只过滤指定动作。


隐私策略
-----------------

除了中间件，我们还可以通过设计隐私策略来指定用户操作权限。针对 **CCLAnnotator** 平台，我们设计了用户策略，路径为 \
``/CCLAnnotator/app/Policies/UserPolicy.php`` ，包括下述策略：

.. function:: index(User $currentUser, User $admin)
   :module: UserPolicy

   判断当前用户的身份是否为管理员

   :param User: $currentUser 当前用户实例
   :param User: $admin 管理员实例
   :rtype: Boolean

.. function:: edit(User $currentUser, User $admin) [stopped]
   :module: UserPolicy

   判断当前用户的身份是否为管理员

   :param User: $currentUser 当前用户实例
   :param User: $admin 管理员实例
   :rtype: Boolean

.. function:: show(User $currentUser, User $user)
   :module: UserPolicy

   判断当前用户是否与指定用户一致

   :param User: $currentUser 当前用户实例
   :param User: $user 指定用户
   :rtype: Boolean

在具体的使用中，开发者可以使用 ``$this->authorize('{policy}', {second-argument})`` 来判断是否合乎策略。其中， \
``{policy}`` 即为上述函数名称， ``{second-argument}`` 为函数的第二个参数。
