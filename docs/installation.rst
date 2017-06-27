.. _installation:

.. Installation
   ============

インストール方法
===================

.. Python Version
   --------------

Python のバージョン
-------------------

.. We recommend using the latest version of Python 3. Flask supports Python 3.3
   and newer, Python 2.6 and newer, and PyPy.

Python 3 の最新バージョンを使うことをお勧めします。
Flask は Python 3.3以降、Python 2.6以降、PyPyをサポートしています。

.. Dependencies
   ------------

依存
---------------

.. These distributions will be installed automatically when installing Flask.

以下のディストリビューションは、Flaskのインストール時に自動的にインストールされます。

* `Werkzeug`_ implements WSGI, the standard Python interface between
  applications and servers.
* `Jinja`_ is a template language that renders the pages your application
  serves.
* `MarkupSafe`_ comes with Jinja. It escapes untrusted input when rendering
  templates to avoid injection attacks.
* `ItsDangerous`_ securely signs data to ensure its integrity. This is used
  to protect Flask's session cookie.
* `Click`_ is a framework for writing command line applications. It provides
  the ``flask`` command and allows adding custom management commands.

.. _Werkzeug: http://werkzeug.pocoo.org/
.. _Jinja: http://jinja.pocoo.org/
.. _MarkupSafe: https://pypi.python.org/pypi/MarkupSafe
.. _ItsDangerous: https://pythonhosted.org/itsdangerous/
.. _Click: http://click.pocoo.org/

.. Optional dependencies
   ~~~~~~~~~~~~~~~~~~~~~

オプションの依存
~~~~~~~~~~~~~~~~~~~~~~~~

These distributions will not be installed automatically. Flask will detect and
use them if you install them.

* `Blinker`_ provides support for :ref:`signals`.
* `SimpleJSON`_ is a fast JSON implementation that is compatible with
  Python's ``json`` module. It is preferred for JSON operations if it is
  installed.

.. _Blinker: https://pythonhosted.org/blinker/
.. _SimpleJSON: https://simplejson.readthedocs.io/

.. Virtual environments
   --------------------

仮想環境
--------------------

Use a virtual environment to manage the dependencies for your project, both in
development and in production.

What problem does a virtual environment solve? The more Python projects you
have, the more likely it is that you need to work with different versions of
Python libraries, or even Python itself. Newer versions of libraries for one
project can break compatibility in another project.

Virtual environments are independent groups of Python libraries, one for each
project. Packages installed for one project will not affect other projects or
the operating system's packages.

Python 3 comes bundled with the :mod:`venv` module to create virtual
environments. If you're using a modern version of Python, you can continue on
to the next section.

If you're using Python 2, see :ref:`install-install-virtualenv` first.

.. _install-create-env:

.. Create an environment
   ~~~~~~~~~~~~~~~~~~~~~

環境の作成
~~~~~~~~~~~~~~~~~~~~~

.. Create a project folder and a :file:`venv` folder within:

次の場所にプロジェクトフォルダと :file:`venv` フォルダを作成します。

.. code-block:: sh

    mkdir myproject
    cd myproject
    python3 -m venv venv

.. On Windows:

Windows の場合:

.. code-block:: bat

    py -3 -m venv venv

.. If you needed to install virtualenv because you are on an older version of
   Python, use the following command instead:

旧バージョンの Python を使用しているために virtualenv をインストールする必要がある場合、
代わりに以下のコマンドを実行して下さい。:

.. code-block:: sh

    virtualenv venv

.. On Windows:

Windows の場合:

.. code-block:: bat

    \Python27\Scripts\virtualenv.exe venv

.. Activate the environment
   ~~~~~~~~~~~~~~~~~~~~~~~~

環境のアクティブ化
~~~~~~~~~~~~~~~~~~~~~~~~

.. Before you work on your project, activate the corresponding environment:

プロジェクトで作業する前に、対応する環境をアクティブ化します。:

.. code-block:: sh

    . venv/bin/activate

.. On Windows:

Windows の場合:

.. code-block:: bat

    venv\Scripts\activate

.. Your shell prompt will change to show the name of the activated environment.

シェルプロンプトがアクティブ化された環境の名前が表示されるようになると思います。

.. Install Flask
   -------------

Flask のインストール
-------------------------

.. Within the activated environment, use the following command to install Flask:

アクティブ化された virtualenv の環境内で、以下のコマンドを実行して Flask をインストールします。:

.. code-block:: sh

    pip install Flask

Living on the edge
~~~~~~~~~~~~~~~~~~

.. If you want to work with the latest Flask code before it's released, install or
   update the code from the master branch:

リリースする前に最新の Flask コードで作業する場合は、master ブランチからコードをインストールするか更新して下さい。:

.. code-block:: sh

    pip install -U https://github.com/pallets/flask/archive/master.tar.gz

.. _install-install-virtualenv:

.. Install virtualenv
   ------------------

virtualenv のインストール
-----------------------------

.. If you are using Python 2, the venv module is not available. Instead,
   install `virtualenv`_.

Python 2 を使用している場合、venv モジュールを使うことができません。
代わりに `virtualenv`_ をインストールして下さい。

.. On Linux, virtualenv is provided by your package manager:

Linux の場合、virtualenv はパッケージマネージャーから提供されています。:

.. code-block:: sh

    # Debian, Ubuntu
    sudo apt-get install python-virtualenv

    # CentOS, Fedora
    sudo yum install python-virtualenv

    # Arch
    sudo pacman -S python-virtualenv

.. If you are on Mac OS X or Windows, download `get-pip.py`_, then:

Mac OS X か Windows の場合は、`get-pip.py`_ をダウンロードして、:

.. code-block:: sh

    sudo python2 Downloads/get-pip.py
    sudo python2 -m pip install virtualenv

.. On Windows, as an administrator:

Windows の場合、管理者権限で:

.. code-block:: bat

    \Python27\python.exe Downloads\get-pip.py
    \Python27\python.exe -m pip install virtualenv

.. Now you can continue to :ref:`install-create-env`.

これで、:ref:`install-create-env` に進むことができます。

.. _virtualenv: https://virtualenv.pypa.io/
.. _get-pip.py: https://bootstrap.pypa.io/get-pip.py
