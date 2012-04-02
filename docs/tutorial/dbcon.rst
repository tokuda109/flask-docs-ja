.. _tutorial-dbcon:

ステップ4: データベース接続
-------------------------------

.. Step 4: Request Database Connections
   ------------------------------------

.. Now we know how we can open database connections and use them for scripts,
   but how can we elegantly do that for requests?  We will need the database
   connection in all our functions so it makes sense to initialize them
   before each request and shut them down afterwards.

データベースに接続をする方法がわかりました。そしてスクリプトで使いますが、リクエスト時にそれをどのように簡潔することができますか?
全ての関数でデータベースに接続する必要があるので、個々のリクエストの前にデータベースを初期化して、その後接続を切ることは理に適っています。

.. Flask allows us to do that with the :meth:`~flask.Flask.before_request`,
   :meth:`~flask.Flask.after_request` and :meth:`~flask.Flask.teardown_request`
   decorators::

Flaskは上述のことを、 :meth:`~flask.Flask.before_request` と :meth:`~flask.Flask.after_request` と
:meth:`~flask.Flask.teardown_request` デコレーターでできます。

    @app.before_request
    def before_request():
        g.db = connect_db()

    @app.teardown_request
    def teardown_request(exception):
        g.db.close()

Functions marked with :meth:`~flask.Flask.before_request` are called before
a request and passed no arguments.  Functions marked with
:meth:`~flask.Flask.after_request` are called after a request and
passed the response that will be sent to the client.  They have to return
that response object or a different one.  They are however not guaranteed
to be executed if an exception is raised, this is where functions marked with
:meth:`~flask.Flask.teardown_request` come in.  They get called after the
response has been constructed.  They are not allowed to modify the request, and
their return values are ignored.  If an exception occurred while the request was
being processed, it is passed to each function; otherwise, `None` is passed in.

.. We store our current database connection on the special :data:`~flask.g`
   object that Flask provides for us.  This object stores information for one
   request only and is available from within each function.  Never store such
   things on other objects because this would not work with threaded
   environments.  That special :data:`~flask.g` object does some magic behind
   the scenes to ensure it does the right thing.

Flaskでは、特別な :data:`~flask.g` オブジェクトにデータベース接続を保管することができます。
このオブジェクトは一つのリクエストで情報を保管し、別の関数から利用可能です。
他のオブジェクトでそのようなことをすることはできません。なぜなら、これはスレッド上で動作していないからです。
そのような特別な :data:`~flask.g` オブジェクトはオブジェクトが正しいことを保証するために裏側でいくつかのマジックを行います。

.. Continue to :ref:`tutorial-views`.

続いては :ref:`tutorial-views` 。

.. Where do I put this code?

   If you've been following along in this tutorial, you might be wondering
   where to put the code from this step and the next.  A logical place is to
   group these module-level functions together, and put your new
   ``before_request`` and ``teardown_request`` functions below your existing
   ``init_db`` function (following the tutorial line-by-line).

   If you need a moment to find your bearings, take a look at how the `example
   source`_ is organized.  In Flask, you can put all of your application code
   into a single Python module.  You don't have to, and if your app :ref:`grows
   larger <larger-applications>`, it's a good idea not to.

.. hint:: このコードをどこに書けばいいですか?

   このチュートリアルをこのステップと次のステップからコードを置く場所を不思議に思うかもしれません。
   これらのモジュールレベルの関数と一緒にグループ化する
   既にある ``init_db`` 関数の下に ``before_request`` と ``teardown_request`` 関数を置いて下さい。

   ベアリングを探すための `example source`_ がどのように組織化されているか見てみましょう。
   Flaskでは、アプリケーションコードの全てを一つのPythonモジュールにすることができます。
   する必要はなくて、 :ref:`grows larger <larger-applications>` 、

.. _example source:
   http://github.com/mitsuhiko/flask/tree/master/examples/flaskr/
