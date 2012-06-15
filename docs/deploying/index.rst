.. _deployment:

デプロイの組み合わせ
=====================

.. Deployment Options
   ==================

.. Depending on what you have available there are multiple ways to run
   Flask applications.  You can use the builtin server during development,
   but you should use a full deployment option for production applications.
   (Do not use the builtin development server in production.)  Several
   options are available and documented here.

利用可能なものに応じて、Flaskアプリケーションを起動する複数の方法があります。
開発中は備え付けのサーバーを使いますが、
本番環境のアプリケーションに対してデプロイするものを全て組み合わせて使うべきです(本番環境では備え付けの開発サーバーは使わないで下さい)。
いくつかの組み合わせが利用可能で、この章で文章化されています。

.. If you have a different WSGI server look up the server documentation
   about how to use a WSGI app with it.  Just remember that your
   :class:`Flask` application object is the actual WSGI application.

異なるWSGIサーバーを使用していて、WSGIアプリケーションをWSGIサーバーと共に使う方法について、サーバーのドキュメントを調べて下さい。
:class:`Flask` アプリケーションオブジェクトは、実際にはWSGIアプリケーションでしかないということを覚えておいて下さい。

.. For hosted options to get up and running quickly, see
   :ref:`quickstart_deployment` in the Quickstart.

起ち上げて、手っ取り早く起動するための組み合わせについては、
クイックスタートの :ref:`quickstart_deployment` を見て下さい。

.. toctree::
   :maxdepth: 2

   mod_wsgi
   wsgi-standalone
   uwsgi
   fastcgi
   cgi
