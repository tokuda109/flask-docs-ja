.. _tutorial-folders:

.. Step 0: Creating The Folders
   ============================

ステップ 0: フォルダを作成する
===============================

.. It is recommended to install your Flask application within a virtualenv. Please
   read the :ref:`installation` section to set up your environment.

Flask アプリケーションを virtualenv 内にインストールすることをお勧めします。
環境を設定するために :ref:`installation` の章を読んで下さい。

.. Now that you have installed Flask, you will need to create the folders required
   for this tutorial. Your directory structure will look like this::

Flask をインストールしたら、このチュートリアルで必要なフォルダを作る必要があります。
ディレクトリ構造は以下のようになります。 ::

    /flaskr
        /flaskr
            /static
            /templates

.. The application will be installed and run as Python package.  This is the
   recommended way to install and run Flask applications.  You will see exactly
   how to run ``flaskr`` later on in this tutorial.

アプリケーションがインストールされ、Python パッケージとして実行されます。
この方法は、Flask アプリケーションをインストールして、実行する方法として推奨しています。
どうすれば ``flaskr`` を実行すればいいかを、このチュートリアルで後ほど説明します。

.. For now go ahead and create the applications directory structure.  In the next
   few steps you will be creating the database schema as well as the main module.

とりあえず、アプリケーションのディレクトリ構造を作成してみましょう。
次のステップとしては、メインモジュールと同様にデータベーススキーマを作成するくらいです。

.. As a quick side note, the files inside of the :file:`static` folder are
   available to users of the application via HTTP.  This is the place where CSS and
   JavaScript files go.  Inside the :file:`templates` folder, Flask will look for
   `Jinja2`_ templates.  You will see examples of this later on.

注意することとして、:file:`static` フォルダの中にあるファイルは、
HTTP を介してアプリケーションのユーザーが利用するものです。
このフォルダには、CSS と JavaScript ファイルがあります。
:file:`templates` フォルダの中から Flask は `Jinja2`_ のテンプレートを探します。
この例は、後々説明します。

.. For now you should continue with :ref:`tutorial-schema`.

続いては、:ref:`tutorial-schema` 。

.. _Jinja2: http://jinja.pocoo.org/
