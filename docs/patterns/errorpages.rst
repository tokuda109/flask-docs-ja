.. Custom Error Pages
   ==================

カスタムエラーページ
========================

.. Flask comes with a handy :func:`~flask.abort` function that aborts a
   request with an HTTP error code early.  It will also provide a plain black
   and white error page for you with a basic description, but nothing fancy.

Flaskは、簡単にHTTPエラーコードでリクエストを中止することができる :func:`~flask.abort`
関数があります。基本的な概要のみの単純な黒と白のエラーページを表示させることもできます。

.. Depending on the error code it is less or more likely for the user to
   actually see such an error.

エラーコードによって、そのようなエラーをユーザーに実際に表示するには

.. Common Error Codes
   ------------------

一般的なエラーコード
--------------------------

.. The following error codes are some that are often displayed to the user,
   even if the application behaves correctly:

以下のようなエラーコードは、まるでアプリケーションが正しく振舞ったかのようにユーザーに表示されます。

*404 Not Found*
    The good old "chap, you made a mistake typing that URL" message.  So
    common that even novices to the internet know that 404 means: damn,
    the thing I was looking for is not there.  It's a very good idea to
    make sure there is actually something useful on a 404 page, at least a
    link back to the index.

*403 Forbidden*
    If you have some kind of access control on your website, you will have
    to send a 403 code for disallowed resources.  So make sure the user
    is not lost when they try to access a forbidden resource.

*410 Gone*
    Did you know that there the "404 Not Found" has a brother named "410
    Gone"?  Few people actually implement that, but the idea is that
    resources that previously existed and got deleted answer with 410
    instead of 404.  If you are not deleting documents permanently from
    the database but just mark them as deleted, do the user a favour and
    use the 410 code instead and display a message that what they were
    looking for was deleted for all eternity.

*500 Internal Server Error*
    Usually happens on programming errors or if the server is overloaded.
    A terrible good idea to have a nice page there, because your
    application *will* fail sooner or later (see also:
    :ref:`application-errors`).


.. Error Handlers
   --------------

エラーハンドラー
---------------------

.. An error handler is a function, just like a view function, but it is
   called when an error happens and is passed that error.  The error is most
   likely a :exc:`~werkzeug.exceptions.HTTPException`, but in one case it
   can be a different error: a handler for internal server errors will be
   passed other exception instances as well if they are uncaught.

エラーハンドラーはビュー関数みたいな関数ですが、エラーが発生した時呼ばれて、エラーが渡されます。
エラーはほとんどが :exc:`~werkzeug.exceptions.HTTPException` ですが、
内部のサーバーエラーの処理などのケースでは別のエラーになります。

An error handler is registered with the :meth:`~flask.Flask.errorhandler`
decorator and the error code of the exception.  Keep in mind that Flask
will *not* set the error code for you, so make sure to also provide the
HTTP status code when returning a response.

.. Here an example implementation for a "404 Page Not Found" exception::

"404 Page Not Found" の例外の実装の例を示します。 ::

    from flask import render_template

    @app.errorhandler(404)
    def page_not_found(e):
        return render_template('404.html'), 404

.. An example template might be this:

テンプレートの例は以下のようになると思います。 :

.. sourcecode:: html+jinja

   {% extends "layout.html" %}
   {% block title %}Page Not Found{% endblock %}
   {% block body %}
     <h1>Page Not Found</h1>
     <p>What you were looking for is just not there.
     <p><a href="{{ url_for('index') }}">go somewhere nice</a>
   {% endblock %}
