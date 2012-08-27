.. _views:

拡張可能なビュー
==================

.. Pluggable Views
   ===============

.. versionadded:: 0.7

.. Flask 0.7 introduces pluggable views inspired by the generic views from
   Django which are based on classes instead of functions.  The main
   intention is that you can replace parts of the implementations and this
   way have customizable pluggable views.

Flask 0.7 ではビュー関数の代わりにDjangoのクラスベースの汎用ビューにインスパイアされた拡張可能なビューが導入されています。
主な目的として、カスタマイズ可能な拡張可能なビューを使って、実装の一部分を置き換えることができます。

.. Basic Principle
   ---------------

基本的な原理
------------------

.. Consider you have a function that loads a list of objects from the
   database and renders into a template::

データベースからオブジェクトの一覧を取得してテンプレートにレンダリングする関数があります。 ::

    @app.route('/users/')
    def show_users(page):
        users = User.query.all()
        return render_template('users.html', users=users)

.. This is simple and flexible, but if you want to provide this view in a
   generic fashion that can be adapted to other models and templates as well
   you might want more flexibility.  This is where pluggable class-based
   views come into place.  As the first step to convert this into a class
   based view you would do this::

これはシンプルでフレキシブルですが、他のモデルとテンプレートに適合させることができる汎用的な方法でこのビューを使う場合は、
さらにフレキシブルにしたいと思うかもしれません。
拡張可能なクラスベースのビューどこに実装していけばいいでしょうか。
クラスベースのビューに切り替えるための最初のステップとして以下のようにして下さい。 ::

    from flask.views import View

    class ShowUsers(View):

        def dispatch_request(self):
            users = User.query.all()
            return render_template('users.html', objects=users)

    app.add_url_rule('/users/', view_func=ShowUsers.as_view('show_users'))

.. As you can see what you have to do is to create a subclass of
   :class:`flask.views.View` and implement
   :meth:`~flask.views.View.dispatch_request`.  Then we have to convert that
   class into an actual view function by using the
   :meth:`~flask.views.View.as_view` class method.  The string you pass to
   that function is the name of the endpoint that view will then have.  But
   this by itself is not helpful, so let's refactor the code a bit::

見て分かるように、 :class:`flask.views.View` のサブクラスを作って、
:meth:`~flask.views.View.dispatch_request` を実装して下さい。
それから、 :meth:`~flask.views.View.as_view` クラスメソッドを使って現在のビュー関数をクラスに修正しなければいけません。
その関数に渡す文字列は、ビューのエンドポイントの名前です。
しかし、これだけでは使えないので、コードを少し修正して下さい。 ::

    from flask.views import View

    class ListView(View):

        def get_template_name(self):
            raise NotImplementedError()

        def render_template(self, context):
            return render_template(self.get_template_name(), **context)

        def dispatch_request(self):
            context = {'objects': self.get_objects()}
            return self.render_template(context)

    class UserView(ListView):

        def get_template_name(self):
            return 'users.html'

        def get_objects(self):
            return User.query.all()

.. This of course is not that helpful for such a small example, but it's good
   enough to explain the basic principle.  When you have a class-based view
   the question comes up what `self` points to.  The way this works is that
   whenever the request is dispatched a new instance of the class is created
   and the :meth:`~flask.views.View.dispatch_request` method is called with
   the parameters from the URL rule.  The class itself is instantiated with
   the parameters passed to the :meth:`~flask.views.View.as_view` function.
   For instance you can write a class like this::

もちろんこれは簡単な例なので役には立ちませんが、基本的な原理を説明するのには十分です。
クラスベースのビューを使うときに `self` が何を指しているのかという質問がよくあります。
この仕組みは、クラスの新しいインスタンスに処理が渡ってきてリクエストが作成された時や、
:meth:`~flask.views.View.dispatch_request` メソッドがURLのルールからパラメータと共に呼ばれた時に処理されます。
クラス自体は :meth:`~flask.views.View.as_view` 関数に渡されたパラメータでインスタンス化されます。
インスタンス化するために以下のようにクラスを書いて下さい。 ::

    class RenderTemplateView(View):
        def __init__(self, template_name):
            self.template_name = template_name
        def dispatch_request(self):
            return render_template(self.template_name)

