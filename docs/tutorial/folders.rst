.. _tutorial-folders:

ステップ0: フォルダを作成する
===============================

.. Step 0: Creating The Folders
   ============================

.. Before we get started, let's create the folders needed for this
   application::

始める前に、このアプリケーションで必要なフォルダを作成しましょう。 ::

    /flaskr
        /static
        /templates

.. The `flaskr` folder is not a python package, but just something where we
   drop our files.  Directly into this folder we will then put our database
   schema as well as main module in the following steps.  The files inside
   the `static` folder are available to users of the application via `HTTP`.
   This is the place where css and javascript files go.  Inside the
   `templates` folder Flask will look for `Jinja2`_ templates.  The
   templates you create later in the tutorial will go in this directory.

`flaskr` フォルダはPythonのパッケージではなく、ファイルを置くための場所です。
このフォルダに以下のステップでメインモジュールとしてデータベーススキーマを直接作成していきます。
`HTTP` 経由でアプリケーションのユーザーが利用可能な `static` フォルダにファイルがあります。
これはCSSやJavaScriptファイルを置く場所です。 `templates` フォルダ内からFlaskは、 `Jinja2`_ テンプレートを探します。
チュートリアルの後で作成したテンプレートはこのディレクトリに置いて下さい。

.. Continue with :ref:`tutorial-schema`.

続いては :ref:`tutorial-schema` 。

.. _Jinja2: http://jinja.pocoo.org/2/
