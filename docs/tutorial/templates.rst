.. _tutorial-templates:

.. Step 7: The Templates
   =====================

ステップ 7: テンプレート
========================

.. Now it is time to start working on the templates.  As you may have
   noticed, if you make requests with the app running, you will get
   an exception that Flask cannot find the templates.  The templates
   are using `Jinja2`_ syntax and have autoescaping enabled by
   default.  This means that unless you mark a value in the code with
   :class:`~flask.Markup` or with the ``|safe`` filter in the template,
   Jinja2 will ensure that special characters such as ``<`` or ``>`` are
   escaped with their XML equivalents.

テンプレートの作業をしていきましょう。
すでに気づいているかもしれませんが、アプリの起動してアクセスすると、
Flask がテンプレートを見つけることができないという例外が発生します。
テンプレートは `Jinja2`_ のシンタックスが使われていて、デフォルトで自動でエスケープする機能が有効になっています。
:class:`~flask.Markup` かテンプレート内で ``|safe`` フィルターのどちらかを使ってコード内の値に指定しない限り、
Jinja2 は ``<`` や ``>`` のような特殊文字がXMLの時と同様にエスケープすることを保証します。

.. We are also using template inheritance which makes it possible to reuse
   the layout of the website in all pages.

テンプレートの継承を使って、全てのページでウェブサイトのレイアウトを再利用をすることができます。

.. Create the follwing three HTML files and place them in the
   :file:`templates` folder:

以下の3つのHTMLファイルを作って、:file:`templates` フォルダに置いて下さい。 :

.. _Jinja2: http://jinja.pocoo.org/docs/templates

layout.html
-----------

.. This template contains the HTML skeleton, the header and a link to log in
   (or log out if the user was already logged in).  It also displays the
   flashed messages if there are any.  The ``{% block body %}`` block can be
   replaced by a block of the same name (``body``) in a child template.

このテンプレートは、HTMLスケルトンやヘッダーやログインページ(ログイン済みユーザーにはログアウトページ)へのリンクがあります。
フラッシュメッセージがある場合は、それも表示されます。
``{% block body %}`` ブロックは子テンプレートの同じ名前(``body``)のブロックに置き換えられます。

.. The :class:`~flask.session` dict is available in the template as well and
   you can use that to check if the user is logged in or not.  Note that in
   Jinja you can access missing attributes and items of objects / dicts which
   makes the following code work, even if there is no ``'logged_in'`` key in
   the session:

:class:`~flask.session` 辞書はテンプレートでも利用可能で、ユーザーがログインしているかどうかを確認するために使えます。
Jinja はセッションに ``'logged_in'`` キーがなくても、存在しないアトリビュートやオブジェクトか辞書の項目にアクセスすることができるので、
次のコードは動作します。 :

.. sourcecode:: html+jinja

    <!doctype html>
    <title>Flaskr</title>
    <link rel=stylesheet type=text/css href="{{ url_for('static', filename='style.css') }}">
    <div class=page>
      <h1>Flaskr</h1>
      <div class=metanav>
      {% if not session.logged_in %}
        <a href="{{ url_for('login') }}">log in</a>
      {% else %}
        <a href="{{ url_for('logout') }}">log out</a>
      {% endif %}
      </div>
      {% for message in get_flashed_messages() %}
        <div class=flash>{{ message }}</div>
      {% endfor %}
      {% block body %}{% endblock %}
    </div>

show_entries.html
-----------------

.. This template extends the :file:`layout.html` template from above to display the
   messages.  Note that the ``for`` loop iterates over the messages we passed
   in with the :func:`~flask.render_template` function.  Notice that the form is
   configured to submit to the `add_entry` view function and use ``POST`` as
   HTTP method:

このテンプレートは、メッセージを表示するために、上述した :file:`layout.html` テンプレートを拡張します。
``for`` ループは、:func:`~flask.render_template` 関数に渡されたメッセージを反復することに気をつけて下さい。
フォームが、HTTPメソッドとして ``POST`` を使って。`add_entry` ビュー関数に送信されるようになっていることに注意して下さい。 :

.. sourcecode:: html+jinja

    {% extends "layout.html" %}
    {% block body %}
      {% if session.logged_in %}
        <form action="{{ url_for('add_entry') }}" method=post class=add-entry>
          <dl>
            <dt>Title:
            <dd><input type=text size=30 name=title>
            <dt>Text:
            <dd><textarea name=text rows=5 cols=40></textarea>
            <dd><input type=submit value=Share>
          </dl>
        </form>
      {% endif %}
      <ul class=entries>
      {% for entry in entries %}
        <li><h2>{{ entry.title }}</h2>{{ entry.text|safe }}</li>
      {% else %}
        <li><em>Unbelievable.  No entries here so far</em></li>
      {% endfor %}
      </ul>
    {% endblock %}

login.html
----------

.. This is the login template, which basically just displays a form to allow
   the user to login:

これは、基本的にはユーザーがログインできるように、フォームを表示するだけのログインテンプレートです。 :

.. sourcecode:: html+jinja

    {% extends "layout.html" %}
    {% block body %}
      <h2>Login</h2>
      {% if error %}<p class=error><strong>Error:</strong> {{ error }}{% endif %}
      <form action="{{ url_for('login') }}" method=post>
        <dl>
          <dt>Username:
          <dd><input type=text name=username>
          <dt>Password:
          <dd><input type=password name=password>
          <dd><input type=submit value=Login>
        </dl>
      </form>
    {% endblock %}

.. Continue with :ref:`tutorial-css`.

続いては、:ref:`tutorial-css` 。
