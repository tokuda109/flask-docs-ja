.. _application-errors:

アプリケーションのエラー処理
==============================

.. Logging Application Errors
   ==========================

.. versionadded:: 0.3

.. Applications fail, servers fail.  Sooner or later you will see an exception
   in production.  Even if your code is 100% correct, you will still see
   exceptions from time to time.  Why?  Because everything else involved will
   fail.  Here some situations where perfectly fine code can lead to server
   errors:

アプリケーションやサーバーにはエラーが起きます。早かれ遅かれ本番環境でエラー見ることになるでしょう。
たとえ、コードが100%正しくても、エラーを見ることになります。なぜでしょう？
なぜならすべてのことに失敗が入り込んでくるからです。
ここでは、本当によく書かれているコードがサーバーエラーを起こすいくつかの状況示します。 :

.. the client terminated the request early and the application was still
   reading from the incoming data.
.. the database server was overloaded and could not handle the query.
.. a filesystem is full
.. a harddrive crashed
.. a backend server overloaded
.. a programming error in a library you are using
.. network connection of the server to another system failed.

- クライアントが早くリクエストを終了し、アプリケーションがまだ読み込みを行なっている。
- データベースサーバーが過負荷になって、クエリを処理できなくなっている。
- ファイルシステムが一杯になっている。
- ハードドライブがクラッシュしている。
- バックエンドサーバーが過負荷になっている。
- 使っているライブラリのエラー。
- 他のサーバーへのネットワーク接続の失敗。

.. And that's just a small sample of issues you could be facing.  So how do we
   deal with that sort of problem?  By default if your application runs in
   production mode, Flask will display a very simple page for you and log the
   exception to the :attr:`~flask.Flask.logger`.

そして、これらは直面するかもしれない小さな問題の例です。
そのような問題をどのように対処すればいいのでしょうか?
標準では、アプリケーションがプロダクションモードで起動されている場合に、Flaskはとてもシンプルなページを表示して、
:attr:`~flask.Flask.logger` に対して例外のログを残します。

.. But there is more you can do, and we will cover some better setups to deal
   with errors.

しかし、他にもできることがあります。そして、エラーを扱うためのより良いセットアップをカバーするでしょう。

.. Error Mails
   -----------

エラーメール
--------------

.. If the application runs in production mode (which it will do on your
   server) you won't see any log messages by default.  Why is that?  Flask
   tries to be a zero-configuration framework.  Where should it drop the logs
   for you if there is no configuration?  Guessing is not a good idea because
   chances are, the place it guessed is not the place where the user has
   permission to create a logfile.  Also, for most small applications nobody
   will look at the logs anyways.

アプリケーションが(あなたのサーバー上で)プロダクションモードで実行されているなら、デフォルトでログメッセージを見ることはないでしょう。
それはなぜでしょう?Flaskは設定がないフレームワークになるようにしているからです。
何も設定していない場合、どこにログを残すべきですか?
可能性があるので推測は良いアイデアではない。推測された場所はユーザーがログファイルを作成する権限持っている場所ではありません。
また、ほとんどの小規模のアプリケーションのために誰もログをとにかく見ている人はいないでしょう。

.. In fact, I promise you right now that if you configure a logfile for the
   application errors you will never look at it except for debugging an issue
   when a user reported it for you.  What you want instead is a mail the
   second the exception happened.  Then you get an alert and you can do
   something about it.

実際には、アプリケーションのエラーに対してログファイルの設定をしている場合、
ユーザーが問題のデバッグの例外を見ることはないでしょう。
その時に警告を発して、それに対して何かすることできます。

.. Flask uses the Python builtin logging system, and it can actually send
   you mails for errors which is probably what you want.  Here is how you can
   configure the Flask logger to send you mails for exceptions::

FlaskはPythonの組み込みのロギングシステムを使っていて、欲しいエラー情報を実際にメールで送ってくれます。
ここでは、例外のメールを送るためのFlaskのロガーの設定の仕方を示します。 ::

    ADMINS = ['yourname@example.com']
    if not app.debug:
        import logging
        from logging.handlers import SMTPHandler
        mail_handler = SMTPHandler('127.0.0.1',
                                   'server-error@example.com',
                                   ADMINS, 'YourApplication Failed')
        mail_handler.setLevel(logging.ERROR)
        app.logger.addHandler(mail_handler)

