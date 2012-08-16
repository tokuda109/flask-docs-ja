.. _deploying-fastcgi:

FastCGI
=======

.. FastCGI is a deployment option on servers like `nginx`_, `lighttpd`_, and
   `cherokee`_; see :ref:`deploying-uwsgi` and :ref:`deploying-wsgi-standalone`
   for other options.  To use your WSGI application with any of them you will need
   a FastCGI server first.  The most popular one is `flup`_ which we will use for
   this guide.  Make sure to have it installed to follow along.

FastCGIは、 `nginx`_ や `lighttpd`_ や `cherokee`_ のようなサーバー上にデプロイできます。
他の選択肢を探しているなら、 :ref:`deploying-uwsgi` や :ref:`deploying-wsgi-standalone` を見てください。
それらをWSGIアプリケーションで使うためには、まずFastCGIサーバーが必要です。
最も一般的なものは、 `flup`_ で、このガイドで使っています。

.. Watch Out

   Please make sure in advance that any ``app.run()`` calls you might
   have in your application file are inside an ``if __name__ ==
   '__main__':`` block or moved to a separate file.  Just make sure it's
   not called because this will always start a local WSGI server which
   we do not want if we deploy that application to FastCGI.

.. admonition:: 注意すること

   Please make sure in advance that any ``app.run()`` calls you might
   have in your application file are inside an ``if __name__ ==
   '__main__':`` block or moved to a separate file.  Just make sure it's
   not called because this will always start a local WSGI server which
   we do not want if we deploy that application to FastCGI.

.. Creating a `.fcgi` file
   -----------------------

`.fcgi` ファイルを作成する
--------------------------------

.. First you need to create the FastCGI server file.  Let's call it
   `yourapplication.fcgi`::

まず、FastCGIサーバーのファイルを作成しなければいけません。
`yourapplication.fcgi` としましょう! ::

    #!/usr/bin/python
    from flup.server.fcgi import WSGIServer
    from yourapplication import app

    if __name__ == '__main__':
        WSGIServer(app).run()

.. This is enough for Apache to work, however nginx and older versions of
   lighttpd need a socket to be explicitly passed to communicate with the
   FastCGI server.  For that to work you need to pass the path to the
   socket to the :class:`~flup.server.fcgi.WSGIServer`::

これは、Apacheを動かすためには十分ですが、nginxやlighttpdの古いバージョンでは、
FastCGIサーバーと明示的に通信するためのソケットが必要です。
動作させるために、 :class:`~flup.server.fcgi.WSGIServer` にソケットのパスを渡さなければいけません。

    WSGIServer(application, bindAddress='/path/to/fcgi.sock').run()

.. The path has to be the exact same path you define in the server
   config.

パスはサーバーのコンフィグで定義したパスと同じにしなければいけません。

Save the `yourapplication.fcgi` file somewhere you will find it again.
It makes sense to have that in `/var/www/yourapplication` or something
similar.

`yourapplication.fcgi` ファイルを保存して
`/var/www/yourapplication` か同じような場所に

.. Make sure to set the executable bit on that file so that the servers
   can execute it:

サーバーが実行できるようにそのファイルを実行できるように設定して下さい。 :

.. sourcecode:: text

    # chmod +x /var/www/yourapplication/yourapplication.fcgi

.. Configuring Apache
   ------------------

Apacheの設定をする
---------------------

.. The example above is good enough for a basic Apache deployment but your `.fcgi`
   file will appear in your application URL
   e.g. example.com/yourapplication.fcgi/news/. There are few ways to configure
   your application so that yourapplication.fcgi does not appear in the URL. A
   preferable way is to use the ScriptAlias configuration directive::

上のサンプルは基本的なApache環境には十分ですが、
`.fcgi` ファイルはexample.com/yourapplication.fcgi/news/のようなURLで表示されます。
yourapplication.fcgiをURLに表示させたくないので、アプリケーションの設定をそのようにする方法はいくつかあります。
いい方法の一つがScriptAlias設定を使うことです。 ::

    <VirtualHost *>
        ServerName example.com
        ScriptAlias / /path/to/yourapplication.fcgi/
    </VirtualHost>

.. If you cannot set ScriptAlias, for example on an shared web host, you can use
   WSGI middleware to remove yourapplication.fcgi from the URLs. Set .htaccess::

共有サーバーなどでScriptAliasを設定できない場合、URLからyourapplication.fcgiを除くためのWSGIミドルウェアを使うことができます。
.htaccessの設定をして下さい。 ::

    <IfModule mod_fcgid.c>
       AddHandler fcgid-script .fcgi
       <Files ~ (\.fcgi)>
           SetHandler fcgid-script
           Options +FollowSymLinks +ExecCGI
       </Files>
    </IfModule>

    <IfModule mod_rewrite.c>
       Options +FollowSymlinks
       RewriteEngine On
       RewriteBase /
       RewriteCond %{REQUEST_FILENAME} !-f
       RewriteRule ^(.*)$ yourapplication.fcgi/$1 [QSA,L]
    </IfModule>

.. Set yourapplication.fcgi::

