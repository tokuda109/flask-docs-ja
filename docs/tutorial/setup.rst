.. _tutorial-setup:

.. Step 2: Application Setup Code
   ==============================

ステップ2: アプリケーションのセットアップコード
===================================================

.. Next, we will create the application module, :file:`flaskr.py`.  Just like the
   :file:`schema.sql` file you created in the previous step, this file should be
   placed inside of the :file:`flaskr/flaskr` folder.

次に、アプリケーションモジュールの :file:`flaskr.py` を作成します。
前のステップで作成した :file:`schema.sql` ファイルの時と同じく、このファイルは :file:`flaskr/flaskr` フォルダ内に置く必要があります。

.. For this tutorial, all the Python code we use will be put into this file
   (except for one line in ``__init__.py``, and any testing or optional files you
   decide to create).

このチュートリアルでは、全ての Python のコードをこのファイルに記述します。
(``__init__.py`` の1行と、テストや任意で作成したファイルは除きます)

.. The first several lines of code in the application module are the needed import
   statements.  After that there will be a few lines of configuration code.

アプリケーションモジュールのコードの最初の数行は import 文です。
その後、設定のためのコードが数行あります。

.. For small applications like ``flaskr``, it is possible to drop the configuration
   directly into the module.  However, a cleaner solution is to create a separate
   ``.py`` file, load that, and import the values from there.

``flaskr`` のような小さなアプリケーションでは、モジュール内に設定を直接記述しても問題ありません。
しかし、別の ``.py`` ファイルを作成して、そこから値をインポートするのがきれいなやり方です。

.. Here are the import statements (in :file:`flaskr.py`)::

(:file:`flaskr.py` の) import 文は次のとおりです。 ::

    import os
    import sqlite3

    from flask import (Flask, request, session, g, redirect, url_for, abort,
        render_template, flash)

.. The next couple lines will create the actual application instance and
   initialize it with the config from the same file in :file:`flaskr.py`::

次の数行は、実際のアプリケーションインスタンスを作成し、
:file:`flaskr.py` で同じファイルのコンフィグを使ってアプリケーションを初期化します。 ::

    app = Flask(__name__) # create the application instance :)
    app.config.from_object(__name__) # load config from this file , flaskr.py

    # Load default config and override config from an environment variable
    app.config.update(dict(
        DATABASE=os.path.join(app.root_path, 'flaskr.db'),
        SECRET_KEY='development key',
        USERNAME='admin',
        PASSWORD='default'
    ))
    app.config.from_envvar('FLASKR_SETTINGS', silent=True)

.. In the above code, the :class:`~flask.Config` object works similarly to a
   dictionary, so it can be updated with new values.

上記のコードでは、:class:`~flask.Config` オブジェクトはディクショナリと同じように動作するため、新しい値で更新することができます。

.. Database Path

    Operating systems know the concept of a current working directory for
    each process.  Unfortunately, you cannot depend on this in web
    applications because you might have more than one application in the
    same process.

    For this reason the ``app.root_path`` attribute can be used to
    get the path to the application.  Together with the ``os.path`` module,
    files can then easily be found.  In this example, we place the
    database right next to it.

    For a real-world application, it's recommended to use
    :ref:`instance-folders` instead.

.. admonition:: データベースのパス

    オペレーティングシステムは、各プロセスの現在の作業ディレクトリの場所を知っています。
    残念ながら、同じプロセス内に複数のアプリケーションが存在する可能性があるため、
    ウェブアプリケーションでは、これを使うことはできません。

    こういった理由から、``app.root_path`` アトリビュートを使ってアプリケーションのパスを取得することができます
    ``os.path`` モジュールと同様にファイルをかんたんに見つけることができます。
    今回の例ではデータベースのすぐ横に置きます。

    実際のアプリケーションでは、代わりに :ref:`instance-folders` を使うことをお勧めします。

.. Usually, it is a good idea to load a separate, environment-specific
   configuration file.  Flask allows you to import multiple configurations and it
   will use the setting defined in the last import.  This enables robust
   configuration setups.  :meth:`~flask.Config.from_envvar` can help achieve
   this. ::

通常は、環境ごとに設定ファイルを分けて読み込むことはいいことです。
Flask は複数のコンフィグをインポートでき、最後にインポートした設定が定義されて使われます。
これによって、堅牢なコンフィグの設定が可能になります。
:meth:`~flask.Config.from_envvar` はこれを満たすのに役立ちます。

   app.config.from_envvar('FLASKR_SETTINGS', silent=True)

.. If you want to do this (not required for this tutorial) simply define the
   environment variable :envvar:`FLASKR_SETTINGS` that points to a config file
   to be loaded.  The silent switch just tells Flask to not complain if no such
   environment key is set.

(このチュートリアルでは必須ではありませんが、)このようにするには、読み込んだコンフィグファイルを指す環境変数の :envvar:`FLASKR_SETTINGS` を定義します。
silent のフラグは、そのような環境変数のキーが設定されていない場合は、Flask に通知しないように伝えます。

.. In addition to that, you can use the :meth:`~flask.Config.from_object`
   method on the config object and provide it with an import name of a
   module.  Flask will then initialize the variable from that module.  Note
   that in all cases, only variable names that are uppercase are considered.

これに加えて、コンフィグオブジェクトの :meth:`~flask.Config.from_object` を使ってモジュールのインポート名を渡すことができます。
Flask はそのモジュールから変数を初期化します。
アッパーケースの変数名しか使えないことに注意して下さい。

.. The ``SECRET_KEY`` is needed to keep the client-side sessions secure.
   Choose that key wisely and as hard to guess and complex as possible.

``SECRET_KEY`` は、クライアント側のセッションをセキュアに保つために必要です。
可能な限り複雑で推測するのが難しいキーにすることは懸命なことです。

.. Lastly, add a method that allows for easy connections to the specified
   database. ::

最後に、指定したデータベースにかんたんに接続できるメソッドを追加します。 ::

    def connect_db():
        """Connects to the specific database."""

        rv = sqlite3.connect(app.config['DATABASE'])
        rv.row_factory = sqlite3.Row
        return rv

.. This can be used to open a connection on request and also from the
   interactive Python shell or a script.  This will come in handy later.
   You can create a simple database connection through SQLite and then tell
   it to use the :class:`sqlite3.Row` object to represent rows. This allows
   the rows to be treated as if they were dictionaries instead of tuples.

このコードは、リクエスト時に接続を開始する時に使われ、Python のインタラクティブシェルやスクリプトからも使われます。
こうすることで、後ほど便利になります。
SQLite を使ったシンプルなデータベース接続を作成し、行を表す :class:`sqlite3.Row` オブジェクトを使うように設定します。
これにより、行がタプルではなく辞書として扱われるようになります。

.. In the next section you will see how to run the application.

次の章で、アプリケーションの実行方法を見ていきましょう。

.. Continue with :ref:`tutorial-packaging`.

続いては :ref:`tutorial-packaging` 。