.. So what just happened?  We created a new
   :class:`~logging.handlers.SMTPHandler` that will send mails with the mail
   server listening on ``127.0.0.1`` to all the `ADMINS` from the address
   *server-error@example.com* with the subject "YourApplication Failed".  If
   your mail server requires credentials, these can also be provided.  For
   that check out the documentation for the
   :class:`~logging.handlers.SMTPHandler`.

何が起こったのでしょうか?
新しい :class:`~logging.handlers.SMTPHandler` 作成します。これは、 "YourApplication Failed" という
タイトルで *server-error@example.com* から全ての `ADMINS` にメールを送ります。
メールサーバーは認証が必要な場合、これも実装することができます。
:class:`~logging.handlers.SMTPHandler` のドキュメントをチェックして下さい。

.. We also tell the handler to only send errors and more critical messages.
   Because we certainly don't want to get a mail for warnings or other
   useless logs that might happen during request handling.

エラーやよりクリティカルなメッセージだけメールを送る処理を指示します。
なぜなら、リクエストを処理している間に発生するだろう警告や、他のログを取るほどでもないもののメールを受けとりたくないからです。

.. Before you run that in production, please also look at :ref:`logformat` to
   put more information into that error mail.  That will save you from a lot
   of frustration.

本番環境で実行する前に、エラーメールに情報を書き込むための :ref:`logformat` も見てください。
たくさんのフラストレーションから救ってくれるでしょう。


.. Logging to a File
   -----------------

ファイルにログを書き込む
-------------------------

.. Even if you get mails, you probably also want to log warnings.  It's a
   good idea to keep as much information around that might be required to
   debug a problem.  Please note that Flask itself will not issue any
   warnings in the core system, so it's your responsibility to warn in the
   code if something seems odd.

たとえメールを取得した場合でも、警告も記録したいかもしれません。
問題をデバッグするために必要とされるかもしれないたくさんの情報をキープしておくことは良いアイデアだと思います。
Flask自体はコアシステムに任意の警告を出さないので、奇妙かもしれませんが、コード内で警告を出す義務があります。

.. There are a couple of handlers provided by the logging system out of the
   box but not all of them are useful for basic error logging.  The most
   interesting are probably the following:

ロギングシステムで提供されているハンドラーは複数あります。
しかし、それらの全てが基本的なエラーログに対して使いやすい訳ではない。
最も興味深いのは、おそらく以下のものだと思います。 :

.. :class:`~logging.FileHandler` - logs messages to a file on the
   filesystem.
.. :class:`~logging.handlers.RotatingFileHandler` - logs messages to a file
   on the filesystem and will rotate after a certain number of messages.
.. :class:`~logging.handlers.NTEventLogHandler` - will log to the system
   event log of a Windows system.  If you are deploying on a Windows box,
   this is what you want to use.
.. :class:`~logging.handlers.SysLogHandler` - sends logs to a UNIX
   syslog.

- :class:`~logging.FileHandler` - ファイルシステムのファイルにメッセージを記録する。
- :class:`~logging.handlers.RotatingFileHandler` - ファイルシステムのファイルにメッセージを記録して、
  メッセージの数がある数に達すると循環される。
- :class:`~logging.handlers.NTEventLogHandler` - Windowsシステムのシステムのイベントログを記録します。
  Windowsに依存している場合には、これは使いたいものでしょう。
- :class:`~logging.handlers.SysLogHandler` - UNIXのsyslogにログを送ります。

.. Once you picked your log handler, do like you did with the SMTP handler
   above, just make sure to use a lower setting (I would recommend
   `WARNING`)::

ログハンドラーを選択すると、上記のSMTPハンドラーでしたように、より低い設定を使っているか確認するだけです(`WARNING` をお勧めします)。 ::

    if not app.debug:
        import logging
        from themodule import TheHandlerYouWant
        file_handler = TheHandlerYouWant(...)
        file_handler.setLevel(logging.WARNING)
        app.logger.addHandler(file_handler)

