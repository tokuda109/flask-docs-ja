.. _deployment:

デプロイオプション
=====================

.. Deployment Options
   ==================

.. Depending on what you have available there are multiple ways to run
   Flask applications.  You can use the builtin server during development,
   but you should use a full deployment option for production applications.
   (Do not use the builtin development server in production.)  Several
   options are available and documented here.

利用可能なものに応じて、Flaskアプリケーションを起動する複数の方法があります。
開発中は組み込みのサーバーを使いますが、プロダクション環境のアプリケーションに対して
全てのデプロイオプションを使うべきです(プロダクション環境では組み込みの開発サーバーは使わないで下さい)。
いくつかのオプションが利用可能で、ここでドキュメント化しています。

.. If you have a different WSGI server look up the server documentation
   about how to use a WSGI app with it.  Just remember that your
   :class:`Flask` application object is the actual WSGI application.

異なるWSGIサーバーがあって、WSGIアプリケーションをWSGIサーバーと共に使う方法についてのいくつかのドキュメントを探している。
:class:`Flask` アプリケーションオブジェクトは実際にはWSGIアプリケーションだということを覚えておいて下さい。

.. For hosted options to get up and running quickly, see
   :ref:`quickstart_deployment` in the Quickstart.

準備をして手っ取り早く起動するためのオプション
クイックスタートの :ref:`quickstart_deployment` を見て下さい。

.. toctree::
   :maxdepth: 2

   mod_wsgi
   wsgi-standalone
   uwsgi
   fastcgi
   cgi
