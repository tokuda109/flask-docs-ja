.. Flask Extensions
   ================

Flaskエクステンション
==========================

.. Flask extensions extend the functionality of Flask in various different
   ways.  For instance they add support for databases and other common tasks.

Flaskエクステンションは様々な異なる方法で、Flaskの機能を拡張します。
例えば、データベースのサポートや他の一般的な処理を追加します。

.. Finding Extensions
   ------------------

エクステンションの探し方
------------------------------

.. Flask extensions are listed on the `Flask Extension Registry`_ and can be
   downloaded with ``easy_install`` or ``pip``.  If you add a Flask extension
   as dependency to your ``requirements.rst`` or ``setup.py`` file they are
   usually installed with a simple command or when your application installs.

Flaskエクステンションは、 `Flask Extension Registry`_ にリストされていて、
``easy_install`` や ``pip`` でダウンロードすることができます。
Flaskエクステンションを ``requirements.rst`` や ``setup.py`` ファイルに依存関係として追加する場合、
通常は簡単なコマンドでインストールするかアプリケーションがインストールされた時にインストールされます。

.. Using Extensions
   ----------------

エクステンションの使い方
------------------------------

.. Extensions typically have documentation that goes along that shows how to
   use it.  There are no general rules in how extensions are supposed to
   behave but they are imported from common locations.  If you have an
   extension called ``Flask-Foo`` or ``Foo-Flask`` it will be always
   importable from ``flask.ext.foo``::

エクステンションは通常、使い方を示すドキュメントと一緒になっています。
エクステンションがどのような振る舞いを一般的なルールはありませんが、一般的な場所からインポート
``Flask-Foo`` や ``Foo-Flask`` のようなエクステンションがある場合、 ``flask.ext.foo`` としてインポートすることができます。 ::

    from flask.ext import foo

.. Flask Before 0.8
   ----------------

バージョン0.8以前のFlask
--------------------------------

.. If you are using Flask 0.7 or earlier the :data:`flask.ext` package will not
   exist, instead you have to import from ``flaskext.foo`` or ``flask_foo``
   depending on how the extension is distributed.  If you want to develop an
   application that supports Flask 0.7 or earlier you should still import
   from the :data:`flask.ext` package.  We provide you with a compatibility
   module that provides this package for older versions of Flask.  You can
   download it from github: `flaskext_compat.py`_

Flask 0.7 を使っているなら、 :data:`flask.ext` パッケージはまだありません。
代わりに ``flaskext.foo`` か ``flask_foo`` としてインポートしなければいけません。
Flask 0.7 以前をサポートするアプリケーションを開発する場合は、まだ、 :data:`flask.ext` パッケージからインポートするべきです。
このパッケージはFlaskの古いバージョンのための互換性のあるモジュールを提供しています。
`flaskext_compat.py`_ をgithubからダウンロードできます。

.. And here is how you can use it::

そして、使い方は以下のとおりです。 ::

    import flaskext_compat
    flaskext_compat.activate()

    from flask.ext import foo

.. Once the ``flaskext_compat`` module is activated the :data:`flask.ext` will
   exist and you can start importing from there.

``flaskext_compat`` モジュールを有効化したなら、 :data:`flask.ext` は存在し、そこからインポートすることができます。

.. _Flask Extension Registry: http://flask.pocoo.org/extensions/
.. _flaskext_compat.py: https://github.com/mitsuhiko/flask/raw/master/scripts/flaskext_compat.py