.. _logformat:

ログのフォーマットの管理
-----------------------------

.. Controlling the Log Format
   --------------------------

.. By default a handler will only write the message string into a file or
   send you that message as mail.  A log record stores more information,
   and it makes a lot of sense to configure your logger to also contain that
   information so that you have a better idea of why that error happened, and
   more importantly, where it did.

デフォルトで、ハンドラーはファイルにメッセージの文字列を書きこむかメールでメッセージを送るでしょう。
ログはさらなる情報を記録して、エラーがなぜ起こったかということや、より詳細な情報や、どこで起こったかということの確かな情報を
得ることができるようにロガーに設定することはいいことです。

.. A formatter can be instantiated with a format string.  Note that
   tracebacks are appended to the log entry automatically.  You don't have to
   do that in the log formatter format string.

フォーマッターはフォーマット文字列で初期化します。
トレースバック自動的にログを取ることを促します。
フォーマッターのフォーマット文字列に何もすることはありません。

.. Here some example setups:

ここでは、幾つかのセットアップ事例を紹介します。 :

.. Email
   `````

Eメール
`````````

::

    from logging import Formatter
    mail_handler.setFormatter(Formatter('''
    Message type:       %(levelname)s
    Location:           %(pathname)s:%(lineno)d
    Module:             %(module)s
    Function:           %(funcName)s
    Time:               %(asctime)s

    Message:

    %(message)s
    '''))

.. File logging
   ````````````

ファイルロギング
`````````````````````

::

    from logging import Formatter
    file_handler.setFormatter(Formatter(
        '%(asctime)s %(levelname)s: %(message)s '
        '[in %(pathname)s:%(lineno)d]'
    ))


.. Complex Log Formatting
   ``````````````````````

複雑なログのフォーマット
````````````````````````````

.. Here is a list of useful formatting variables for the format string.  Note
   that this list is not complete, consult the official documentation of the
   :mod:`logging` package for a full list.

ここでは、フォーマット文字列のための便利な値をフォーマットのリストです。
このリストは完璧ではないので注意して下さい。全てのリストを見るには、 :mod:`logging` パッケージの公式ドキュメントを調べて下さい。

.. tabularcolumns:: |p{3cm}|p{12cm}|

+------------------+----------------------------------------------------+
| Format           | Description                                        |
+==================+====================================================+
| ``%(levelname)s``| メッセージを残すログレベルのテキスト       　|
|                  | (``'DEBUG'``, ``'INFO'``, ``'WARNING'``,           |
|                  | ``'ERROR'``, ``'CRITICAL'``).                      |
+------------------+----------------------------------------------------+
| ``%(pathname)s`` | Full pathname of the source file where the         |
|                  | logging call was issued (if available).            |
+------------------+----------------------------------------------------+
| ``%(filename)s`` | Filename portion of pathname.                      |
+------------------+----------------------------------------------------+
| ``%(module)s``   | Module (name portion of filename).                 |
+------------------+----------------------------------------------------+
| ``%(funcName)s`` | Name of function containing the logging call.      |
+------------------+----------------------------------------------------+
| ``%(lineno)d``   | Source line number where the logging call was      |
|                  | issued (if available).                             |
+------------------+----------------------------------------------------+
| ``%(asctime)s``  | Human-readable time when the LogRecord` was        |
|                  | created.  By default this is of the form           |
|                  | ``"2003-07-08 16:49:45,896"`` (the numbers after   |
|                  | the comma are millisecond portion of the time).    |
|                  | This can be changed by subclassing the formatter   |
|                  | and overriding the                                 |
|                  | :meth:`~logging.Formatter.formatTime` method.      |
+------------------+----------------------------------------------------+
| ``%(message)s``  | The logged message, computed as ``msg % args``     |
+------------------+----------------------------------------------------+

