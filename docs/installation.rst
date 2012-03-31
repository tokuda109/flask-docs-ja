.. _installation:

インストールする方法
======================

.. Installation
   ============

.. Flask depends on two external libraries, `Werkzeug
   <http://werkzeug.pocoo.org/>`_ and `Jinja2 <http://jinja.pocoo.org/2/>`_.
   Werkzeug is a toolkit for WSGI, the standard Python interface between web
   applications and a variety of servers for both development and deployment.
   Jinja2 renders templates.

Flaskは、 `Werkzeug <http://werkzeug.pocoo.org/>`_ と `Jinja2 <http://jinja.pocoo.org/2/>`_ の
2つの外部ライブラリに依存しています。Werkzeugは、WSGIのツールキットで、開発やデプロイをする時に、
ウェブアプリケーションと様々なサーバーの間の標準的なPythonインターフェースとなります。
Jinja2は、テンプレートをレンダリングします。

.. So how do you get all that on your computer quickly?  There are many ways you
   could do that, but the most kick-ass method is virtualenv, so let's have a look
   at that first.

手っ取り早くコンピューターにすべて取得するにはどのようにしますか?
方法はたくさんありますが、virtualenvは素晴らしい手段です。
では、まずは見てみましょう。

.. You will need Python 2.5 or higher to get started, so be sure to have an
   up-to-date Python 2.x installation.  At the time of writing, the WSGI
   specification has not yet been finalized for Python 3, so Flask cannot support
   the 3.x series of Python.

使うにはPython 2.5かそれ以上のバージョンが必要なので、Python 2.xの最新バージョンにするようにして下さい。
これを書いている時点で、WSGIの仕様はPython 3でまだ最終決定していないので、FlaskはPython 3.xシリーズをサポートしていません。

.. _virtualenv:

virtualenv
----------

.. Virtualenv is probably what you want to use during development, and if you have
   shell access to your production machines, you'll probably want to use it there,
   too.

Virtualenvは、開発中に使いたいと思うもので、本番環境においてもシェルでアクセスする場合におそらく使いたいと思うものだと思います。

.. What problem does virtualenv solve?  If you like Python as much as I do,
   chances are you want to use it for other projects besides Flask-based web
   applications.  But the more projects you have, the more likely it is that you
   will be working with different versions of Python itself, or at least different
   versions of Python libraries.  Let's face it: quite often libraries break
   backwards compatibility, and it's unlikely that any serious application will
   have zero dependencies.  So what do you do if two or more of your projects have
   conflicting dependencies?

Virtualenvはどのような問題を解決するのでしょうか?
私がそうであるようにあなたもPythonが好きなら、Flaskベースのウェブアプリケーション以外の他のプロジェクトでも使う可能性があります。
しかし、他のプロジェクトがあるなら、異なるPythonのバージョンや異なるPythonライブラリのバージョンで作業する可能性が高いと思います。
そういう状況に直面したら、ライブラリはかなりの頻度で後方互換性がなくなります。
そして、アプリケーションが依存関係を持っていないということはまずありません。
プロジェクトで2つ、もしくはそれ以上の依存関係で競合しているなら、どうすればいいのでしょうか?

.. Virtualenv to the rescue!  Virtualenv enables multiple side-by-side
   installations of Python, one for each project.  It doesn't actually install
   separate copies of Python, but it does provide a clever way to keep different
   project environments isolated.  Let's see how virtualenv works.

Virtualenvが解決します!
Virtualenvはそれぞれのプロジェクト毎に一つ、Pythonを複数インストールすることができます。
実際はPythonのコピーを別々にインストールするのではなく、異なるプロジェクト環境毎に隔離された方法を提供します。
では、Virtualenvをどのように使うか見てみましょう!

.. If you are on Mac OS X or Linux, chances are that one of the following two
   commands will work for you::

Mac OS XかLinuxを使っているなら、以下の2つのコマンドの内のどちらかで使うことができるでしょう。 ::

    $ sudo easy_install virtualenv

.. or even better::

またはよりいい方法として、 ::

    $ sudo pip install virtualenv

.. One of these will probably install virtualenv on your system.  Maybe it's even
   in your package manager.  If you use Ubuntu, try::

以上の方法のどちらかで、システムにVirtualenvがインストールされるでしょう。
もしかすると、パッケージマネージャーを使いたくて、もしUbuntuを使っているなら以下を試して下さい。 ::

    $ sudo apt-get install python-virtualenv

.. If you are on Windows and don't have the `easy_install` command, you must
   install it first.  Check the :ref:`windows-easy-install` section for more
   information about how to do that.  Once you have it installed, run the same
   commands as above, but without the `sudo` prefix.

もしWindowsを使っていて、 `easy_install` コマンドが入っていないなら、まずそれをインストールして下さい。
インストールする方法についての詳細は、 :ref:`windows-easy-install` の章をチェックして下さい。
一度インストールしているなら上の同じコマンドを `sudo` プリフィックスなしで実行して下さい。

.. Once you have virtualenv installed, just fire up a shell and create
   your own environment.  I usually create a project folder and an `env`
   folder within::

Virtualenvをインストールしているなら、シェルを立ち上げて、自身の環境を作成して下さい。
通常は `env` フォルダ内やプロジェクトフォルダを作成します。 ::

    $ mkdir myproject
    $ cd myproject
    $ virtualenv venv
    New python executable in venv/bin/python
    Installing distribute............done.

.. Now, whenever you want to work on a project, you only have to activate the
   corresponding environment.  On OS X and Linux, do the following::

プロジェクトで作業する場合環境をアクティベートする必要があります。
OS XとLinuxでは、以下のコマンドを実行します。 ::

    $ . env/bin/activate

.. If you are a Windows user, the following command is for you::

