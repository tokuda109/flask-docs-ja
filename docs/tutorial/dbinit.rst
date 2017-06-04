.. _tutorial-dbinit:

.. Step 5: Creating The Database
   =============================

ステップ5: データベースの作成
================================

.. As outlined earlier, Flaskr is a database powered application, and more
   precisely, it is an application powered by a relational database system.  Such
   systems need a schema that tells them how to store that information.
   Before starting the server for the first time, it's important to create
   that schema.

前述したように、Flaskr はデータベースを使うアプリケーションで、より正確に言うとリレーショナルデータベースを使うアプリケーションです。
そのようなシステムは、情報をどのように保管するかを指定するスキーマが必要です。
最初にサーバーを起動する前に、スキーマを作成することは重要です。

.. Such a schema could be created by piping the ``schema.sql`` file into the
   ``sqlite3`` command as follows::

このようなスキーマは、以下のような ``sqlite3`` コマンドで ``schema.sql`` ファイルをパイプすることで作成できます。

    sqlite3 /tmp/flaskr.db < schema.sql

.. However, the downside of this is that it requires the ``sqlite3`` command
   to be installed, which is not necessarily the case on every system. This
   also requires that you provide the path to the database, which can introduce
   errors.

しかし、全てのシステムに当てはまるわけではありませんが、
``sqlite3`` コマンドをインストールする必要があるという欠点があります。
データベースへのパスを指定する必要があり、エラーが発生する可能性があります。

.. Instead of the ``sqlite3`` command above, it's a good idea to add a function
   to our application that initializes the database for you. To do this, you
   can create a function and hook it into a :command:`flask` command that
   initializes the database.

上記の ``sqlite3`` コマンドの代わりに、データベースを初期化する関数を追加することをお勧めします。
これをするには、関数を作成して、データベースを初期化する :command:`flask` コマンドでフックします。

.. Take a look at the code segment below. A good place to add this function,
   and command, is just below the ``connect_db`` function in :file:`flaskr.py`::

以下のコードを見て下さい。
:file:`flaskr.py` ファイルの ``connect_db`` 関数に下に、以下の関数とコマンドがあります。 ::

    def init_db():
        db = get_db()

        with app.open_resource('schema.sql', mode='r') as f:
            db.cursor().executescript(f.read())

        db.commit()


    @app.cli.command('initdb')
    def initdb_command():
        """Initializes the database."""

        init_db()
        print('Initialized the database.')

.. The ``app.cli.command()`` decorator registers a new command with the
   :command:`flask` script.  When the command executes, Flask will automatically
   create an application context which is bound to the right application.
   Within the function, you can then access :attr:`flask.g` and other things as
   you might expect.  When the script ends, the application context tears down
   and the database connection is released.

``app.cli.command()`` デコレータは、:command:`flask` スクリプトに新しいコマンドを登録します。
コマンドが実行されると、Flask はアプリケーションにバインドされたアプリケーションコンテキストを自動的に作成します。
関数内では、:attr:`flask.g` にアクセスすることができ、実行したいことを行うことができます。
スクリプトが終了すると、アプリケーションコンテキストは破棄され、データベース接続が開放されます。

.. You will want to keep an actual function around that initializes the database,
   though, so that we can easily create databases in unit tests later on.  (For
   more information see :ref:`testing`.)

データベースを初期化した関数を保持しておくと、後で単体テストでデータベース接続をかんたんに作成することができます。
(より詳しくは、:ref:`testing` を見て下さい。)

.. The :func:`~flask.Flask.open_resource` method of the application object
   is a convenient helper function that will open a resource that the
   application provides.  This function opens a file from the resource
   location (the :file:`flaskr/flaskr` folder) and allows you to read from it.
   It is used in this example to execute a script on the database connection.

アプリケーションオブジェクトの :func:`~flask.Flask.open_resource` メソッドは、
アプリケーションが提供するリソースを開くことができる便利なヘルパー関数です。
この関数は、リソースの場所(:file:`flaskr/flaskr` フォルダ)からファイルを開き、そこから読み込むことができます。
データベースの接続をするスクリプトを実行するサンプルで使われています。

.. The connection object provided by SQLite can give you a cursor object.
   On that cursor, there is a method to execute a complete script.  Finally, you
   only have to commit the changes.  SQLite3 and other transactional
   databases will not commit unless you explicitly tell it to.

SQLite によって提供された接続オブジェクトは、カーソルオブジェクトを提供します。
そのカーソルに、完全なスクリプトを実行するメソッドがあります。
最後に、変更をコミットするだけです。
SQLite3 と他のトランザクションデータベースでは、明示的に指定しない限りコミットされません。

.. Now, in a terminal, from the application root directory :file:`flaskr/` it is
   possible to create a database with the :command:`flask` script::

ターミナルで、アプリケーションのルートディレクトリの :file:`flaskr/` から、
:command:`flask` スクリプトを使って、データベースを作成することができます。 ::

    flask initdb
    Initialized the database.

.. Troubleshooting

   If you get an exception later on stating that a table cannot be found, check
   that you did execute the ``initdb`` command and that your table names are
   correct (singular vs. plural, for example).

.. admonition:: トラブルシューティング

   テーブルが見つからなくて例外が発生した場合、``initdb`` コマンドを実行できたかと、
   テーブル名が正しいか(例として、単数形と複数形を間違えていないか)を確認して下さい。

.. Continue with :ref:`tutorial-views`

続いては、:ref:`tutorial-views` 。