.. +------------------+----------------------------------------------------+
   | Format           | Description                                        |
   +==================+====================================================+
   | ``%(levelname)s``| Text logging level for the message                 |
   |                  | (``'DEBUG'``, ``'INFO'``, ``'WARNING'``,           |
   |                  | ``'ERROR'``, ``'CRITICAL'``).                      |
   +------------------+----------------------------------------------------+
   | ``%(pathname)s`` | Full pathname of the source file where the         |
   |                  | logging call was issued (if available).            |
   +------------------+----------------------------------------------------+
   | ``%(filename)s`` | Filename portion of pathname.                      |
   +------------------+----------------------------------------------------+
   | ``%(module)s``   | Module (name portion of filename).                 |
   +------------------+----------------------------------------------------+
   | ``%(funcName)s`` | Name of function containing the logging call.      |
   +------------------+----------------------------------------------------+
   | ``%(lineno)d``   | Source line number where the logging call was      |
   |                  | issued (if available).                             |
   +------------------+----------------------------------------------------+
   | ``%(asctime)s``  | Human-readable time when the LogRecord` was        |
   |                  | created.  By default this is of the form           |
   |                  | ``"2003-07-08 16:49:45,896"`` (the numbers after   |
   |                  | the comma are millisecond portion of the time).    |
   |                  | This can be changed by subclassing the formatter   |
   |                  | and overriding the                                 |
   |                  | :meth:`~logging.Formatter.formatTime` method.      |
   +------------------+----------------------------------------------------+
   | ``%(message)s``  | The logged message, computed as ``msg % args``     |
   +------------------+----------------------------------------------------+

.. If you want to further customize the formatting, you can subclass the
   formatter.  The formatter has three interesting methods:

フォーマットをさらにカスタマイズしたい場合は、フォーマットのサブクラスを作ることができます。
フォーマットのクラスは三つのメソッドがあります。:

.. :meth:`~logging.Formatter.format`:
       handles the actual formatting.  It is passed a
       :class:`~logging.LogRecord` object and has to return the formatted
       string.
.. :meth:`~logging.Formatter.formatTime`:
       called for `asctime` formatting.  If you want a different time format
       you can override this method.
.. :meth:`~logging.Formatter.formatException`
       called for exception formatting.  It is passed an :attr:`~sys.exc_info`
       tuple and has to return a string.  The default is usually fine, you
       don't have to override it.

:meth:`~logging.Formatter.format`:
    実際のフォーマットで処理します。
    :class:`~logging.LogRecord` オブジェクトに渡してフォーマットにする文字列を返す必要があります。
:meth:`~logging.Formatter.formatTime`:
    called for `asctime` formatting.  If you want a different time format
    you can override this method.
:meth:`~logging.Formatter.formatException`
    called for exception formatting.  It is passed an :attr:`~sys.exc_info`
    tuple and has to return a string.  The default is usually fine, you
    don't have to override it.

.. For more information, head over to the official documentation.

詳細については公式のドキュメントを見てください。


.. Other Libraries
   ---------------

他のライブラリ
------------------

.. So far we only configured the logger your application created itself.
   Other libraries might log themselves as well.  For example, SQLAlchemy uses
   logging heavily in its core.  While there is a method to configure all
   loggers at once in the :mod:`logging` package, I would not recommend using
   it.  There might be a situation in which you want to have multiple
   separate applications running side by side in the same Python interpreter
   and then it becomes impossible to have different logging setups for those.

ここまでは、アプリケーションで作成したロガーのみ設定してきました。
他のライブラリも同じようなログを残すことができます。例えば、SQLAlchemyは内部でよくログを取ります。
:mod:`logging` パッケージを一度全てのロガーに設定するためのメソッドがあります。
複数の分割したアプリケーションでそれぞれ同じPythonインタープリターで起動したい場合もあるかもしれません。
以下のように異なるロギングをセットアップすることは不可能です。

.. Instead, I would recommend figuring out which loggers you are interested
   in, getting the loggers with the :func:`~logging.getLogger` function and
   iterating over them to attach handlers::

代わりに、 :func:`~logging.getLogger` 関数でロガーを作成して、ハンドラーにそれぞれ追加して、
使いたいロガーを使うことをお勧めします。 ::

    from logging import getLogger
    loggers = [app.logger, getLogger('sqlalchemy'),
               getLogger('otherlibrary')]
    for logger in loggers:
        logger.addHandler(mail_handler)
        logger.addHandler(file_handler)


.. Debugging Application Errors
   ============================

アプリケーションエラーのデバッグ
========================================

.. For production applications, configure your application with logging and
   notifications as described in :ref:`application-errors`.  This section provides
   pointers when debugging deployment configuration and digging deeper with a
   full-featured Python debugger.

本番環境のアプリケーション用に、 :ref:`application-errors`　に書かれているとおり、
ロギングと通知をアプリケーションに設定します。
この章では、デプロイ済みの設定をデバッグして、フル機能のPythonデバッガーで深く掘り下げていく時の手引きとなります。

.. When in Doubt, Run Manually
   ---------------------------

疑わしい場合は、手動で実行して下さい
-----------------------------------

.. Having problems getting your application configured for production?  If you
   have shell access to your host, verify that you can run your application
   manually from the shell in the deployment environment.  Be sure to run under
   the same user account as the configured deployment to troubleshoot permission
   issues.  You can use Flask's builtin development server with `debug=True` on
   your production host, which is helpful in catching configuration issues, but
   **be sure to do this temporarily in a controlled environment.** Do not run in
   production with `debug=True`.

本番環境用にアプリケーションを設定する際に問題がありましたか?
ホストにシェルアクセスできる場合、デプロイ環境にシェルから手動でアプリケーションを実行することができます。

.. _working-with-debuggers:

デバッガーと同時に動かす
-------------------------

.. Working with Debuggers
   ----------------------

.. To dig deeper, possibly to trace code execution, Flask provides a debugger out
   of the box (see :ref:`debug-mode`).  If you would like to use another Python
   debugger, note that debuggers interfere with each other.  You have to set some
   options in order to use your favorite debugger:

より深く掘り下げるためには、コードの実行をトレースすることができ、
そのために、Flaskはデバッガーを提供しています(:ref:`debug-mode` を見て下さい)。
他のPythonデバッガーを使いたい場合、それぞれのデバッガーのインターフェースに注意して下さい。
好きなデバッガーを使うための設定をしなければいけません。

.. ``debug``        - whether to enable debug mode and catch exceptinos
.. ``use_debugger`` - whether to use the internal Flask debugger
.. ``use_reloader`` - whether to reload and fork the process on exception

* ``debug``        - デバッグモードを有効にして例外を捕まえるかどうか
* ``use_debugger`` - 内部のFlaskデバッガーを使うかどうか
* ``use_reloader`` - 例外のプロセスをフォークとリロードするかどうか

.. ``debug`` must be True (i.e., exceptions must be caught) in order for the other
   two options to have any value.

他のオプションは任意の値でも構いませんが、 ``debug`` はTrueにして下さい(例外を捕まえることができるように)。

.. If you're using Aptana/Eclipse for debugging you'll need to set both
   ``use_debugger`` and ``use_reloader`` to False.

デバッグにAptana/Eclipseを使っている場合、
``use_debugger`` と ``use_reloader`` をFalseに設定して下さい。

.. A possible useful pattern for configuration is to set the following in your
   config.yaml (change the block as appropriate for your application, of course)::

コンフィグに設定可能なパターンとして、config.yamlに以下のように設定することができます。
(もちろん、アプリケーションに応じてブロックを変更することができます) ::

   FLASK:
       DEBUG: True
       DEBUG_WITH_APTANA: True

.. Then in your application's entry-point (main.py), you could have something like::

それから、アプリケーションのエントリポイント(main.py)には、次のようなものを持つことができます。 ::

   if __name__ == "__main__":
       # To allow aptana to receive errors, set use_debugger=False
       app = create_app(config="config.yaml")

       if app.debug: use_debugger = True
       try:
           # Disable Flask's debugger if external debugger is requested
           use_debugger = not(app.config.get('DEBUG_WITH_APTANA'))
       except:
           pass
       app.run(use_debugger=use_debugger, debug=app.debug,
               use_reloader=use_debugger, host='0.0.0.0')
