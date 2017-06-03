.. _tutorial-dbcon:

.. Step 4: Database Connections
   ----------------------------

ステップ4: データベース接続
-------------------------------

.. Let's continue building our code in the ``flaskr.py`` file.
   (Scroll to the end of the page for more about project layout.)

``flaskr.py`` ファイルにコードを追加していきましょう。
(プロジェクトの構成について、より詳しく見る場合はページの下までスクロールして下さい。)

.. You currently have a function for establishing a database connection with
   `connect_db`, but by itself, it is not particularly useful.  Creating and
   closing database connections all the time is very inefficient, so you will
   need to keep it around for longer.  Because database connections
   encapsulate a transaction, you will need to make sure that only one
   request at a time uses the connection. An elegant way to do this is by
   utilizing the *application context*.

`connect_db` でデータベース接続を確立する関数がありますが、あまり役に立ちません。
データベース接続の確立と切断を常に行うのは非効率なので、持続的に接続する必要があります。
データベース接続はトランザクションをカプセル化するので、一度に1リクエストしか接続しないようにする必要があります。
これをするためのエレガントな方法は、*アプリケーションコンテキスト* を利用することです。

.. Flask provides two contexts: the *application context* and the
   *request context*.  For the time being, all you have to know is that there
   are special variables that use these.  For instance, the
   :data:`~flask.request` variable is the request object associated with
   the current request, whereas :data:`~flask.g` is a general purpose
   variable associated with the current application context.  The tutorial
   will cover some more details of this later on.

Flask は、*アプリケーションコンテキスト* と *リクエストコンテキスト* の2つのコンテキストを提供しています。
:data:`~flask.request` 変数は、現在のリクエストに関連付けられたリクエストのオブジェクトですが、
:data:`~flask.g` は、現在のアプリケーションコンテキストに関連付けられた汎用的な変数です。
より詳細なチュートリアルを後ほど紹介します。

.. For the time being, all you have to know is that you can store information
   safely on the :data:`~flask.g` object.

今は、:data:`~flask.g` オブジェクトに安全に情報を保持できるということだけ覚えておいて下さい。

.. So when do you put it on there?  To do that you can make a helper
   function.  The first time the function is called, it will create a database
   connection for the current context, and successive calls will return the
   already established connection::

g オブジェクトにどのように代入するかを、ヘルパー関数を実装して行います。
関数が初めて呼ばれた時、現在のコンテキストでデータベース接続を確立し、確立済みのコネクションを常に返却します。 ::

    def get_db():
        """Opens a new database connection if there is none yet for the
        current application context.
        """
        if not hasattr(g, 'sqlite_db'):
            g.sqlite_db = connect_db()
        return g.sqlite_db

.. Now you know how to connect, but how can you properly disconnect?  For
   that, Flask provides us with the :meth:`~flask.Flask.teardown_appcontext`
   decorator.  It's executed every time the application context tears down::

データベース接続をする方法はわかりましたが、接続を正しく切るにはどのようにしますか?
そのために、Flask は :meth:`~flask.Flask.teardown_appcontext` デコレータを提供しています。
アプリケーションコンテキストを破棄するたび実行されます。

    @app.teardown_appcontext
    def close_db(error):
        """Closes the database again at the end of the request."""
        if hasattr(g, 'sqlite_db'):
            g.sqlite_db.close()

.. Functions marked with :meth:`~flask.Flask.teardown_appcontext` are called
   every time the app context tears down.  What does this mean?
   Essentially, the app context is created before the request comes in and is
   destroyed (torn down) whenever the request finishes.  A teardown can
   happen because of two reasons: either everything went well (the error
   parameter will be ``None``) or an exception happened, in which case the error
   is passed to the teardown function.

:meth:`~flask.Flask.teardown_appcontext` で関数は、アプリケーションコンテキストが破棄されるたびに呼び出されます。
これは何を意味するのでしょうか?
基本的には、アプリケーションコンテキストはリクエストがくる前に作られ、リクエストが終了するたびに破棄されます。
アプリケーションコンテキストが破棄される理由は、すべてが上手くいった(エラーパラメーターが ``None`` になる)か、
例外が発生してエラーが破棄する関数に渡された時の2つあります。

.. Curious about what these contexts mean?  Have a look at the
   :ref:`app-context` documentation to learn more.

このコンテキストがどのような意味があるか知りたい場合は、:ref:`app-context` の文章を見て下さい。

.. Continue to :ref:`tutorial-dbinit`.

続いては、:ref:`tutorial-dbinit` 。

.. Where do I put this code?

   If you've been following along in this tutorial, you might be wondering
   where to put the code from this step and the next.  A logical place is to
   group these module-level functions together, and put your new
   ``get_db`` and ``close_db`` functions below your existing
   ``connect_db`` function (following the tutorial line-by-line).

   If you need a moment to find your bearings, take a look at how the `example
   source`_ is organized.  In Flask, you can put all of your application code
   into a single Python module.  You don't have to, and if your app :ref:`grows
   larger <larger-applications>`, it's a good idea not to.

.. hint:: このコードをどこに書けばいいですか?

   このチュートリアル通りにすると、今回のステップと次のステップでコードをどこに書くか疑問に思うかもしれません。
   これらのモジュールをまとめてグループ化し、既存の ``connect_db`` 関数の下に ``get_db`` と ``close_db`` 関数を新しく置くことです(チュートリアルの最後)。

   ベアリングを探す、`example source`_ がどのように構成されているか見て下さい。
   Flask では、単一の Python モジュールにアプリケーションコードを全て入れることができます。
   :ref:`grows larger <larger-applications>` になって、

.. _example source:
   https://github.com/pallets/flask/tree/master/examples/flaskr/
