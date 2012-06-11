.. _patterns:

Flaskのパターン
=====================

.. Patterns for Flask
   ==================

.. Certain things are common enough that the chances are high you will find
   them in most web applications.  For example quite a lot of applications
   are using relational databases and user authentication.  In that case,
   chances are they will open a database connection at the beginning of the
   request and get the information of the currently logged in user.  At the
   end of the request, the database connection is closed again.

特定の事をしたい場合は、ほとんどのウェブアプリケーションで見つけることができる可能性が十分にあります。
リレーショナルデータベースやユーザー認証を使っているアプリケーションはとてもたくさん例があります。
その場合には、リクエストの開始時にデータベース接続を開始してくれたり、
現在ログインしているユーザーの情報を取得できたりします。
リクエストの最後でデータベースの接続を終了してくれます。

.. There are more user contributed snippets and patterns in the `Flask
   Snippet Archives <http://flask.pocoo.org/snippets/>`_.

ユーザーが提供してくれたスニペットやパターンが `Flask Snippet Archives <http://flask.pocoo.org/snippets/>`_ にあります。

.. toctree::
   :maxdepth: 2

   packages
   appfactories
   appdispatch
   urlprocessors
   distribute
   fabric
   sqlite3
   sqlalchemy
   fileuploads
   caching
   viewdecorators
   wtforms
   templateinheritance
   flashing
   jquery
   errorpages
   lazyloading
   mongokit
   favicon
   streaming
   deferredcallbacks
