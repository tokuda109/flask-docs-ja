.. _app-context:

アプリケーションコンテキスト
================================

.. The Application Context
   =======================

.. versionadded:: 0.9

.. One of the design ideas behind Flask is that there are two different
   “states” in which code is executed.  The application setup state in which
   the application implicitly is on the module level.  It starts when the
   :class:`Flask` object is instantiated, and it implicitly ends when the
   first request comes in.  While the application is in this state a few
   assumptions are true:

Flaskの背景にある設計思想の一つは、コードが実行される際に2つの異なる "状態" があるということです。
アプリケーションが暗黙的に、モジュールレベルであるアプリケーションのセットアップ状態になります。
:class:`Flask` オブジェクトがインスタンス化された時に起動し、最初にリクエストが来た際に明示的に終了します。
アプリケーションがこの状態である間は、この前提条件は正しいものとされます。

.. the programmer can modify the application object safely.
.. no request handling happened so far
.. you have to have a reference to the application object in order to
   modify it, there is no magic proxy that can give you a reference to
   the application object you're currently creating or modifying.

- 開発者はアプリケーションのオブジェクトを安全に変更することができます。
- リクエストの処理がない場合は何も起こりません。
- you have to have a reference to the application object in order to
  modify it, there is no magic proxy that can give you a reference to
  the application object you're currently creating or modifying.

.. On the contrast, during request handling, a couple of other rules exist:

対照的に、リクエストの処理中にいくつかのルールがあります。 :

.. while a request is active, the context local objects
   (:data:`flask.request` and others) point to the current request.
.. any code can get hold of these objects at any time.

- リクエストがコンテキストローカルのオブジェクト( :data:`flask.request` や他のもの)は現在のリクエストを参照しています。
- 任意のコードを任意の時点で、これらのオブジェクトを保持することができます。

There is a third state which is sitting in between a little bit.
Sometimes you are dealing with an application in a way that is similar to
how you interact with applications during request handling just that there
is no request active.  Consider for instance that you're sitting in an
interactive Python shell and interacting with the application, or a
command line application.

.. The application context is what powers the :data:`~flask.current_app`
   context local.

アプリケーションコンテキストは、 :data:`~flask.current_app` というコンテキストローカルを持っています。

.. Purpose of the Application Context
   ----------------------------------

アプリケーションコンテキストの目的
----------------------------------------

The main reason for the application's context existance is that in the
past a bunch of functionality was attached to the request context in lack
of a better solution.  Since one of the pillar's of Flask's design is that
you can have more than one application in the same Python process.

.. アプリケーションのコンテキストがあることの主な理由として、
   いいソリューションが欠けているものにリクエストコンテキストを

So how does the code find the “right” application?  In the past we
recommended passing applications around explicitly, but that caused issues
with libraries that were not designed with that in mind.

A common workaround for that problem was to use the
:data:`~flask.current_app` proxy later on, which was bound to the current
request's application reference.  Since however creating such a request
context is an unnecessarily expensive operation in case there is no
request around, the application context was introduced.

.. Creating an Application Context
   -------------------------------

アプリケーションコンテキストの作成
----------------------------------------

.. To make an application context there are two ways.  The first one is the
   implicit one: whenever a request context is pushed, an application context
   will be created alongside if this is necessary.  As a result of that, you
   can ignore the existance of the application context unless you need it.

アプリケーションコンテキストを作成するには二つの方法があります。
一つ目は明示的な方法で、リクエストコンテキストが追加されるたびに、
アプリケーションコンテキストが必要なら作成されます。
その結果として、必要になるまでアプリケーションコンテキストの存在を無視することができます。

.. The second way is the explicit way using the
   :meth:`~flask.Flask.app_context` method::

二つ目の方法は明示的な方法で、 :meth:`~flask.Flask.app_context` メソッドを使うことです。 ::

    from flask import Flask, current_app

    app = Flask(__name__)
    with app.app_context():
        # within this block, current_app points to app.
        print current_app.name

.. The application context is also used by the :func:`~flask.url_for`
   function in case a ``SERVER_NAME`` was configured.  This allows you to
   generate URLs even in the absence of a request.

アプリケーションコンテキストは ``SERVER_NAME`` が設定されている場合に、
:func:`~flask.url_for` 関数によって使われます。
これによってリクエストがない場合には、URLを生成することができるようになります。

.. Locality of the Context
   -----------------------

コンテキストのローカリティー
---------------------------------

.. The application context is created and destroyed as necessary.  It never
   moves between threads and it will not be shared between requests.  As such
   it is the perfect place to store database connection information and other
   things.  The internal stack object is called :data:`flask._app_ctx_stack`.
   Extensions are free to store additional information on the topmost level,
   assuming they pick a sufficiently unique name.

アプリケーションコンテキストは必要に応じて作成され、破棄されます。
スレッド間でやり取りされることはなく、リクエスト間で共有されることもありません。
データベース接続の情報や他の情報を保管するところです。
内部のスタックオブジェクトは、 :data:`flask._app_ctx_stack` と呼ばれています。
ユニークな名前で選んでいると仮定して、
エクステンションは最上位にある追加情報を保管することは自由にできます。

.. For more information about that, see :ref:`extension-dev`.

さらに詳しい情報については :ref:`extension-dev` を見て下さい。
