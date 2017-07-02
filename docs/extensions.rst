.. _extensions:

.. Flask Extensions
   ================

Flask エクステンション
==========================

.. Flask extensions extend the functionality of Flask in various different
   ways.  For instance they add support for databases and other common tasks.

Flask のエクステンションは様々な方法で Flask の機能を拡張します。
例として、データベースのサポートや、他の一般的なタスクを追加します。

.. Finding Extensions
   ------------------

エクステンションの検索
---------------------

.. Flask extensions are listed on the `Flask Extension Registry`_ and can be
   downloaded with :command:`easy_install` or :command:`pip`.  If you add a Flask extension
   as dependency to your :file:`requirements.txt` or :file:`setup.py` file they are
   usually installed with a simple command or when your application installs.

Flask のエクステンションは、`Flask Extension Registry`_ に一覧であって、
:command:`easy_install` や :command:`pip` でダウンロードできます。
:file:`requirements.txt` や :file:`setup.py` ファイルに依存として Flask のエクステンションを追加した場合、
簡単なコマンドでインストールするか、アプリケーションのインストール時にインストールされます。

.. Using Extensions
   ----------------

エクステンションの使用
-------------------

.. Extensions typically have documentation that goes along that shows how to
   use it.  There are no general rules in how extensions are supposed to
   behave but they are imported from common locations.  If you have an
   extension called ``Flask-Foo`` or ``Foo-Flask`` it should be always
   importable from ``flask_foo``::

エクステンションは、使い方を記載しているドキュメントが普通はあります。
エクステンションがどのように動作するかという一般的なルールはありませんが、共通の場所からインポートされます。
``Flask-Foo`` や ``Foo-Flask`` という名前のエクステンションの場合、``flask_foo`` でインポート可能になっていなければいけません。::

    import flask_foo

.. Building Extensions
   -------------------

エクステンションの作成
-------------------------

.. While `Flask Extension Registry`_ contains many Flask extensions, you may not find
   an extension that fits your need. If this is the case, you can always create your own. 
   Consider reading :ref:`extension-dev` to develop your own Flask extension.

`Flask Extension Registry`_ には多くの Flask エクステンションがありますが、必要とするエクステンションが見つからないかもしれません。
このような場合、独自のものを作成することができます。
独自の Flask エクステンションを開発する場合は、:ref:`extension-dev` を読んでみて下さい。

.. Flask Before 0.8
   ----------------

Flask のバージョン0.8以前
----------------

.. If you are using Flask 0.7 or earlier the :data:`flask.ext` package will not
   exist, instead you have to import from ``flaskext.foo`` or ``flask_foo``
   depending on how the extension is distributed.  If you want to develop an
   application that supports Flask 0.7 or earlier you should still import
   from the :data:`flask.ext` package.  We provide you with a compatibility
   module that provides this package for older versions of Flask.  You can
   download it from GitHub: `flaskext_compat.py`_

Flask のバージョン0.7以下を使っている場合、:data:`flask.ext` パッケージは存在しません。
代わりに、エクステンションの配布方法によって、``flaskext.foo`` か ``flask_foo`` からインポートする必要があります。
Flask のバージョン0.7以下をサポートするアプリケーションを開発する場合は、:data:`flask.ext` パッケージからインポートする必要があります。
Flask の古いバージョン用にこのパッケージを提供していて、互換モジュールを提供します。
GitHub から `flaskext_compat.py`_ をダウンロードできます。

.. And here is how you can use it::

そして、それを使う方法は以下のとおりです。::

    import flaskext_compat
    flaskext_compat.activate()

    from flask.ext import foo

.. Once the ``flaskext_compat`` module is activated the :data:`flask.ext` will
   exist and you can start importing from there.

``flaskext_compat`` モジュールを起動して、:data:`flask.ext` が存在すると、そこからインポートすることができるようになっています。


.. _Flask Extension Registry: http://flask.pocoo.org/extensions/
.. _flaskext_compat.py: https://raw.githubusercontent.com/pallets/flask/master/scripts/flaskext_compat.py