.. And then you can register it like this::

そして、以下のように登録して下さい。 ::

    app.add_url_rule('/about', view_func=RenderTemplateView.as_view(
        'about_page', template_name='about.html'))

.. Method Hints
   ------------

メソッドのヒント
-----------------

.. Pluggable views are attached to the application like a regular function by
   either using :func:`~flask.Flask.route` or better
   :meth:`~flask.Flask.add_url_rule`.  That however also means that you would
   have to provide the names of the HTTP methods the view supports when you
   attach this.  In order to move that information to the class you can
   provide a :attr:`~flask.views.View.methods` attribute that has this
   information::

拡張可能なビューは、 :func:`~flask.Flask.route` や、よりベターな :meth:`~flask.Flask.add_url_rule` の
どちらかを使って、普通の関数のようにアプリケーションに付け加えることができます。
しかし、拡張可能なビューを付け加える時に、HTTPのメソッドをビューが使えるように実装しなければいけません。
クラスにその情報を渡すためには以下のように :attr:`~flask.views.View.methods` を使います。 ::

    class MyView(View):
        methods = ['GET', 'POST']

        def dispatch_request(self):
            if request.method == 'POST':
                ...
            ...

    app.add_url_rule('/myview', view_func=MyView.as_view('myview'))

.. Method Based Dispatching
   ------------------------

メソッドベースのディスパッチング
------------------------------------------

.. For RESTful APIs it's especially helpful to execute a different function
   for each HTTP method.  With the :class:`flask.views.MethodView` you can
   easily do that.  Each HTTP method maps to a function with the same name
   (just in lowercase)::

RESTful APIは、HTTPメソッド毎に違う関数を実行しやすくなります。
:class:`flask.views.MethodView` で簡単に実装できます。
HTTPメソッド毎に同じ名前(小文字で)の関数をマッピングします。 ::

    from flask.views import MethodView

    class UserAPI(MethodView):

        def get(self):
            users = User.query.all()
            ...

        def post(self):
            user = User.from_form_data(request.form)
            ...

    app.add_url_rule('/users/', view_func=UserAPI.as_view('users'))

.. That way you also don't have to provide the
   :attr:`~flask.views.View.methods` attribute.  It's automatically set based
   on the methods defined in the class.

その場合は、 :attr:`~flask.views.View.methods` アトリビュートを実装する必要はありません。
クラスに定義されたメソッドをベースにして自動的にセットされます。

.. Decorating Views
   ----------------

デコレータービュー
-------------------

.. Since the view class itself is not the view function that is added to the
   routing system it does not make much sense to decorate the class itself.
   Instead you either have to decorate the return value of
   :meth:`~flask.views.View.as_view` by hand::

ビュークラス自体はビュー関数ではないので、ルーティングシステムに追加するためにクラス自体をデコレートする意味はあまりありません。
代わりに、手動で :meth:`~flask.views.View.as_view` の値を返すデコレーターを実装する必要があります。 ::

    def user_required(f):
        """Checks whether user is logged in or raises error 401."""
        def decorator(*args, **kwargs):
            if not g.user:
                abort(401)
            return f(*args, **kwargs)
        return decorator

    view = user_required(UserAPI.as_view('users'))
    app.add_url_rule('/users/', view_func=view)

.. Starting with Flask 0.8 there is also an alternative way where you can
   specify a list of decorators to apply in the class declaration::

Flask 0.8 では、クラスに適用するためのデコレーターのリストを指定する別の方法もあります。 ::

    class UserAPI(MethodView):
        decorators = [user_required]

.. Due to the implicit self from the caller's perspective you cannot use
   regular view decorators on the individual methods of the view however,
   keep this in mind.

