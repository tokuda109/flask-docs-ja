.. _shell:

シェルでの作業
=========================

.. Working with the Shell
   ======================

.. versionadded:: 0.3

.. One of the reasons everybody loves Python is the interactive shell.  It
   basically allows you to execute Python commands in real time and
   immediately get results back.  Flask itself does not come with an
   interactive shell, because it does not require any specific setup upfront,
   just import your application and start playing around.

全ての人がPythonを好きな理由の内の一つはインタラクティブシェルです。
基本的にリアルタイムにPythonコマンドを実行することができ、すぐに結果が返ってきます。
Flask自身はインタラクティブシェルが付属していません。
事前に任意の設定を必要としないので、アプリケーションをインポートするだけで使い始めることができます。

.. There are however some handy helpers to make playing around in the shell a
   more pleasant experience.  The main issue with interactive console
   sessions is that you're not triggering a request like a browser does which
   means that :data:`~flask.g`, :data:`~flask.request` and others are not
   available.  But the code you want to test might depend on them, so what
   can you do?

しかし、シェルをより楽しく使うための便利なヘルパーがいくつかあります。
インタラクティブコンソールのセッションの主な問題は、 :data:`~flask.g` や :data:`~flask.request` や他にも利用できないものがあり、
そのようなブラウザが行うようなリクエストを処理できません。
しかし、テストしたいコードはそれらに依存しているので、何をすることができますか?

.. This is where some helper functions come in handy.  Keep in mind however
   that these functions are not only there for interactive shell usage, but
   also for unittesting and other situations that require a faked request
   context.

いくつかのヘルパー関数があります。
しかし、これらの機能はインタラクティブシェルだけではなく、ユニットテストやダミーのリクエストコンテキストが必要な状況でも使われるということに注意して下さい。

.. Generally it's recommended that you read the :ref:`request-context`
   chapter of the documentation first.

まず、ドキュメントの :ref:`request-context` の章を読むことをお勧めします。

.. Creating a Request Context
   --------------------------

リクエストコンテキストの作成
----------------------------------

.. The easiest way to create a proper request context from the shell is by
   using the :attr:`~flask.Flask.test_request_context` method which creates
   us a :class:`~flask.ctx.RequestContext`:

適切なリクエストコンテキストをシェルから作成する簡単な方法は、
:class:`~flask.ctx.RequestContext` を作成する :attr:`~flask.Flask.test_request_context` メソッドを使うことです。 :

>>> ctx = app.test_request_context()

.. Normally you would use the `with` statement to make this request object
   active, but in the shell it's easier to use the
   :meth:`~flask.ctx.RequestContext.push` and
   :meth:`~flask.ctx.RequestContext.pop` methods by hand:

このリクエストオブジェクトを作るために、 `with` 文を普通は使うと思いますが、
:meth:`~flask.ctx.RequestContext.push` と :meth:`~flask.ctx.RequestContext.pop` メソッドをシェルで使うことは簡単です。 :

>>> ctx.push()

.. From that point onwards you can work with the request object until you
   call `pop`:

その時点から `pop` を呼ぶまでは、リクエストオブジェクトを扱うことができます。 :

>>> ctx.pop()

.. Firing Before/After Request
   ---------------------------

リクエストの前後
----------------------

.. By just creating a request context, you still don't have run the code that
   is normally run before a request.  This might result in your database
   being unavailable if you are connecting to the database in a
   before-request callback or the current user not being stored on the
   :data:`~flask.g` object etc.

リクエストコンテキストを作成しても、まだコードは実行されていません
これは、before-requestコールバックでデータベースに接続するか、
現在のユーザーが :data:`~flask.g` オブジェクトなどに保管されていない場合に、
データベースが利用できなくなったという結果になるかもしれません。

.. This however can easily be done yourself.  Just call
   :meth:`~flask.Flask.preprocess_request`:

これは簡単に実装することができます。
:meth:`~flask.Flask.preprocess_request` を呼び出すだけです。 :

>>> ctx = app.test_request_context()
>>> ctx.push()
>>> app.preprocess_request()

.. Keep in mind that the :meth:`~flask.Flask.preprocess_request` function
   might return a response object, in that case just ignore it.

:meth:`~flask.Flask.preprocess_request` 関数はレスポンスオブジェクトを返すということに注意して下さい。

.. To shutdown a request, you need to trick a bit before the after request
   functions (triggered by :meth:`~flask.Flask.process_response`) operate on
   a response object:

リクエストをシャットダウンするために、レスポンスオブジェクトを操作する
after request関数(:meth:`~flask.Flask.process_response` によって呼び出される)の前にちょっとしたトリックが必要です。 :

>>> app.process_response(app.response_class())
<Response 0 bytes [200 OK]>
>>> ctx.pop()

.. The functions registered as :meth:`~flask.Flask.teardown_request` are
   automatically called when the context is popped.  So this is the perfect
   place to automatically tear down resources that were needed by the request
   context (such as database connections).

:meth:`~flask.Flask.teardown_request` として登録されている関数は、コンテキストがpopされた時に自動的に呼ばれます。
だから、これはリクエストコンテキストで必要とされたリソース(データベース接続のような)を自動的に廃棄するための場所です。


.. Further Improving the Shell Experience
   --------------------------------------

さらにシェル環境を改善するには
-----------------------------------------

.. If you like the idea of experimenting in a shell, create yourself a module
   with stuff you want to star import into your interactive session.  There
   you could also define some more helper methods for common things such as
   initializing the database, dropping tables etc.

インタラクティブのセッション中にモジュールのスタッフを全てインポートしたい
データベースの初期化やテーブルの削除などのような一般的な事のためのヘルパー関数を定義することもできます。

.. Just put them into a module (like `shelltools` and import from there):

モジュール(`shelltools` とそこからインポート)に以下のように書くだけです。 :

>>> from shelltools import *
