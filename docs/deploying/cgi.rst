CGI
===

.. If all other deployment methods do not work, CGI will work for sure.
   CGI is supported by all major servers but usually has a sub-optimal
   performance.

他のデプロイメソッドが全て動かないなら、CGIはきっと動くでしょう。
CGIは全ての主要なサーバーでサポートされているが、通常はパフォーマンスがよくありません。

.. This is also the way you can use a Flask application on Google's `App
   Engine`_, where execution happens in a CGI-like environment.

これはCGIみたいな環境のGoogleの `App Engine`_ 上でFlaskアプリケーションを使う場合の方法です。

.. admonition:: Watch Out

   Please make sure in advance that any ``app.run()`` calls you might
   have in your application file are inside an ``if __name__ ==
   '__main__':`` block or moved to a separate file.  Just make sure it's
   not called because this will always start a local WSGI server which
   we do not want if we deploy that application to CGI / app engine.

.. Creating a `.cgi` file
   ----------------------

`.cgi` ファイルの作成
-------------------------

.. First you need to create the CGI application file.  Let's call it
   `yourapplication.cgi`::

最初にCGIアプリケーションのファイルを作成する必要があります。
仮に、 `yourapplication.cgi` としましょう。 ::

    #!/usr/bin/python
    from wsgiref.handlers import CGIHandler
    from yourapplication import app

    CGIHandler().run(app)

.. Server Setup
   ------------

サーバーの設定
-----------------

.. Usually there are two ways to configure the server.  Either just copy the
   `.cgi` into a `cgi-bin` (and use `mod_rewrite` or something similar to
   rewrite the URL) or let the server point to the file directly.

サーバーの設定をする方法は通常は2つあります。
`cgi-bin` に `.cgi` をコピーする( `mod_rewrite` やURLを書き換える似たようなものを使う)だけか、
サーバーがファイルを直接そのファイルを指すようにするかのどちらかです。

.. In Apache for example you can put something like this into the config:

Apacheの例では、configに以下のような行を追加するだけです。 :

.. sourcecode:: apache

    ScriptAlias /app /path/to/the/application.cgi

.. On shared webhosting, though, you might not have access to your Apache config.
   In this case, a file called `.htaccess`, sitting in the public directory you want
   your app to be available, works too but the `ScriptAlias` directive won't
   work in that case:

共有のホスティングサーバーで、Apacheコンフィグにアクセスできない場合もあります。
この場合は、 `.htaccess` というファイルをアプリケーションを置いている公開ディレクトリにおいて下さい。
`ScriptAlias` ディレクティブはその場合動かないかもしれません。

.. sourcecode:: apache

    RewriteEngine On
    RewriteCond %{REQUEST_FILENAME} !-f # Don't interfere with static files
    RewriteRule ^(.*)$ /path/to/the/application.cgi/$1 [L]

.. For more information consult the documentation of your webserver.

さらなる情報はウェブサーバーのドキュメントを確認して下さい。

.. _App Engine: http://code.google.com/appengine/
