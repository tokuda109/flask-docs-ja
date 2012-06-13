.. _message-flashing-pattern:

フラッシュメッセージ
=======================

.. Message Flashing
   ================

.. Good applications and user interfaces are all about feedback.  If the user
   does not get enough feedback they will probably end up hating the
   application.  Flask provides a really simple way to give feedback to a
   user with the flashing system.  The flashing system basically makes it
   possible to record a message at the end of a request and access it next
   request and only next request.  This is usually combined with a layout
   template that does this.

いいアプリケーションといいユーザーインターフェースにはフィードバックが全てです。
ユーザーフィードバックを十分に得ることができなければ、アプリケーションを嫌いになるかもしれません。
Flaskは、フラッシュメッセージの仕組みを用いて本当にシンプルな方法でユーザーにフィードバックを与える方法が提供されています。
フラッシュメッセージは基本的に、リクエストの終了時に次のアクセスするページへリクエストを出して、その時だけ有効なメッセージを記録しておくことができます。
これをすることでレイアウトテンプレートに使いやすいように統合されています。

.. Simple Flashing
   ---------------

簡単なフラッシュメッセージ
-------------------------------

.. So here is a full example::

では、以下に例を紹介します。 ::

    from flask import Flask, flash, redirect, render_template, \
         request, url_for

    app = Flask(__name__)
    app.secret_key = 'some_secret'

    @app.route('/')
    def index():
        return render_template('index.html')

    @app.route('/login', methods=['GET', 'POST'])
    def login():
        error = None
        if request.method == 'POST':
            if request.form['username'] != 'admin' or \
                    request.form['password'] != 'secret':
                error = 'Invalid credentials'
            else:
                flash('You were successfully logged in')
                return redirect(url_for('index'))
        return render_template('login.html', error=error)

    if __name__ == "__main__":
        app.run()


.. And here the ``layout.html`` template which does the magic:

そして、以下のようにすると ``layout.html`` テンプレートは魔法を使うことができます。 :

.. sourcecode:: html+jinja

   <!doctype html>
   <title>My Application</title>
   {% with messages = get_flashed_messages() %}
     {% if messages %}
       <ul class=flashes>
       {% for message in messages %}
         <li>{{ message }}</li>
       {% endfor %}
       </ul>
     {% endif %}
   {% endwith %}
   {% block body %}{% endblock %}

.. And here the index.html template:

そして、以下はindex.htmlテンプレートです。 :

.. sourcecode:: html+jinja

   {% extends "layout.html" %}
   {% block body %}
     <h1>Overview</h1>
     <p>Do you want to <a href="{{ url_for('login') }}">log in?</a>
   {% endblock %}

.. And of course the login template:

そして、もちろんログインテンプレートもです。 :

.. sourcecode:: html+jinja

   {% extends "layout.html" %}
   {% block body %}
     <h1>Login</h1>
     {% if error %}
       <p class=error><strong>Error:</strong> {{ error }}
     {% endif %}
     <form action="" method=post>
       <dl>
         <dt>Username:
         <dd><input type=text name=username value="{{
             request.form.username }}">
         <dt>Password:
         <dd><input type=password name=password>
       </dl>
       <p><input type=submit value=Login>
     </form>
   {% endblock %}

.. Flashing With Categories
   ------------------------

カテゴリ付きのフラッシュメッセージ
----------------------------------------

.. versionadded:: 0.3

.. It is also possible to provide categories when flashing a message.  The
   default category if nothing is provided is ``'message'``.  Alternative
   categories can be used to give the user better feedback.  For example
   error messages could be displayed with a red background.

フラッシュメッセージを記録するときにカテゴリを追加することも可能です。
指定をしなければ、デフォルトのカテゴリは ``'message'`` になります。
他のカテゴリはユーザーにとっていいと思われるフィードバックを与えるために使って下さい。
例えば、エラーメッセージを赤い背景で表示させる場合などです。

.. To flash a message with a different category, just use the second argument
   to the :func:`~flask.flash` function::

異なるカテゴリでメッセージを記録するためには、 :func:`~flask.flash` 関数の二番目の引数を使って下さい。 ::

    flash(u'Invalid password provided', 'error')

.. Inside the template you then have to tell the
   :func:`~flask.get_flashed_messages` function to also return the
   categories.  The loop looks slightly different in that situation then:

テンプレート内で、 :func:`~flask.get_flashed_messages` 関数がカテゴリを返すように指示しなければいけません。
その場合にループ処理は少し違う形式になります。

.. sourcecode:: html+jinja

   {% with messages = get_flashed_messages(with_categories=true) %}
     {% if messages %}
       <ul class=flashes>
       {% for category, message in messages %}
         <li class="{{ category }}">{{ message }}</li>
       {% endfor %}
       </ul>
     {% endif %}
   {% endwith %}

.. This is just one example of how to render these flashed messages.  One
   might also use the category to add a prefix such as
   ``<strong>Error:</strong>`` to the message.

これは、フラッシュメッセージを表示するための一つの例でしかありません。
メッセージに ``<strong>Error:</strong>`` のような接頭詞を追加するためにカテゴリを使う場合もあるかもしれません。

.. Filtering Flash Messages
   ------------------------

フラッシュメッセージのフィルタリング
---------------------------------------

.. versionadded:: 0.9

.. Optionally you can pass a list of categories which filters the results of
   :func:`~flask.get_flashed_messages`.  This is useful if you wish to
   render each category in a separate block.

任意で、 :func:`~flask.get_flashed_messages` の結果をフィルタリングするカテゴリのリストを渡すことができます。
これは異なるブロックに個々のカテゴリを表示させたい場合に便利です。

.. sourcecode:: html+jinja

    {% with errors = get_flashed_messages(category_filter=["error"]) %}
    {% if errors %}
    <div class="alert-message block-message error">
      <a class="close" href="#">×</a>
      <ul>
        {%- for msg in errors %}
        <li>{{ msg }}</li>
        {% endfor -%}
      </ul>
    </div>
    {% endif %}
    {% endwith %}
