.. _tutorial:

.. Tutorial
   ========

チュートリアル
================

.. Learn by example to develop an application with Python and Flask.

PythonとFlaskを使ったアプリケーションの開発をするための例を学んでみましょう。

.. In this tutorial, we will create a simple blogging application. It only
   supports one user, only allows text entries, and has no feeds or comments.

このチュートリアルでは、簡単なブログアプリケーションを作りたいと思います。
1ユーザーのみサポートし、文字列のみの記事を投稿できて、フィードやコメント機能はありません。

.. While very simple, this example still features everything you need to get
   started. In addition to Flask, we will use SQLite for the database, which is
   built-in to Python, so there is nothing else you need.

とても簡単なアプリケーションですが、学ぶために必要な機能は全て含まれています。
Flaskに加えて、データベースに Python に組み込みに入っている SQLite を使うので、他に必要なものはありません。

.. If you want the full source code in advance or for comparison, check out
   the `example source`_.

比較のためや拡張して使う場合の、全てのソースコードが欲しい方は、 `example source`_ を確認して下さい。

.. _example source: https://github.com/pallets/flask/tree/master/examples/flaskr/

.. toctree::
   :maxdepth: 2

   introduction
   folders
   schema
   setup
   packaging
   dbcon
   dbinit
   views
   templates
   css
   testing