Windowsユーザーなら、以下のコマンドを実行して下さい。 ::

    $ venv\scripts\activate

.. Either way, you should now be using your virtualenv (notice how the prompt of
   your shell has changed to show the active environment).

いずれにせよ、Virtualenvを使える状態になっています(シェルのプロンプトがvirtualenvの表示に切り替わっているのが分かると思います)。

.. Now you can just enter the following command to get Flask activated in your
   virtualenv::

Virtualenvがアクティブの状態で、以下のコマンドを実行するだけでFlaskを取得することができます。 ::

    $ pip install Flask

.. A few seconds later and you are good to go.

数秒後には使えます。

.. System-Wide Installation
   ------------------------

システムにインストールする方法
-----------------------------------

.. This is possible as well, though I do not recommend it.  Just run
   `easy_install` with root privileges::

これは同じことができますが、私はおすすめはしません。
root権限で `easy_install` を実行するだけです。 ::

    $ sudo pip install Flask

.. (On Windows systems, run it in a command-prompt window with administrator
   privileges, and leave out `sudo`.)

(Windowsでは、 `sudo` なしで管理者権限でコマンドプロンプトで実行できます。)

.. Living on the Edge
   ------------------

Living on the Edge
------------------

.. If you want to work with the latest version of Flask, there are two ways: you
   can either let `pip` pull in the development version, or you can tell
   it to operate on a git checkout.  Either way, virtualenv is recommended.

もし、Flaskの最新バージョンで作業する場合は、2つの方法があります。:
`pip` で開発バージョンをプルするかgit checkoutで操作するように指示するかのどちらかをできます。
いずれにせよ、Virtualenvをお勧めします。

.. Get the git checkout in a new virtualenv and run in development mode::

新しいVirtualenvでgit checkoutで取得して、開発モードで実行します。 ::

    $ git clone http://github.com/mitsuhiko/flask.git
    Initialized empty Git repository in ~/dev/flask/.git/
    $ cd flask
    $ virtualenv venv --distribute
    New python executable in venv/bin/python
    Installing distribute............done.
    $ . venv/bin/activate
    $ python setup.py develop
    ...
    Finished processing dependencies for Flask

.. This will pull in the dependencies and activate the git head as the current
   version inside the virtualenv.  Then all you have to do is run ``git pull
   origin`` to update to the latest version.

これは依存するものもプルして、Virtualenv内の現在のバージョンにgit headを有効化します。
それから、最新バージョンを取得するために ``git pull origin`` をする必要があります。

.. To just get the development version without git, do this instead::

gitを使わずに開発バージョンを使うためには、代わりに以下を実行して下さい。 ::

    $ mkdir flask
    $ cd flask
    $ virtualenv venv --distribute
    $ . venv/bin/activate
    New python executable in venv/bin/python
    Installing distribute............done.
    $ pip install Flask==dev
    ...
    Finished processing dependencies for Flask==dev

.. _windows-easy-install:

Windowsで `pip` と `distribute`
-----------------------------------

.. `pip` and `distribute` on Windows
   -----------------------------------

.. On Windows, installation of `easy_install` is a little bit trickier, but still
   quite easy.  The easiest way to do it is to download the
   `distribute_setup.py`_ file and run it.  The easiest way to run the file is to
   open your downloads folder and double-click on the file.

Windowsにおいて `easy_install` のインストールは少しトリッキーですが、まだ簡単です。
一番簡単な方法は、 `distribute_setup.py`_ ファイルをダウンロードして実行するだけです。
ファイルを実行する一番簡単な方法は、ダウンロードしたフォルダを開いて、ファイルをダブルクリックして下さい。

.. Next, add the `easy_install` command and other Python scripts to the
   command search path, by adding your Python installation's Scripts folder
   to the `PATH` environment variable.  To do that, right-click on the
   "Computer" icon on the Desktop or in the Start menu, and choose "Properties".
   Then click on "Advanced System settings" (in Windows XP, click on the
   "Advanced" tab instead).  Then click on the "Environment variables" button.
   Finally, double-click on the "Path" variable in the "System variables" section,
   and add the path of your Python interpreter's Scripts folder. Be sure to
   delimit it from existing values with a semicolon.  Assuming you are using
   Python 2.7 on the default path, add the following value::

次に、PythonインタープリターのScriptsフォルダを `PATH` 環境変数に追加することによって、
`easy_install` コマンドと他のPythonスクリプトをコマンドサーチパスに追加します。
それをするためには、デスクトップ上の "Computer" アイコンかスタートメニューのアイコンを右クリックして
"Properties" を選択して下さい。
それから、Windows VistaとWindows 7では、 "Advanced System settings" をクリックして下さい。
Windows XPでは代わりに "Advanced" タブをクリックして下さい。
それから、 "Environment variables" ボタンをクリックして、 "System variables" セクションの
"Path" をダブルクリックして下さい。PythonインタープリターのScriptsフォルダのパスを追加して下さい。
デフォルトのパスにPython 2.7を使っていると仮定する場合は、以下の値を追加して下さい。 ::

    ;C:\Python27\Scripts

.. And you are done!  To check that it worked, open the Command Prompt and execute
   ``easy_install``.  If you have User Account Control enabled on Windows Vista or
   Windows 7, it should prompt you for administrator privileges.

これで終わりです。
動作するかチェックするには、コマンドプロンプトを開いて、 ``easy_install`` を実行して下さい。
Windows VistaかWindows でユーザーアカウントコントロールが有効なら、管理者権限でプロンプトを
表示して下さい。

.. Now that you have ``easy_install``, you can use it to install ``pip``::

``easy_install`` があるので、 ``pip`` をインストールするために使います。 ::

    > easy_install pip


.. _distribute_setup.py: http://python-distribute.org/distribute_setup.py
