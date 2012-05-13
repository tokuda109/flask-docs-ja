.. Foreword
   ========

進めるにあたって
=================

.. Read this before you get started with Flask.  This hopefully answers some
   questions about the purpose and goals of the project, and when you
   should or should not be using it.

Flaskを使いはじめる前にこの章を読んでください。
このドキュメントは、プロジェクトの目的とゴールについて、あなたが何をすべきで何をすべきでないかの答えとなるでしょう。

.. What does "micro" mean?
   -----------------------

“micro”の意味するところ
--------------------------

.. “Micro” does not mean that your whole web application has to fit into a single
   Python file, although it certainly can. Nor does it mean that Flask is lacking
   in functionality. The "micro" in microframework means Flask aims to keep the
   core simple but extensible. Flask won't make many decisions for you, such as
   what database to use. Those decisions that it does make, such as what
   templating engine to use, are easy to change.  Everything else is up to you, so
   that Flask can be everything you need and nothing you don't.

“Micro” は全てのウェブアプリケーションを一つのPythonファイルにまとめなければいけないということではありません。

マイクロフレームワークの “micro” は、Flaskのコアをシンプルにしつつ、拡張性があるということです。
Flaskは、どのようなデータベースを使うかというような、選択を何回もさせないようにしています。



しかしながら、私達にとって “micro” の意味は全ての実装を一つのPythonファイルに収めることではありません。
私にとって、microframeworkの"micro"は、フレームワークの簡易性やフレームワークの小ささを意味するものではなく、
何回も意思決定をしなければならないようにさせないこと意味しています。
Flaskはテンプレートエンジンを選択したことによって、あるデータストアや他の部分で制限されてしまうことがないようにしています。

.. By default, Flask does not include a database abstraction layer, form
   validation or anything else where different libraries already exist that can
   handle that. Instead, Flask supports extensions to add such functionality to
   your application as if it was implemented in Flask itself. Numerous extensions
   provide database integration, form validation, upload handling, various open
   authentication technologies, and more. Flask may be "micro", but it's ready for
   production use on a variety of needs.

デフォルトで、Flaskはデータベースのレイヤーやバリデーションやがありません。

.. Configuration and Conventions
   -----------------------------

設定より規約
-------------------

.. Flask has many configuration values, with sensible defaults, and a few
   conventions when getting started.  By convention templates and static files are
   stored in subdirectories within the application's Python source tree, with the
   names `templates` and `static` respectively. While this can be changed you
   usually don't have to, especially when getting started.

Flaskには設定値がたくさんあり、
規約によって、テンプレートや静的ファイルは、それぞれ `templates` や `static` という名前で、
アプリケーションのPythonのソースツリー内のサブディレクトリ内に保管されます。
これは、最初に実行するときは変更することは特にありませんが、変更することができます。

Growing with Flask
------------------

.. Once you have Flask up and running, you'll find a variety of extensions
   available in the community to integrate your project for production. The Flask
   core team reviews extensions and ensures approved extensions do not break with
   future releases.

一度Flaskを起動したら、コミュニティにある様々なエクステンションを探す
Flaskのコアチームは

.. As your codebase grows, you are free to make the design decisions appropriate
   for your project.  Flask will continue to provide a very simple glue layer to
   the best that Python has to offer.  You can implement advanced patterns in
   SQLAlchemy or another database tool, introduce non-relational data persistence
   as appropriate, and take advantage of framework-agnostic tools built for WSGI,
   the Python web interface.

コードの量が多くなってきた時、
Flask

.. Flask includes many hooks to customize its behavior. Should you need more
   customization, the Flask class is built for subclassing. If you are interested
   in that, check out the :ref:`becomingbig` chapter.  If you are curious about
   the Flask design principles, head over to the section about :ref:`design`.

Flaskには、Flask自体の振る舞いをカスタマイズするためのフックがたくさんあります。
さらにカスタマイズする必要があるなら、Flaskクラスのサブクラスを作って下さい。
興味があるなら :ref:`becomingbig` の章を確認して下さい。
Flaskのデザイン規約を不思議に思うなら、 :ref:`design` の章を確認して下さい。

.. Continue to :ref:`installation`, the :ref:`quickstart`, or the
   :ref:`advanced_foreword`.

続いては、 :ref:`installation` 、 :ref:`quickstart` 、 :ref:`advanced_foreword` の章です。
