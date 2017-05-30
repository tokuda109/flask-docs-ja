.. _tutorial-packaging:

.. Step 3: Installing flaskr as a Package
   ======================================

ステップ3: パッケージとして flaskr をインストール
==================================================================

.. Flask is now shipped with built-in support for `Click`_.  Click provides
   Flask with enhanced and extensible command line utilities.  Later in this
   tutorial you will see exactly how to extend the ``flask`` command line
   interface (CLI).

Flask は `Click`_ のサポートが組み込まれた状態で公開されています。
Click は Flask に拡張可能なコマンドラインユーティリティを提供します。
このチュートリアルの後半で、``flask`` のコマンドラインインターフェース(CLI)をどのように拡張するか説明します。

.. A useful pattern to manage a Flask application is to install your app
   following the `Python Packaging Guide`_.  Presently this involves
   creating two new files; :file:`setup.py` and :file:`MANIFEST.in` in the
   projects root directory.  You also need to add an :file:`__init__.py`
   file to make the :file:`flaskr/flaskr` directory a package.  After these
   changes, your code structure should be::

Flask アプリケーションを管理するための便利な方法は、`Python Packaging Guide`_ に沿ってアプリケーションをインストールすることです。
現時点では、プロジェクトのルートディレクトリに :file:`setup.py` と :file:`MANIFEST.in` の2つの新しいファイルを作成します。
:file:`flaskr/flaskr` ディレクトリをパッケージにするために、:file:`__init__.py` ファイルを追加する必要があります。
以上の作業をしたあと、コードの構成は次のようになっていると思います。 ::

    /flaskr
        /flaskr
            __init__.py
            /static
            /templates
            flaskr.py
            schema.sql
        setup.py
        MANIFEST.in

.. Create the ``setup.py`` file for ``flaskr`` with the following content::

``flaskr`` 用に ``setup.py`` ファイルを以下の内容で作成します。 ::

    from setuptools import setup

    setup(
        name='flaskr',
        packages=['flaskr'],
        include_package_data=True,
        install_requires=[
            'flask',
        ],
    )

.. When using setuptools, it is also necessary to specify any special files
   that should be included in your package (in the :file:`MANIFEST.in`).
   In this case, the static and templates directories need to be included,
   as well as the schema.

setuptools を使う時は、パッケージに含める特別なファイル(:file:`MANIFEST.in`)を指定することも必要です。
この場合、スキーマの場合と同様に static と templates ディレクトリを含める必要があります。

.. Create the :file:`MANIFEST.in` and add the following lines::

:file:`MANIFEST.in` を作成して、次のコードを追加して下さい。 ::

    graft flaskr/templates
    graft flaskr/static
    include flaskr/schema.sql

.. Next, to simplify locating the application, create the file,
   :file:`flaskr/__init__.py` containing only the following import statement::

次にアプリケーション
次のような import 文だけの :file:`flaskr/__init__.py` ファイルを作成して下さい。 ::

    from .flaskr import app

.. This import statement brings the application instance into the top-level
   of the application package.  When it is time to run the application, the
   Flask development server needs the location of the app instance.  This
   import statement simplifies the location process.  Without the above
   import statement, the export statement a few steps below would need to be
   ``export FLASK_APP=flaskr.flaskr``.

この import 文は、アプリケーションのインスタンスをアプリケーションのパッケージのトップに保持します。
アプリケーションを実行する段階になると、Flask の開発用サーバーはアプリケーションインスタンスの場所が必要になります。
この import 文は、この処理を簡単にします。上記の import 文がない場合、
``export FLASK_APP=flaskr.flaskr`` をするために、以下のようないくつかのステップが必要になります。

.. At this point you should be able to install the application.  As usual, it
   is recommended to install your Flask application within a `virtualenv`_.
   With that said, from the ``flaskr/`` directory, go ahead and install the
   application with::

この時点で、アプリケーションをインストールできるはずです。
いつもどおり、`virtualenv`_ 内に Flask アプリケーションをインストールすることｗお勧めします。
これで ``flaskr/`` ディレクトリから、次のようにアプリケーションをインストールします。 ::

    pip install --editable .

.. The above installation command assumes that it is run within the projects
   root directory, ``flaskr/``.  The ``editable`` flag allows editing
   source code without having to reinstall the Flask app each time you make
   changes.  The flaskr app is now installed in your virtualenv (see output
   of ``pip freeze``).

上記のインストールコマンドは、プロジェクトのルートディレクトリの ``flaskr/`` 上で実行されることを想定しています。
``editable`` フラグは、変更されるたびに Flask アプリケーションを再インストールする必要がなく、ソースコードを変更することができるようになります。
これで flaskr アプリケーションは、virtualenv にインストールされました(``pip freeze`` の出力を見て下さい)。

.. With that out of the way, you should be able to start up the application.
   Do this on Mac or Linux with the following commands in ``flaskr/``::

以上で、アプリケーションを起動することができるはずです。
Mac か Linux 上の ``flaskr/`` で次のコマンドを実行して下さい。 ::

    export FLASK_APP=flaskr
    export FLASK_DEBUG=true
    flask run

.. (In case you are on Windows you need to use ``set`` instead of ``export``).
   The :envvar:`FLASK_DEBUG` flag enables or disables the interactive debugger.
   *Never leave debug mode activated in a production system*, because it will
   allow users to execute code on the server!

(Windows の場合は、``export`` の代わりに ``set`` を使って下さい)。
:envvar:`FLASK_DEBUG` フラグは、インタラクティブデバッガーの有効・無効を切り替えることができます。
ユーザーがサーバー上でコードを実行できるようになるため、本番環境のシステムではデバックモードを有効にしないで下さい!

.. You will see a message telling you that server has started along with
   the address at which you can access it in a browser.

ブラウザでアクセスできるアドレスとともにサーバーが起動したことを示すメッセージが表示されます。

.. When you head over to the server in your browser, you will get a 404 error
   because we don't have any views yet.  That will be addressed a little later,
   but first, you should get the database working.

ブラウザでサーバーにアクセスすると、ビューがまだないので、404エラーになると思います。
少し後でも取り上げますが、まずデータベースが動作するようにしなければいけません。

.. Externally Visible Server

   Want your server to be publicly available?  Check out the
   :ref:`externally visible server <public-server>` section for more
   information.

.. admonition:: 外部から見えるサーバー

   サーバーを公開しますか？
   詳しくは、:ref:`externally visible server <public-server>` の章を参照して下さい。

.. Continue with :ref:`tutorial-dbcon`.

続いては :ref:`tutorial-dbcon` 。

.. _Click: http://click.pocoo.org
.. _Python Packaging Guide: https://packaging.python.org
.. _virtualenv: https://virtualenv.pypa.io
