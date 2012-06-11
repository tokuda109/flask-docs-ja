.. _larger-applications:

大きなアプリケーション
======================

.. Larger Applications
   ===================

.. For larger applications it's a good idea to use a package instead of a
   module.  That is quite simple.  Imagine a small application looks like
   this::

大きなアプリケーションでは、モジュールの代わりにパッケージを使うことはいいことです。
とても簡単です。以下のような小さなアプリケーションを想像してみてください。 ::

    /yourapplication
        /yourapplication.py
        /static
            /style.css
        /templates
            layout.html
            index.html
            login.html
            ...

.. Simple Packages
   ---------------

シンプルなパッケージ
-----------------------

.. To convert that into a larger one, just create a new folder
   `yourapplication` inside the existing one and move everything below it.
   Then rename `yourapplication.py` to `__init__.py`.  (Make sure to delete
   all `.pyc` files first, otherwise things would most likely break)

大きなパッケージに置き換えるには、 `yourapplication` などの新しいフォルダを作って、
今あるファイルをそのフォルダ内に移動させるだけです。
それから、 `yourapplication.py` を `__init__.py` に修正して下さい。
(最初に全ての `.pyc` ファイルを削除して下さい。それをしない場合動かなくなるかもしれません。)

.. You should then end up with something like that::

以下のようになっているはずです。 ::

    /yourapplication
        /yourapplication
            /__init__.py
            /static
                /style.css
            /templates
                layout.html
                index.html
                login.html
                ...

.. But how do you run your application now?  The naive ``python
   yourapplication/__init__.py`` will not work.  Let's just say that Python
   does not want modules in packages to be the startup file.  But that is not
   a big problem, just add a new file called `runserver.py` next to the inner
   `yourapplication` folder with the following contents::

しかし、どうやってアプリケーションを実行しますか?
ネイティブの ``python yourapplication/__init__.py`` は動作しません。
Pythonに起動するファイルであるパッケージにモジュールが必要なことを教えなければいけません。
しかし、大きな問題ではありません。
以下のコンテンツで `yourapplication` フォルダ内に `runserver.py` という新しいファイルを追加するだけです。

    from yourapplication import app
    app.run(debug=True)

.. What did we gain from this?  Now we can restructure the application a bit
   into multiple modules.  The only thing you have to remember is the
   following quick checklist:

これはどのような進展があったのでしょうか?
アプリケーションの構造を複数のモジュールに分割しました。
以下の簡単なチェックリストを押さえておくだけです。 :

1. the `Flask` application object creation has to be in the
   `__init__.py` file.  That way each module can import it safely and the
   `__name__` variable will resolve to the correct package.
2. all the view functions (the ones with a :meth:`~flask.Flask.route`
   decorator on top) have to be imported in the `__init__.py` file.
   Not the object itself, but the module it is in. Import the view module
   **after the application object is created**.

.. Here's an example `__init__.py`::

`__init__.py` の例です。 ::

    from flask import Flask
    app = Flask(__name__)

    import yourapplication.views

.. And this is what `views.py` would look like::

そして、 `views.py` は以下のようにします。 ::

    from yourapplication import app

    @app.route('/')
    def index():
        return 'Hello World!'

.. You should then end up with something like that::

以下のようになっているはずです。 ::

    /yourapplication
        /runserver.py
        /yourapplication
            /__init__.py
            /views.py
            /static
                /style.css
            /templates
                layout.html
                index.html
                login.html
                ...

.. admonition:: Circular Imports

   Every Python programmer hates them, and yet we just added some:
   circular imports (That's when two modules depend on each other.  In this
   case `views.py` depends on `__init__.py`).  Be advised that this is a
   bad idea in general but here it is actually fine.  The reason for this is
   that we are not actually using the views in `__init__.py` and just
   ensuring the module is imported and we are doing that at the bottom of
   the file.

   There are still some problems with that approach but if you want to use
   decorators there is no way around that.  Check out the
   :ref:`becomingbig` section for some inspiration how to deal with that.


.. _working-with-modules:

Blueprintsを使う
--------------------------

.. Working with Blueprints
   -----------------------

.. If you have larger applications it's recommended to divide them into
   smaller groups where each group is implemented with the help of a
   blueprint.  For a gentle introduction into this topic refer to the
   :ref:`blueprints` chapter of the documentation.

より大きなアプリケーションの場合、小さなグループに分けて、
個々のグループ毎にBlueprintsを使って実装することをお勧めします。
このトピックをゆっくりと導入のためには、ドキュメントの :ref:`blueprints` の章を参照して下さい。
