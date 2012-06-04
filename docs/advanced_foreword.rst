.. _advanced_foreword:

経験豊富なプログラマのための前書き
=======================================

.. Foreword for Experienced Programmers
   ====================================

.. Thread-Locals in Flask
   ----------------------

Flaskのスレッドローカル
------------------------------

.. One of the design decisions in Flask was that simple tasks should be simple;
   they should not take a lot of code and yet they should not limit you. Because
   of that, Flask has few design choices that some people might find surprising or
   unorthodox. For example, Flask uses thread-local objects internally so that you
   don’t have to pass objects around from function to function within a request in
   order to stay threadsafe. This approach is convenient, but requires a valid
   request context for dependency injection or when attempting to reuse code which
   uses a value pegged to the request.  The Flask project is honest about
   thread-locals, does not hide them, and calls out in the code and documentation
   where they are used.

Flaskにおけるデザイン上の決まりごとの一つは簡単な処理はシンプルにすべきです。
たくさんのコードを書かないようにするべきで、また、書くことを制限しないようにするべきです。
その理由から、私たちが驚いたり、正統ではないと思ったりするかもしれない設計上の選択をいくつかしています。
例えば、Flaskは内部でスレッドローカルのオブジェクトを使っていて、スレッドセーフにするためにリクエスト内で関数から関数を呼び出して
この方法は便利ですが、
これらの問題を解決するためにスレッドローカルを隠しません。

.. Develop for the Web with Caution
   --------------------------------

慎重なウェブ開発
------------------------

.. Always keep security in mind when building web applications.

Webアプリケーションを構築する際に、セキュリティを常に念頭に置いておかなければいけません。

.. If you write a web application, you are probably allowing users to register
   and leave their data on your server.  The users are entrusting you with data.
   And even if you are the only user that might leave data in your application,
   you still want that data to be stored securely.

ウェブアプリケーションを書いているなら、ユーザーがユーザー登録やデータの削除をあなたのアプリケーションのサーバー上で行うことになります。
ユーザーはあなたにデータを預けています。そして、あなたがアプリケーションにデータが残す唯一のユーザーだとしても、データを安全に保管したいかもしれません。

.. Unfortunately, there are many ways the security of a web application can be
   compromised.  Flask protects you against one of the most common security
   problems of modern web applications: cross-site scripting (XSS).  Unless you
   deliberately mark insecure HTML as secure, Flask and the underlying Jinja2
   template engine have you covered.  But there are many more ways to cause
   security problems.

不幸にも、ウェブアプリケーションで保証できるセキュリティにはたくさんの方法があります。
Flaskはモダンなウェブアプリケーションの一般的なほとんどのセキュリティの問題(クロスサイトスクリプティング(XSS)等)を防ぎます。
故意にセキュアではないHTMLを記述しない限り、FlaskやそのベースのJinja2テンプレートエンジンはセキュアに保護しようとします。
しかし、セキュリティの問題となる原因は他にもたくさんあります。

.. The documentation will warn you about aspects of web development that require
   attention to security.  Some of these security concerns are far more complex
   than one might think, and we all sometimes underestimate the likelihood that a
   vulnerability will be exploited - until a clever attacker figures out a way to
   exploit our applications.  And don't think that your application is not
   important enough to attract an attacker.  Depending on the kind of attack,
   chances are that automated bots are probing for ways to fill your database with
   spam, links to malicious software, and the like.

ドキュメントはウェブアプリケーションをセキュアにするための注意を警告するでしょう。
これらのセキュリティは考えられる以上に複雑なものが関係してきます。
そして、私たちは賢いアタッカーがアプリケーションを悪用する方法を理解していて、脆弱性を突かれる可能性があることを時々過小評価します。
攻撃の種類に応じて、自動ボットがスパムや悪意のあるソフトウェアへのリンク等でデータベースに入れる方法を調査します。

.. Flask is no different from any other framework in that you the developer must
   build with caution, watching for exploits when building to your requirements.

開発者は要件に応じて構築する時に不正を監視して、慎重に構築しなければいけないという点で、
Flaskは他のフレームワークと違いはありません。

.. The Status of Python 3
   ----------------------

Python3の状況
----------------------

.. Currently the Python community is in the process of improving libraries to
   support the new iteration of the Python programming language.  While the
   situation is greatly improving there are still some issues that make it
   hard for us to switch over to Python 3 just now.  These problems are
   partially caused by changes in the language that went unreviewed for too
   long, partially also because we have not quite worked out how the lower-
   level API should change to account for the Unicode differences in Python 3.

現在、PythonコミュニティはPythonプログラミング言語の新しい開発サイクルに入るためにライブラリを改良しているところです。
とは言え、状況はだいぶ良くなって来ましたが、まだいくつかの問題があり、それが現在Python3に移行するのを難しくしています。
これらの問題は長い間レビューされていない言語の変更によって引き起こされたり、
低レベルのAPIがどのようにPython3のユニコードを違うものに変更するべきかわからないかもしれません。

.. Werkzeug and Flask will be ported to Python 3 as soon as a solution for
   the changes is found, and we will provide helpful tips how to upgrade
   existing applications to Python 3.  Until then, we strongly recommend
   using Python 2.6 and 2.7 with activated Python 3 warnings during
   development.  If you plan on upgrading to Python 3 in the near future we
   strongly recommend that you read `How to write forwards compatible
   Python code <http://lucumr.pocoo.org/2011/1/22/forwards-compatible-python/>`_.

WerkzeugとFlaskは、Python3用に変更する方法を検討後、すぐにPython3に移植されるでしょう。
そして、既存のアプリケーションをPython 3にアップグレードする方法のヒントを提供しています。
その時までは、Python 3で開発中に警告が出るなら、Python 2.6と2.7を使うことを強くお勧めしています。
近々Python3にアップグレードする計画がある場合、
`How to write forwards compatible Python code <http://lucumr.pocoo.org/2011/1/22/forwards-compatible-python/>`_
を読むことをお勧めします。

.. Continue to :ref:`installation` or the :ref:`quickstart`.

続いては、 :ref:`installation` 、 :ref:`quickstart` です。
