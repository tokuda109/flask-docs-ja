.. _becomingbig:

.. Becoming Big
   ============

大きくなってきたら
======================

.. Your application is becoming more and more complex?  If you suddenly
   realize that Flask does things in a way that does not work out for your
   application there are ways to deal with that.

アプリケーションがもっと複雑になってきたら?
Flaskの処理にいくつか方法があって、その方法が見つけられないならこともあります。

.. Flask is powered by Werkzeug and Jinja2, two libraries that are in use at
   a number of large websites out there and all Flask does is bring those
   two together.  Being a microframework Flask does not do much more than
   combining existing libraries - there is not a lot of code involved.
   What that means for large applications is that it's very easy to take the
   code from Flask and put it into a new module within the applications and
   expand on that.

FlaskはWerkzeugとJinja2に依存していて、この2つのライブラリは少ないながらも大規模なウェブサイトで使われています。

.. Flask is designed to be extended and modified in a couple of different
   ways:

Flaskはいくつかの異なる方法で拡張したり、挙動を変えたりできるようにデザインされています。 :

.. Flask extensions.  For a lot of reusable functionality you can create
   extensions.  For extensions a number of hooks exist throughout Flask
   with signals and callback functions.

- Flaskエクステンション。
  たくさんの再利用できる関数によって、エクステンションを作ることでできます。
  エクステンション用に、Flaskのシグナルやコールバック関数を使っていくつかのフックがあります。

.. Subclassing.  The majority of functionality can be changed by creating
   a new subclass of the :class:`~flask.Flask` class and overriding
   methods provided for this exact purpose.

- サブクラス。
  関数群の大部分は :class:`~flask.Flask` クラスのサブクラスを新しく作ったり、メソッドを上書きすることで変更できます。

.. Forking.  If nothing else works out you can just take the Flask
   codebase at a given point and copy/paste it into your application
   and change it.  Flask is designed with that in mind and makes this
   incredible easy.  You just have to take the package and copy it
   into your application's code and rename it (for example to
   `framework`).  Then you can start modifying the code in there.

- フォーク。
  アプリケーションにコピーペーストして、修正して、Flaskのコードを指定する以外することはありません。
  Flaskは信じられないくらい簡単に作られていて、そういう考えでデザインされています。
  パッケージを取得して、アプリケーションのコードにコピーして、名前を変える(例として、`framework`)だけです。
  それからコードを修正すると始めることができます。

.. Why consider Forking?
   ---------------------

フォーク?
--------------

.. The majority of code of Flask is within Werkzeug and Jinja2.  These
   libraries do the majority of the work.  Flask is just the paste that glues
   those together.  For every project there is the point where the underlying
   framework gets in the way (due to assumptions the original developers
   had).  This is natural because if this would not be the case, the
   framework would be a very complex system to begin with which causes a
   steep learning curve and a lot of user frustration.

Flaskのコードの大部分は、WerkzeugとJinja2にあります。このライブラリは処理の大部分を実行しています。
Flaskはこのライブラリをくっつけて、貼りつけているだけです。
全てのプロジェクトでフレームワークの(オリジナルの作成者が持っていると仮定して)
これは普通のことで、これができないならフレームワークはとても複雑なシステムになって学習コストが上がって、たくさんのユーザーが不満に思うでしょう。

.. This is not unique to Flask.  Many people use patched and modified
   versions of their framework to counter shortcomings.  This idea is also
   reflected in the license of Flask.  You don't have to contribute any
   changes back if you decide to modify the framework.

これはFlaskに限ったことではありません。
たくさんの人はフレームワークにパッチを当てたり修正したりして使っています。
このアイデアはFlaskのライセンスにも反映されています。
フレームワークを修正しても、その修正をフィードバックする必要はありません。

.. The downside of forking is of course that Flask extensions will most
   likely break because the new framework has a different import name.
   Furthermore integrating upstream changes can be a complex process,
   depending on the number of changes.  Because of that, forking should be
   the very last resort.

フォークのマイナス面はもちろんFlaskエクステンションは新しいフレームワークは別々の
さらに上のレベルの変更を統合することはいくつか変更することになるので複雑なプロセスになります。
そういった理由から、フォークは最後の手段にしなければなりません。

.. Scaling like a Pro
   ------------------

プロのようなスケーリング
----------------------------

.. For many web applications the complexity of the code is less an issue than
   the scaling for the number of users or data entries expected.  Flask by
   itself is only limited in terms of scaling by your application code, the
   data store you want to use and the Python implementation and webserver you
   are running on.

ほとんどのウェブアプリケーションでは、コードの複雑さはユーザーの数や期待していたデータのエントリーをスケールすることよりも問題が少ない。
Flaskにおいても、アプリケーションのコードや使いたいデータストアやPythonの実装やウェブサーバーにおいてスケールするのは限界がある。

.. Scaling well means for example that if you double the amount of servers
   you get about twice the performance.  Scaling bad means that if you add a
   new server the application won't perform any better or would not even
   support a second server.

スケーリングは例えば、サーバーの台数を二倍にすると二倍のパフォーマンスを得ることができるということを意味しています。
スケーリングの悪い面は、新しいサーバーを追加するとアプリケーションのパフォーマンスが思っていたよりよくならないか、二台目のサーバーをサポートすることさえない。

.. There is only one limiting factor regarding scaling in Flask which are
   the context local proxies.  They depend on context which in Flask is
   defined as being either a thread, process or greenlet.  If your server
   uses some kind of concurrency that is not based on threads or greenlets,
   Flask will no longer be able to support these global proxies.  However the
   majority of servers are using either threads, greenlets or separate
   processes to achieve concurrency which are all methods well supported by
   the underlying Werkzeug library.

Flaskでコンテキストローカルのプロキシとしてスケールすることには一つ制限となる要因があります。
スレッドやプロセスやgreenletのどれかになるように定義されたFlaskにおいて、Flaskはコンテキストに依存します。
サーバーがスレッドやgreenlets以外で並列処理をする場合、Flaskはグローバルなプロキシをサポートすることができなくなります。
しかし、大部分のサーバーはスレッドやgreenletsやプロセスを分けて並列処理を実行するかのどれかを使うことになります。
これはWerkzeugでサポートしているメソッドで全てできます。

.. Dialogue with the Community
   ---------------------------

コミュニティとの対話
------------------------

.. The Flask developers are very interested to keep everybody happy, so as
   soon as you find an obstacle in your way, caused by Flask, don't hesitate
   to contact the developers on the mailinglist or IRC channel.  The best way
   for the Flask and Flask-extension developers to improve it for larger
   applications is getting feedback from users.

Flaskのデベロッパーは全ての人がハッピーになるようにしようとしています。
コードを書いていて、Flaskのバグを見つけたら躊躇せずに、デベロッパーかメーリングリストかIRCで教えてください。
FlaskとFlaskエクステンションのデベロッパーがユーザーからのフィードバックをもらって改良することはいい方法です。
