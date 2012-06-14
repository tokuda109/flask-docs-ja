.. _deploying-uwsgi:

uWSGI
=====

.. uWSGI is a deployment option on servers like `nginx`_, `lighttpd`_, and
   `cherokee`_; see :ref:`deploying-fastcgi` and :ref:`deploying-wsgi-standalone`
   for other options.  To use your WSGI application with uWSGI protocol you will
   need a uWSGI server first. uWSGI is both a protocol and an application server;
   the application server can serve uWSGI, FastCGI, and HTTP protocols.

uWSGIは、 `nginx`_ や、 `lighttpd`_ や、 `cherokee`_ のようなサーバーのデプロイオプションです。
他のオプションについては、 :ref:`deploying-fastcgi` や、 :ref:`deploying-wsgi-standalone` を見てください。
WSGIアプリケーションでuWSGIプロトコルを使うためには、まずuWSGIサーバーが必要です。
uWSGIはプロトコルとアプリケーションサーバーの両方です。
アプリケーションサーバーは、uWSGI、FastCGI、HTTPで配信することができます。

.. The most popular uWSGI server is `uwsgi`_, which we will use for this
   guide.  Make sure to have it installed to follow along.

最も有名なuWSGIサーバーは `uwsgi`_ です。
このガイドでも使っています。
そのガイドに沿ってインストールされているか確認して下さい。

.. admonition:: Watch Out

   Please make sure in advance that any ``app.run()`` calls you might
   have in your application file are inside an ``if __name__ ==
   '__main__':`` block or moved to a separate file.  Just make sure it's
   not called because this will always start a local WSGI server which
   we do not want if we deploy that application to uWSGI.

.. Starting your app with uwsgi
   ----------------------------

uwsgiでアプリケーションを起動する
-------------------------------

.. `uwsgi` is designed to operate on WSGI callables found in python modules.

`uwsgi` は、Pythonモジュールから呼び出し可能なWSGIを探して操作するように設計されています。

.. Given a flask application in myapp.py, use the following command:

myapp.pyにFlaskアプリケーションを与えたい場合は、以下の様なコマンドを使って下さい。 :

.. sourcecode:: text

    $ uwsgi -s /tmp/uwsgi.sock --module myapp --callable app

.. Or, if you prefer:

もしくは、以下のほうがいいなら以下のようにして下さい。 :

.. sourcecode:: text

    $ uwsgi -s /tmp/uwsgi.sock -w myapp:app

.. Configuring nginx
   -----------------

nginxの設定
--------------------

.. A basic flask uWSGI configuration for nginx looks like this::

Flaskでは基本的に、以下のようなnginxのuWSGI設定になります。 ::

    location = /yourapplication { rewrite ^ /yourapplication/; }
    location /yourapplication { try_files $uri @yourapplication; }
    location @yourapplication {
      include uwsgi_params;
      uwsgi_param SCRIPT_NAME /yourapplication;
      uwsgi_modifier1 30;
      uwsgi_pass unix:/tmp/uwsgi.sock;
    }

.. This configuration binds the application to `/yourapplication`.  If you want
   to have it in the URL root it's a bit simpler because you don't have to tell
   it the WSGI `SCRIPT_NAME` or set the uwsgi modifier to make use of it::

この設定はアプリケーションを `/yourapplication` にバインドします。
URLのルートに配置したい場合は、WSGIに `SCRIPT_NAME` を教える必要はないか、
使えるようにuwsgiを修正するだけなのでさらに簡単です。 ::

    location / { try_files $uri @yourapplication; }
    location @yourapplication {
        include uwsgi_params;
        uwsgi_pass unix:/tmp/uwsgi.sock;
    }

.. _nginx: http://nginx.org/
.. _lighttpd: http://www.lighttpd.net/
.. _cherokee: http://www.cherokee-project.com/
.. _uwsgi: http://projects.unbit.it/uwsgi/
