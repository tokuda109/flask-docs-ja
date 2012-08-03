.. _caching-pattern:

キャッシュする
=================

.. Caching
   =======

.. When your application runs slow, throw some caches in.  Well, at least
   it's the easiest way to speed up things.  What does a cache do?  Say you
   have a function that takes some time to complete but the results would
   still be good enough if they were 5 minutes old.  So then the idea is that
   you actually put the result of that calculation into a cache for some
   time.

アプリケーションの処理が遅くなってきた時、どこかをキャッシュして下さい。
少なくともスピードアップするための簡単な方法です。
何をキャッシュしたらいいでしょう?


.. Flask itself does not provide caching for you, but Werkzeug, one of the
   libraries it is based on, has some very basic cache support.  It supports
   multiple cache backends, normally you want to use a memcached server.

Flask自体にはキャッシュ機能がありませんが、ベースにしているライブラリの一つのWerkzeugには、
基本的なキャッシュ機能がいくつか用意されています。
それは複数のキャッシュバックエンドをサポートしていて、memcachedサーバー使いたいかもしれません。

.. Setting up a Cache
   ------------------

キャッシュ機能の設定
------------------------------------

.. You create a cache object once and keep it around, similar to how
   :class:`~flask.Flask` objects are created.  If you are using the
   development server you can create a
   :class:`~werkzeug.contrib.cache.SimpleCache` object, that one is a simple
   cache that keeps the item stored in the memory of the Python interpreter::

キャッシュオブジェクトを一度作成して、それを保持します。
:class:`~flask.Flask` オブジェクトが作成されるのと似ています。
開発サーバーを使う場合は、 :class:`~werkzeug.contrib.cache.SimpleCache` オブジェクトを作成
Pythonインタープリターのメモリ上に保管されているアイテムを簡単にキャッシュすることができます。

    from werkzeug.contrib.cache import SimpleCache
    cache = SimpleCache()

.. If you want to use memcached, make sure to have one of the memcache modules
   supported (you get them from `PyPI <http://pypi.python.org/>`_) and a
   memcached server running somewhere.  This is how you connect to such an
   memcached server then::

memcachedを使う場合、サポートされているmemcacheモジュールのどれかが必要になるので注意して下さい。
これはmemcachedサーバーのようなものに接続するために必要なものです。 ::

    from werkzeug.contrib.cache import MemcachedCache
    cache = MemcachedCache(['127.0.0.1:11211'])

.. If you are using App Engine, you can connect to the App Engine memcache
   server easily::

App Engineを使っているなら、App Engineのmemcacheサーバーに簡単に接続することができます。 ::

    from werkzeug.contrib.cache import GAEMemcachedCache
    cache = GAEMemcachedCache()

.. Using a Cache
   -------------

キャッシュを使う
--------------------------

Now how can one use such a cache?  There are two very important
operations: :meth:`~werkzeug.contrib.cache.BaseCache.get` and
:meth:`~werkzeug.contrib.cache.BaseCache.set`.  This is how to use them:

.. To get an item from the cache call
   :meth:`~werkzeug.contrib.cache.BaseCache.get` with a string as key name.
   If something is in the cache, it is returned.  Otherwise that function
   will return `None`::

キャッシュからアイテムを取得するには、
:meth:`~werkzeug.contrib.cache.BaseCache.get` のキー名に文字列を渡して呼び出します。
キャッシュされているものがあれば、それが返されます。
逆に何も無ければ、関数は `None` を返します。 ::

    rv = cache.get('my-item')

.. To add items to the cache, use the :meth:`~werkzeug.contrib.cache.BaseCache.set`
   method instead.  The first argument is the key and the second the value
   that should be set.  Also a timeout can be provided after which the cache
   will automatically remove item.

キャッシュにアイテムを追加するには、 :meth:`~werkzeug.contrib.cache.BaseCache.set` メソッドを代わりに使います。
最初の引数はキーで二番目はセットしたい値です。
キャッシュした後にタイムアウトすると自動的にアイテムは削除されます。

.. Here a full example how this looks like normally::

以下に一般的なサンプルがあります。 ::

    def get_my_item():
        rv = cache.get('my-item')
        if rv is None:
            rv = calculate_value()
            cache.set('my-item', rv, timeout=5 * 60)
        return rv
