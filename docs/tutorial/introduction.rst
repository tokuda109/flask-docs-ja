.. _tutorial-introduction:

Flaskrの紹介
==================

.. Introducing Flaskr
   ==================

.. We will call our blogging application flaskr here, feel free to choose a
   less web-2.0-ish name ;)  Basically we want it to do the following things:

ブログアプリケーションをここではflaskrと呼びます。Web2.0らしい名前です。
基本的に以下のことをしたいと思います。 :

.. let the user sign in and out with credentials specified in the
   configuration.  Only one user is supported.
.. when the user is logged in they can add new entries to the page
   consisting of a text-only title and some HTML for the text.  This HTML
   is not sanitized because we trust the user here.
.. the page shows all entries so far in reverse order (newest on top) and
   the user can add new ones from there if logged in.

1. 設定したユーザーのみが認証機能でサインインとサインアウトをすることができます。
   この機能はユーザー一人だけサポートします。
2. ユーザーがログインした状態で、文字のみのタイトルといくつかのHTMLが使える本文からなるページを新しいエントリーとして追加できます。
   このHTMLは、ここではユーザーを信頼してサニタイズはしません。
3. ページは全てのエントリーを逆になるように(新しいものがトップになるように)表示します。
   そして、ユーザーがログインしているなら、そこから新しいエントリーを追加することができます。

.. We will be using SQLite3 directly for that application because it's good
   enough for an application of that size.  For larger applications however
   it makes a lot of sense to use `SQLAlchemy`_ that handles database
   connections in a more intelligent way, allows you to target different
   relational databases at once and more.  You might also want to consider
   one of the popular NoSQL databases if your data is more suited for those.

このくらいのサイズのアプリケーションでは十分に使えるので、そのアプリケーションではSQLite3を使います。
より大きなアプリケーションには、より賢い方法でデータベース接続を処理することができる `SQLAlchemy`_ を使うことをお勧めします。
データを、より良いデータベースの組み合わせで使いたい場合は、人気のNoSQLデータベースのどれかを考慮したいかもしれません。

.. Here a screenshot from the final application:

最終的にアプリケーションのスクリーンショットは以下のようになります。 :

.. image:: ../_static/flaskr.png
   :align: center
   :class: screenshot
   :alt: screenshot of the final application

.. Continue with :ref:`tutorial-folders`.

続いては :ref:`tutorial-folders` 。

.. _SQLAlchemy: http://www.sqlalchemy.org/