yourapplication.fcgiを設定して下さい。 ::

    #!/usr/bin/python
    #: optional path to your local python site-packages folder
    import sys
    sys.path.insert(0, '<your_local_path>/lib/python2.6/site-packages')

    from flup.server.fcgi import WSGIServer
    from yourapplication import app

    class ScriptNameStripper(object):
       to_strip = '/yourapplication.fcgi'

       def __init__(self, app):
           self.app = app

       def __call__(self, environ, start_response):
           environ['SCRIPT_NAME'] = ''
           return self.app(environ, start_response)

    app = ScriptNameStripper(app)

    if __name__ == '__main__':
        WSGIServer(app).run()

.. Configuring lighttpd
   --------------------

lighttpdの設定をする
-----------------------

.. A basic FastCGI configuration for lighttpd looks like that::

lighttpdの基本的なFastCGIの設定は以下のようになります。 ::

    fastcgi.server = ("/yourapplication.fcgi" =>
        ((
            "socket" => "/tmp/yourapplication-fcgi.sock",
            "bin-path" => "/var/www/yourapplication/yourapplication.fcgi",
            "check-local" => "disable",
            "max-procs" => 1
        ))
    )

    alias.url = (
        "/static/" => "/path/to/your/static"
    )

    url.rewrite-once = (
        "^(/static.*)$" => "$1",
        "^(/.*)$" => "/yourapplication.fcgi$1"

.. Remember to enable the FastCGI, alias and rewrite modules. This configuration
   binds the application to `/yourapplication`.  If you want the application to
   work in the URL root you have to work around a lighttpd bug with the
   :class:`~werkzeug.contrib.fixers.LighttpdCGIRootFix` middleware.

FastCGI、alias、rewriteモジュールが有効になりました。
この設定はアプリケーションを `/yourapplication` にバインドします。
アプリケーションをURLのルートとして動かしたい場合、
:class:`~werkzeug.contrib.fixers.LighttpdCGIRootFix` ミドルウェアでlighttpdのバグの作業をする必要があります。

Make sure to apply it only if you are mounting the application the URL
root. Also, see the Lighty docs for more information on `FastCGI and Python
<http://redmine.lighttpd.net/wiki/lighttpd/Docs:ModFastCGI>`_ (note that
explicitly passing a socket to run() is no longer necessary).



.. Configuring nginx
   -----------------

nginxの設定をする
--------------------

.. Installing FastCGI applications on nginx is a bit different because by
   default no FastCGI parameters are forwarded.

nginx上でFastCGIアプリケーションをインストールすることは、
FastCGIをデフォルトでインストールする場合のパラメーターと少し異なります。

.. A basic flask FastCGI configuration for nginx looks like this::

基本的なFlaskのnginx用のFastCGI設定は以下のようになります。 ::

    location = /yourapplication { rewrite ^ /yourapplication/ last; }
    location /yourapplication { try_files $uri @yourapplication; }
    location @yourapplication {
        include fastcgi_params;
    fastcgi_split_path_info ^(/yourapplication)(.*)$;
        fastcgi_param PATH_INFO $fastcgi_path_info;
        fastcgi_param SCRIPT_NAME $fastcgi_script_name;
        fastcgi_pass unix:/tmp/yourapplication-fcgi.sock;
    }

.. This configuration binds the application to `/yourapplication`.  If you
   want to have it in the URL root it's a bit simpler because you don't
   have to figure out how to calculate `PATH_INFO` and `SCRIPT_NAME`::

この設定はアプリケーションを `/yourapplication` にバインドします。
URLのルートにしたい場合は、 `PATH_INFO` と `SCRIPT_NAME` の

    location / { try_files $uri @yourapplication; }
    location @yourapplication {
        include fastcgi_params;
        fastcgi_param PATH_INFO $fastcgi_script_name;
        fastcgi_param SCRIPT_NAME "";
        fastcgi_pass unix:/tmp/yourapplication-fcgi.sock;
    }

.. Running FastCGI Processes
   -------------------------

FastCGIのプロセスを起動する
----------------------------

Since Nginx and others do not load FastCGI apps, you have to do it by
yourself.  `Supervisor can manage FastCGI processes.
<http://supervisord.org/configuration.html#fcgi-program-x-section-settings>`_
You can look around for other FastCGI process managers or write a script
to run your `.fcgi` file at boot, e.g. using a SysV ``init.d`` script.
For a temporary solution, you can always run the ``.fcgi`` script inside
GNU screen.  See ``man screen`` for details, and note that this is a
manual solution which does not persist across system restart::

    $ screen
    $ /var/www/yourapplication/yourapplication.fcgi

.. Debugging
   ---------

デバッグする
--------------

FastCGI deployments tend to be hard to debug on most webservers.  Very
often the only thing the server log tells you is something along the
lines of "premature end of headers".  In order to debug the application
the only thing that can really give you ideas why it breaks is switching
to the correct user and executing the application by hand.

This example assumes your application is called `application.fcgi` and
that your webserver user is `www-data`::

    $ su www-data
    $ cd /var/www/yourapplication
    $ python application.fcgi
    Traceback (most recent call last):
      File "yourapplication.fcgi", line 4, in <module>
    ImportError: No module named yourapplication

In this case the error seems to be "yourapplication" not being on the
python path.  Common problems are:

-   Relative paths being used.  Don't rely on the current working directory
-   The code depending on environment variables that are not set by the
    web server.
-   Different python interpreters being used.

.. _nginx: http://nginx.org/
.. _lighttpd: http://www.lighttpd.net/
.. _cherokee: http://www.cherokee-project.com/
.. _flup: http://trac.saddi.com/flup
