.. _tutorial-views:

.. Step 6: The View Functions
   ==========================

ステップ5: ビュー関数
=============================

.. Now that the database connections are working, you can start writing the
   view functions.  You will need four of them; Show Entries, Add New Entry,
   Login and Logout.  Add the following code snipets to :file:`flaskr.py`.

データベース接続ができたので、ビュー関数を実装していきましょう。
エントリの表示、エントリの新規追加、ログインとログアウトの4つが必要です。
:file:`flaskr.py` に、以下のコードスニペットを追加していきます。

.. Show Entries
   ------------

エントリの表示
------------

.. This view shows all the entries stored in the database.  It listens on the
   root of the application and will select title and text from the database.
   The one with the highest id (the newest entry) will be on top.  The rows
   returned from the cursor look a bit like dictionaries because we are using
   the :class:`sqlite3.Row` row factory.

このビューは、データベースに保持されているエントリが全て表示されます。
アプリケーションのルートを表示し、データベースからタイトルとテキストを選択します。
IDが一番高い(最も新しいエントリ)が上に表示されます。
カーソルから返された行は、:class:`sqlite3.Row` の行ファクトリーを使っているので、辞書のように扱えます。

.. The view function will pass the entries to the :file:`show_entries.html`
   template and return the rendered one::

ビュー関数は、:file:`show_entries.html` テンプレートにエントリを渡し、レンダリングされたものを返します。 ::

    @app.route('/')
    def show_entries():
        db = get_db()
        cur = db.execute('select title, text from entries order by id desc')
        entries = cur.fetchall()
        return render_template('show_entries.html', entries=entries)

.. Add New Entry
   -------------

エントリの新規追加
-------------

.. This view lets the user add new entries if they are logged in.  This only
   responds to ``POST`` requests; the actual form is shown on the
   `show_entries` page.  If everything worked out well, it will
   :func:`~flask.flash` an information message to the next request and
   redirect back to the `show_entries` page::

このビューは、ユーザーがログインしている場合は新しいエントリを追加することができます。
``POST`` でのみ受け付けて、`show_entries` ページに表示されます。
全てが上手くいけば、:func:`~flask.flash` は次のリクエストにフラッシュメッセージを渡し、
`show_entries` ページにリダイレクトします。 ::

    @app.route('/add', methods=['POST'])
    def add_entry():
        if not session.get('logged_in'):
            abort(401)
        db = get_db()
        db.execute('insert into entries (title, text) values (?, ?)',
                     [request.form['title'], request.form['text']])
        db.commit()
        flash('New entry was successfully posted')
        return redirect(url_for('show_entries'))

.. Note that this view checks that the user is logged in (that is, if the
   `logged_in` key is present in the session and ``True``).

このビューは、ユーザーがログインしている(`logged_in` キーがセッションに存在し、``True`` である場合)か確認することに注意して下さい。

.. Security Note

   Be sure to use question marks when building SQL statements, as done in the
   example above.  Otherwise, your app will be vulnerable to SQL injection when
   you use string formatting to build SQL statements.
   See :ref:`sqlite3` for more.

.. admonition:: セキュリティに関する注意点

   上記の例のように、SQL文を組み立てる場合は、疑問符を使うようにしてください。
   そうしない場合、文字列のフォーマットをして、SQL文を組み立てる時にSQLインジェクションに対して脆弱になります。
   詳しくは、:ref:`sqlite3` を見て下さい。

.. Login and Logout
   ----------------

ログインとログアウト
----------------

.. These functions are used to sign the user in and out.  Login checks the
   username and password against the ones from the configuration and sets the
   `logged_in` key for the session.  If the user logged in successfully, that
   key is set to ``True``, and the user is redirected back to the `show_entries`
   page.  In addition, a message is flashed that informs the user that he or
   she was logged in successfully.  If an error occurred, the template is
   notified about that, and the user is asked again::

これらの関数は、ユーザーのサインインとサインアウトの時に使われます。
ログインは、設定からユーザー名とパスワードをチェックし、セッションに `logged_in` キーをセットします。
ユーザーのログインが成功すると、このキーには ``True`` がセットされ、ユーザーは `show_entries` ページにリダイレクトされます。
さらに、ログインが成功したことをフラッシュメッセージで知らせます。
エラーが発生した場合、テンプレートにその内容が通知され、ユーザーに再度ログインを促します。

    @app.route('/login', methods=['GET', 'POST'])
    def login():
        error = None
        if request.method == 'POST':
            if request.form['username'] != app.config['USERNAME']:
                error = 'Invalid username'
            elif request.form['password'] != app.config['PASSWORD']:
                error = 'Invalid password'
            else:
                session['logged_in'] = True
                flash('You were logged in')
                return redirect(url_for('show_entries'))
        return render_template('login.html', error=error)

.. The `logout` function, on the other hand, removes that key from the session
   again.  There is a neat trick here: if you use the :meth:`~dict.pop` method
   of the dict and pass a second parameter to it (the default), the method
   will delete the key from the dictionary if present or do nothing when that
   key is not in there.  This is helpful because now it is not necessary to
   check if the user was logged in.

一方で、ログアウト関数はセッションからキーを削除します。
いいやり方があって、辞書の :meth:`~dict.pop` メソッドの2番目のパラメータに None (デフォルト)を渡すと
辞書にキーがある場合は辞書からキーを削除し、ない場合は何もしません。
ユーザーがログインしたかどうかを確認する必要がないため、このやり方は簡単です。

::

    @app.route('/logout')
    def logout():
        session.pop('logged_in', None)
        flash('You were logged out')
        return redirect(url_for('show_entries'))

.. Security Note

    Passwords should never be stored in plain text in a production
    system. This tutorial uses plain text passwords for simplicity. If you
    plan to release a project based off this tutorial out into the world,
    passwords should be both `hashed and salted`_ before being stored in a
    database or file.

    Fortunately, there are Flask extensions for the purpose of
    hashing passwords and verifying passwords against hashes, so adding
    this functionality is fairly straight forward. There are also
    many general python libraries that can be used for hashing.

    You can find a list of recommended Flask extensions
    `here <http://flask.pocoo.org/extensions/>`_

.. admonition:: Security Note

   パスワードはプロダクションのシステムではプレーンテキストとして保持しないで下さい。
   このチュートリアルでは、簡単にするためにプレーンテキストのパスワードを使用しています。
   このチュートリアルをベースとしたプロジェクトをリリースする予定なら、
   パスワードはデータベースやファイルに保持される前に `hashed and salted`_ されていなければいけません。

   幸い、Flask のエクステンションには、パスワードをハッシュ化し、ハッシュ化されたパスワードを検証する機能があって、
   この機能を追加するのはかなり簡単です。
   ハッシュ化するのに使える Python の一般的なライブラリもたくさんあります。

   おすすめの Flask エクステンションの一覧を `こちら <http://flask.pocoo.org/extensions/>`_ から見つけることができます。

.. Continue with :ref:`tutorial-templates`.

続いては、:ref:`tutorial-templates` 。

.. _hashed and salted: https://blog.codinghorror.com/youre-probably-storing-passwords-incorrectly/
