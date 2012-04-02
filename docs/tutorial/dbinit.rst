.. _tutorial-dbinit:

ステップ3: データベースの作成
================================

.. Step 3: Creating The Database
   =============================

.. Flaskr is a database powered application as outlined earlier, and more
   precisely, an application powered by a relational database system.  Such
   systems need a schema that tells them how to store that information. So
   before starting the server for the first time it's important to create
   that schema.

前に述べたようにFlaskrは、データベースを使うアプリケーションです。
そして、より正確には、リレーショナルデータベースシステムを使うアプリケーションです。
このようなシステムは、情報の保存方法を伝えるスキーマが必要です。
最初にサーバーを立ち上げる前に、そのようなスキーマを作成することは重要なことです。

.. Such a schema can be created by piping the `schema.sql` file into the
   `sqlite3` command as follows::

このようなスキーマは、以下のような `sqlite3` コマンドに `schema.sql` ファイルを続けて書くことで作成できます。

    sqlite3 /tmp/flaskr.db < schema.sql

.. The downside of this is that it requires the sqlite3 command to be
   installed which is not necessarily the case on every system.  Also one has
   to provide the path to the database there which leaves some place for
   errors.  It's a good idea to add a function that initializes the database
   for you to the application.

これの欠点は、全てのシステムで必ずしもインストールされていない場合に、sqlite3コマンドをインストールする必要があることです。
データベースへのパスを指定しなければいけません。
アプリケーションにデータベースを初期化する関数を追加することは良い考えです。

.. If you want to do that, you first have to import the
   :func:`contextlib.closing` function from the contextlib package.  If you
   want to use Python 2.5 it's also necessary to enable the `with` statement
   first (`__future__` imports must be the very first import)::

そうしたいなら、まずcontextlibパッケージから :func:`contextlib.closing` 関数をインポートする必要があります。
Python 2.5 を使いたい場合は、最初に `with` 文を有効化して下さい(`__future__` を最初にインポートして下さい)。

    from __future__ import with_statement
    from contextlib import closing

.. Next we can create a function called `init_db` that initializes the
   database.  For this we can use the `connect_db` function we defined
   earlier.  Just add that function below the `connect_db` function::

次に、データベースを初期化する `init_db` という関数を作成して下さい。
これは、定義されている `connect_db` 関数を使います。
`connect_db` 関数の下に関数を追加するだけです。 ::

    def init_db():
        with closing(connect_db()) as db:
            with app.open_resource('schema.sql') as f:
                db.cursor().executescript(f.read())
            db.commit()

.. The :func:`~contextlib.closing` helper function allows us to keep a
   connection open for the duration of the `with` block.  The
   :func:`~flask.Flask.open_resource` method of the application object
   supports that functionality out of the box, so it can be used in the
   `with` block directly.  This function opens a file from the resource
   location (your `flaskr` folder) and allows you to read from it.  We are
   using this here to execute a script on the database connection.

:func:`~contextlib.closing` ヘルパー関数は、 `with` ブロックの中で接続を保ちます。
アプリケーションオブジェクトの :func:`~flask.Flask.open_resource` メソッドは直接 `with` ブロックで使います。
この関数はリソースの置いている場所でファイルを開いて、それを読むことができます。
データベース接続のスクリプトを実行するために、ここでこれを使います。

.. When we connect to a database we get a connection object (here called
   `db`) that can give us a cursor.  On that cursor there is a method to
   execute a complete script.  Finally we only have to commit the changes.
   SQLite 3 and other transactional databases will not commit unless you
   explicitly tell it to.

データベースに接続した時、コネクションオブジェクト(ここでは `db` を言います)を取得してカーソルを使うことができます。
カーソル上でスクリプトを実行するためのメソッドがあります。最後に変更をコミットして下さい。
SQLite3と他のトランザクション機能を持ったデータベースは、明示的に指定するまでコミットしません。

.. Now it is possible to create a database by starting up a Python shell and
   importing and calling that function::

Pythonシェルを起動して、インポートして関数を実行することでデータベースを作成することができます。 ::

>>> from flaskr import init_db
>>> init_db()

.. Troubleshooting

   If you get an exception later that a table cannot be found check that
   you did call the `init_db` function and that your table names are
   correct (singular vs. plural for example).

.. admonition:: トラブルシューティング

   例外が発生したら `init_db` 関数を呼び出した

.. Continue with :ref:`tutorial-dbcon`

続いては :ref:`tutorial-dbcon` 。
