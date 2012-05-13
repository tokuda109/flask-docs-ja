.. _mod_wsgi-deployment:

mod_wsgi (Apache)
=================

.. If you are using the `Apache`_ webserver, consider using `mod_wsgi`_.

`Apache`_ ウェブサーバーを使っている場合、 `mod_wsgi`_ を使うことを検討して下さい。

.. Watch Out

   Please make sure in advance that any ``app.run()`` calls you might
   have in your application file are inside an ``if __name__ ==
   '__main__':`` block or moved to a separate file.  Just make sure it's
   not called because this will always start a local WSGI server which
   we do not want if we deploy that application to mod_wsgi.

.. admonition:: 注意すること

   事前に確認して下さい。
   念のために
   なぜなら、これは常にローカルのWSGIサーバー

.. _Apache: http://httpd.apache.org/

.. Installing `mod_wsgi`
   ---------------------

`mod_wsgi` のインストール
------------------------------

.. If you don't have `mod_wsgi` installed yet you have to either install it
   using a package manager or compile it yourself.  The mod_wsgi
   `installation instructions`_ cover source installations on UNIX systems.

`mod_wsgi` をまだインストールしていないなら、
パッケージマネージャーを使うか自身でコンパイルしてインストールするかしなければいけません。
mod_wsgiの `installation instructions`_ で、UNIXシステムにソースからインストールする
方法が書かれています。

.. If you are using Ubuntu/Debian you can apt-get it and activate it as
   follows:

Ubuntu/Debianを使っているなら、以下のように、apt-getを使って有効化できます。 :

.. sourcecode:: text

    # apt-get install libapache2-mod-wsgi

.. On FreeBSD install `mod_wsgi` by compiling the `www/mod_wsgi` port or by
   using pkg_add:

FreeBSDの場合、pkg_addを使うか `www/mod_wsgi` ポートをコンパイルすることで `mod_wsgi` をインストールできます。 :

.. sourcecode:: text

    # pkg_add -r mod_wsgi

.. If you are using pkgsrc you can install `mod_wsgi` by compiling the
   `www/ap2-wsgi` package.

pkgsrcを使っている場合、 `www/ap2-wsgi` パッケージをコンパイルすることで `mod_wsgi` をインストールすることができます。

.. If you encounter segfaulting child processes after the first apache
   reload you can safely ignore them.  Just restart the server.

最初にApacheリロードした後に子プロセスでセグメンテーション違反に遭遇した場合、それらを安全に無視できます。
サーバーをリスタートするだけです。

.. Creating a `.wsgi` file
   -----------------------

`.wsgi` ファイルの作成
--------------------------

.. To run your application you need a `yourapplication.wsgi` file.  This file
   contains the code `mod_wsgi` is executing on startup to get the application
   object.  The object called `application` in that file is then used as
   application.

アプリケーションを起動するには `yourapplication.wsgi` ファイルが必要です。



.. For most applications the following file should be sufficient::

ほとんどのアプリケーションは、 ::

    from yourapplication import app as application

.. If you don't have a factory function for application creation but a singleton
   instance you can directly import that one as `application`.

アプリケーションの作成のための関数を
しかし、シングルトンインスタンス

Store that file somewhere that you will find it again (e.g.:
`/var/www/yourapplication`) and make sure that `yourapplication` and all
the libraries that are in use are on the python load path.  If you don't
want to install it system wide consider using a `virtual python`_
instance.  Keep in mind that you will have to actually install your
application into the virtualenv as well.  Alternatively there is the
option to just patch the path in the `.wsgi` file before the import::

    import sys
    sys.path.insert(0, '/path/to/the/application')

.. Configuring Apache
   ------------------

Apacheの設定
------------------

The last thing you have to do is to create an Apache configuration file
for your application.  In this example we are telling `mod_wsgi` to
execute the application under a different user for security reasons:

.. sourcecode:: apache

    <VirtualHost *>
        ServerName example.com

        WSGIDaemonProcess yourapplication user=user1 group=group1 threads=5
        WSGIScriptAlias / /var/www/yourapplication/yourapplication.wsgi

        <Directory /var/www/yourapplication>
            WSGIProcessGroup yourapplication
            WSGIApplicationGroup %{GLOBAL}
            Order deny,allow
            Allow from all
        </Directory>
    </VirtualHost>

For more information consult the `mod_wsgi wiki`_.

.. _mod_wsgi: http://code.google.com/p/modwsgi/
.. _installation instructions: http://code.google.com/p/modwsgi/wiki/QuickInstallationGuide
.. _virtual python: http://pypi.python.org/pypi/virtualenv
.. _mod_wsgi wiki: http://code.google.com/p/modwsgi/wiki/

Troubleshooting
---------------

If your application does not run, follow this guide to troubleshoot:

**Problem:** application does not run, errorlog shows SystemExit ignored
    You have a ``app.run()`` call in your application file that is not
    guarded by an ``if __name__ == '__main__':`` condition.  Either
    remove that :meth:`~flask.Flask.run` call from the file and move it
    into a separate `run.py` file or put it into such an if block.

**Problem:** application gives permission errors
    Probably caused by your application running as the wrong user.  Make
    sure the folders the application needs access to have the proper
    privileges set and the application runs as the correct user
    (``user`` and ``group`` parameter to the `WSGIDaemonProcess`
    directive)

**Problem:** application dies with an error on print
    Keep in mind that mod_wsgi disallows doing anything with
    :data:`sys.stdout` and :data:`sys.stderr`.  You can disable this
    protection from the config by setting the `WSGIRestrictStdout` to
    ``off``:

    .. sourcecode:: apache

        WSGIRestrictStdout Off

    Alternatively you can also replace the standard out in the .wsgi file
    with a different stream::

        import sys
        sys.stdout = sys.stderr

**Problem:** accessing resources gives IO errors
    Your application probably is a single .py file you symlinked into
    the site-packages folder.  Please be aware that this does not work,
    instead you either have to put the folder into the pythonpath the
    file is stored in, or convert your application into a package.

    The reason for this is that for non-installed packages, the module
    filename is used to locate the resources and for symlinks the wrong
    filename is picked up.

Support for Automatic Reloading
-------------------------------

To help deployment tools you can activate support for automatic
reloading.  Whenever something changes the `.wsgi` file, `mod_wsgi` will
reload all the daemon processes for us.

For that, just add the following directive to your `Directory` section:

.. sourcecode:: apache

   WSGIScriptReloading On

.. Working with Virtual Environments
   ---------------------------------

仮想環境上で動かす
--------------------------

.. Virtual environments have the advantage that they never install the
   required dependencies system wide so you have a better control over what
   is used where.  If you want to use a virtual environment with mod_wsgi
   you have to modify your `.wsgi` file slightly.

仮想環境は、システム全体に依存しているものをインストールする必要がないというアドバンテージがあるので、
そこで使われているものの管理が簡単です。
mod_wsgiを仮想環境で使いたいなら、 `.wsgi` ファイルを修正しなければいけません。

.. Add the following lines to the top of your `.wsgi` file::

`.wsgi` ファイルの上の行に、以下の行を追加して下さい。 ::

    activate_this = '/path/to/env/bin/activate_this.py'
    execfile(activate_this, dict(__file__=activate_this))

.. This sets up the load paths according to the settings of the virtual
   environment.  Keep in mind that the path has to be absolute.

これは、仮想環境の設定を読み込むためのパスを設定します。
パスは絶対パスにしなければいけないことに注意して下さい。
