.. _tutorial-introduction:

.. Introducing Flaskr
   ==================

Flaskrの紹介
==================

.. This tutorial will demonstrate a blogging application named Flaskr, but feel
   free to choose your own less Web-2.0-ish name ;)  Essentially, it will do the
   following things:

このチュートリアルでは、Flaskr というブログアプリケーションのデモをしますが、
Web2.0っぽい名前を自由に変更して下さい。  
基本的に以下のことをしたいと思います。 :

.. Let the user sign in and out with credentials specified in the
   configuration.  Only one user is supported.
.. When the user is logged in, they can add new entries to the page
   consisting of a text-only title and some HTML for the text.  This HTML
   is not sanitized because we trust the user here.
.. The index page shows all entries so far in reverse chronological order
   (newest on top) and the user can add new ones from there if logged in.

1. 設定ファイルで指定した認証情報でサインインとサインアウトをすることができます。
   この機能は1ユーザーのみサポートします。
2. ユーザーがログインした状態で、
   文字のみのタイトルと、いくつかのHTMLが使える本文からなるページを新しいエントリーとして追加できます。
   このHTMLは、ここではユーザーを信頼してサニタイズはしません。
3. インデックスページは全てのエントリーを逆になるように(新しいものがトップになるように)表示します。
   そして、ユーザーがログインしている場合、そのページから新しいエントリーを追加することができます。

.. SQLite3 will be used directly for this application because it's good enough
   for an application of this size.  For larger applications, however,
   it makes a lot of sense to use `SQLAlchemy`_, as it handles database
   connections in a more intelligent way, allowing you to target different
   relational databases at once and more.  You might also want to consider
   one of the popular NoSQL databases if your data is more suited for those.

このくらいのサイズのアプリケーションでは十分に使えるため、SQLite3を使います。
より大きなアプリケーションの場合は、`SQLAlchemy`_ を使うことは意味があって、
さらに良い方法でデータベース接続を処理するため、同時に異なるリレーショナルデータベースを使うことができます。
扱っているデータが適しているなら、ポピュラーな NoSQL データベースのどれかを検討してみてもいいかもしれません。

.. Here a screenshot of the final application:

最終的にアプリケーションのスクリーンショットは以下のようになります。 :

.. image:: ../_static/flaskr.png
   :align: center
   :class: screenshot
   :alt: screenshot of the final application

.. Continue with :ref:`tutorial-folders`.

続いては、:ref:`tutorial-folders` 。

.. _SQLAlchemy: https://www.sqlalchemy.org/
