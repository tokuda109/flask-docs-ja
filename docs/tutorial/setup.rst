.. _tutorial-setup:

ステップ2: アプリケーションのセットアップコード
===================================================

.. Step 2: Application Setup Code
   ==============================

.. Now that we have the schema in place we can create the application module.
   Let's call it `flaskr.py` inside the `flaskr` folder.  For starters we
   will add the imports we will need as well as the config section.  For
   small applications it's a possibility to drop the configuration directly
   into the module which we will be doing here.  However a cleaner solution
   would be to create a separate `.ini` or `.py` file and load that or import
   the values from there.

スキーマがある場所にアプリケーションのモジュールを作成することができます。
`flaskr` フォルダ内に、 `flaskr.py` というファイルを作成して下さい。
最初に、設定が必要なのでインポートするものを追加して下さい。
小さなアプリケーションでは、ここでするようにモジュールに直接設定を書いていくこともできます。
しかし、 `.ini` や `.py` ファイルを分割したものを作成して、そのファイルから値をインポートして読み込むこともできます。 ::

.. In `flaskr.py`::

`flaskr.py` に ::

    # all the imports
    import sqlite3
    from flask import Flask, request, session, g, redirect, url_for, \
         abort, render_template, flash

    # configuration
    DATABASE = '/tmp/flaskr.db'
    DEBUG = True
    SECRET_KEY = 'development key'
    USERNAME = 'admin'
    PASSWORD = 'default'

.. Next we can create our actual application and initialize it with the
   config from the same file, in `flaskr.py`::

次に、実際のアプリケーションを作成して、同じファイルから設定とともに初期化します。 ::

    # create our little application :)
    app = Flask(__name__)
    app.config.from_object(__name__)

.. :meth:`~flask.Config.from_object` will look at the given object (if it's a
   string it will import it) and then look for all uppercase variables
   defined there.  In our case, the configuration we just wrote a few lines
   of code above.  You can also move that into a separate file.

:meth:`~flask.Config.from_object` は、与えられたオブジェクトを検査して(文字列の場合はインポートします)
そこで定義されている全てのアッパーケースの変数を探します。
今回の場合は、上述のように設定を2〜3行書くだけです。分割したファイルにそれを移すこともできます。

.. Usually, it is a good idea to load a configuration from a configurable
   file. This is what :meth:`~flask.Config.from_envvar` can do, replacing the
   :meth:`~flask.Config.from_object` line above::

設定ファイルから設定を読み込むことは、良いアイデアです。
これは :meth:`~flask.Config.from_envvar` でも可能です。 ::

    app.config.from_envvar('FLASKR_SETTINGS', silent=True)

.. That way someone can set an environment variable called
   :envvar:`FLASKR_SETTINGS` to specify a config file to be loaded which will then
   override the default values. The silent switch just tells Flask to not complain
   if no such environment key is set.

その方法では、デフォルトの値を上書きするために読み込む設定ファイルを指定するために、 :envvar:`FLASKR_SETTINGS` という環境変数を設定することができます。
silentは、設定されていない環境変数をFlaskに伝えないように切り替わります。

.. The `secret_key` is needed to keep the client-side sessions secure.
   Choose that key wisely and as hard to guess and complex as possible.  The
   debug flag enables or disables the interactive debugger.  *Never leave
   debug mode activated in a production system*, because it will allow users to
   execute code on the server!

クライアントサイドでセッションをセキュアに保つために `secret_key` は必要です。
推測が難しくて、可能な限り複雑になるように賢くキーを選んで下さい。
デバッグフラグはインタラクティブデバッガーを有効化/無効化します。
サーバーでコードを実行することができるようになるので、プロダクションのシステムでデバッグモードをアクティブにしないで下さい。

.. We also add a method to easily connect to the database specified.  That
   can be used to open a connection on request and also from the interactive
   Python shell or a script.  This will come in handy later.

データベースに簡単に接続するためのメソッドを追加します。
リクエストやインタラクティブなPythonシェルかスクリプトからも接続することができます。
これは後で役に立つ時が来るでしょう。

::

    def connect_db():
        return sqlite3.connect(app.config['DATABASE'])

.. Finally we just add a line to the bottom of the file that fires up the
   server if we want to run that file as a standalone application::

最後に、単独のアプリケーションとしてファイルを起動する場合に、起動するためにファイルの一番下に一行追加するだけです。 ::

    if __name__ == '__main__':
        app.run()

.. With that out of the way you should be able to start up the application
   without problems.  Do this with the following command::

問題なくアプリケーションを起動できるはずなので、以下のコマンドでこれを実行して下さい。 ::

   python flaskr.py

.. You will see a message telling you that server has started along with
   the address at which you can access it.

アクセスしたアドレスでサーバーが開始されたことを示すメッセージが表示されます。

.. When you head over to the server in your browser you will get an 404
   page not found error because we don't have any views yet.  But we will
   focus on that a little later.  First we should get the database working.

まだ任意のビューがないので、404 Page Not Foundのエラーが表示されます。
しかし、少し後で取り上げます。まず、データベースを動作するようにしましょう。

.. Externally Visible Server

   Want your server to be publicly available?  Check out the
   :ref:`externally visible server <public-server>` section for more
   information.

.. admonition:: 外部から見えるサーバー

   サーバーを公開したいですか?
   より詳しいことは、 :ref:`externally visible server <public-server>` の章を確認して下さい。

.. Continue with :ref:`tutorial-dbinit`.

続いては :ref:`tutorial-dbinit` 。
