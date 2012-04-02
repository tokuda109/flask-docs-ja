.. _tutorial:

チュートリアル
================

.. Tutorial
   ========

.. You want to develop an application with Python and Flask?  Here you have
   the chance to learn that by example.  In this tutorial we will create a
   simple microblog application.  It only supports one user that can create
   text-only entries and there are no feeds or comments, but it still
   features everything you need to get started.  We will use Flask and SQLite
   as database which comes out of the box with Python, so there is nothing
   else you need.

PythonとFlaskでアプリケーションを開発したいですか?
ここではサンプルを使って、それを学ぶことができます。このチュートリアルでは、簡単なマイクロブログのアプリケーションを作ります。
あるユーザーが文字のみのエントリーを作成できる機能のみをサポートし、フィードやコメント機能はありません。
しかし、学習のための必要な機能は全てあります。
FlaskとデータベースとしてSQLiteを使い、このデータベースはPythonに組み込みのものを使うので、他に必要なものは何もありません。

.. If you want the full sourcecode in advance or for comparison, check out
   the `example source`_.

比較のためや、拡張して使う場合に、完全なソースコードが欲しい方は、 `example source`_ を確認して下さい。

.. _example source:
   http://github.com/mitsuhiko/flask/tree/master/examples/flaskr/

.. toctree::
   :maxdepth: 2

   introduction
   folders
   schema
   setup
   dbinit
   dbcon
   views
   templates
   css
   testing