ビューの個々のメソッドで、通常のビューデコレーターを使用することはできません。
呼び出し元の観点からの暗黙的な自己のためにしかし、これを頭に入れておきましょう。

.. Method Views for APIs
   ---------------------

API用のメソッドビュー
------------------------

.. Web APIs are often working very closely with HTTP verbs so it makes a lot
   of sense to implement such an API based on the
   :class:`~flask.views.MethodView`.  That said, you will notice that the API
   will require different URL rules that go to the same method view most of
   the time.  For instance consider that you are exposing a user object on
   the web:

ウェブのAPIはHTTPと非常に似ているので、 :class:`~flask.views.MethodView` を使って
APIベースみたいに実装することはとてもいいことです。
つまり、APIは異なるURLルールを要求するということに気づくでしょう。
例えば、ウェブ上でユーザーオブジェクトを公開することを考えてみて下さい。 :

.. =============== =============== ======================================
   URL             Method          Description
   --------------- --------------- --------------------------------------
   ``/users/``     ``GET``         Gives a list of all users
   ``/users/``     ``POST``        Creates a new user
   ``/users/<id>`` ``GET``         Shows a single user
   ``/users/<id>`` ``PUT``         Updates a single user
   ``/users/<id>`` ``DELETE``      Deletes a single user
   =============== =============== ======================================

=============== =============== ======================================
URL             Method          説明
--------------- --------------- --------------------------------------
``/users/``     ``GET``         全ユーザーのリストを与える
``/users/``     ``POST``        新しいユーザーを作成する
``/users/<id>`` ``GET``         一人のユーザーを表示する
``/users/<id>`` ``PUT``         一人のユーザーをアップデートする
``/users/<id>`` ``DELETE``      一人のユーザーを削除する
=============== =============== ======================================

.. So how would you go about doing that with the
   :class:`~flask.views.MethodView`?  The trick is to take advantage of the
   fact that you can provide multiple rules to the same view.

:class:`~flask.views.MethodView` でどのようにするべきでしょうか?
方法は、同じビューに複数のルールを実装することができるので、それを使うことです。

.. Let's assume for the moment the view would look like this::

ビューを少しの間だけ以下のように仮定してみましょう。 ::

    class UserAPI(MethodView):

        def get(self, user_id):
            if user_id is None:
                # return a list of users
                pass
            else:
                # expose a single user
                pass

        def post(self):
            # create a new user
            pass

        def delete(self, user_id):
            # delete a single user
            pass

        def put(self, user_id):
            # update a single user
            pass

.. So how do we hook this up with the routing system?  By adding two rules
   and explicitly mentioning the methods for each::

ではどのようにしてこれをルーティングでフックするのでしょうか?
ルールを二つ追加して、それぞれのメソッドに明示的に処理します。 ::

    user_view = UserAPI.as_view('user_api')
    app.add_url_rule('/users/', defaults={'user_id': None},
                     view_func=user_view, methods=['GET',])
    app.add_url_rule('/users/', view_func=user_view, methods=['POST',])
    app.add_url_rule('/users/<int:user_id>', view_func=user_view,
                     methods=['GET', 'PUT', 'DELETE'])

.. If you have a lot of APIs that look similar you can refactor that
   registration code::

同じようなAPIがたくさんある場合は、登録するためのコードを作って書きなおすことができます。 ::

    def register_api(view, endpoint, url, pk='id', pk_type='int'):
        view_func = view.as_view(endpoint)
        app.add_url_rule(url, defaults={pk: None},
                         view_func=view_func, methods=['GET',])
        app.add_url_rule(url, view_func=view_func, methods=['POST',])
        app.add_url_rule('%s<%s:%s>' % (url, pk_type, pk), view_func=view_func,
                         methods=['GET', 'PUT', 'DELETE'])

    register_api(UserAPI, 'user_api', '/users/', pk='user_id')
