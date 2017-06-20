.. _tutorial-testing:

.. Bonus: Testing the Application
   ==============================

ボーナス: アプリケーションのテスト
=================================

.. Now that you have finished the application and everything works as
   expected, it's probably not a bad idea to add automated tests to simplify
   modifications in the future.  The application above is used as a basic
   example of how to perform unit testing in the :ref:`testing` section of the
   documentation.  Go there to see how easy it is to test Flask applications.

全てが期待したとおりに動作するようになったので、アプリケーションを終了して、将来の修正を用意にするために自動化されたテストを追加することは良いことです。
上記のアプリケーションは、ドキュメントの :ref:`testing` の章で単体テストを実行する基本的な例として使われています。
そこに行って、Flask アプリケーションをテストするのがどれほど簡単か見てみましょう。

.. Adding tests to flaskr
   ----------------------

flaskr にテストを追加
-------------------------

.. Assuming you have seen the :ref:`testing` section and have either written
   your own tests for ``flaskr`` or have followed along with the examples
   provided, you might be wondering about ways to organize the project.

:ref:`testing` の章を見て、``flaskr`` のテストを書いているか、提供されているサンプル通りの場合は、
プロジェクトの構造を不思議に思うかもしれません。

.. One possible and recommended project structure is::

可能性の一つとして、お勧めするプロジェクトの構造は次のとおりです。::

    flaskr/
        flaskr/
            __init__.py
            static/
            templates/
        tests/
            test_flaskr.py
        setup.py
        MANIFEST.in

.. For now go ahead a create the :file:`tests/` directory as well as the 
   :file:`test_flaskr.py` file.

ここでは、:file:`test_flaskr.py` ファイルと同様に :file:`tests/` ディレクトリを作成して下さい。

.. Running the tests
   -----------------

テストの実行
--------------------

.. At this point you can run the tests. Here ``pytest`` will be used. 

この時点でテストを実行することができます。
ここでは、``pytest`` が使われています。

.. Make sure that ``pytest`` is installed in the same virtualenv 
   as flaskr. Otherwise ``pytest`` test will not be able to import the 
   required components to test the application::

       pip install -e .
       pip install pytest 

.. note::

   ``pytest`` が flaskr と同じ virtualenv にインストールされているか確認して下さい。
   そうしないと、``pytest`` はアプリケーションをテストするために必要なコンポーネントをインポートできなくなります。

       pip install -e .
       pip install pytest

.. Run and watch the tests pass, within the top-level :file:`flaskr/` 
   directory as::

トップレベルの :file:`flaskr/` ディレクトリで実行して、テストがパスするか見てみましょう::

    pytest

.. Testing + setuptools
   --------------------

テストとsetuptools
-----------------------

.. One way to handle testing is to integrate it with ``setuptools``. Here
   that requires adding a couple of lines to the :file:`setup.py` file and
   creating a new file :file:`setup.cfg`. One benefit of running the tests 
   this way is that you do not have to install ``pytest``. Go ahead and 
   update the :file:`setup.py` file to contain::

テストを扱う方法の一つとして、``setuptools`` と統合することです。
ここでは、:file:`setup.py` ファイルにコードを数行追加し、新しく :file:`setup.cfg` を作成する必要があります。
この方法でテストを実行する利点の一つは、``pytest`` をインストールする必要がないことです。
:file:`setup.py` に以下を追加してみましょう。::

    from setuptools import setup

    setup(
        name='flaskr',
        packages=['flaskr'],
        include_package_data=True,
        install_requires=[
            'flask',
        ],
        setup_requires=[
            'pytest-runner',
        ],
        tests_require=[
            'pytest',
        ],
    )

.. Now create :file:`setup.cfg` in the project root (alongside
   :file:`setup.py`)::

次に、(:file:`setup.py` と一緒の)プロジェクトルートに :file:`setup.cfg` を作成します。::

    [aliases]
    test=pytest

.. Now you can run::

以下を実行してみましょう::

    python setup.py test

.. This calls on the alias created in :file:`setup.cfg` which in turn runs
   ``pytest`` via ``pytest-runner``, as the :file:`setup.py` script has
   been called. (Recall the `setup_requires` argument in :file:`setup.py`)
   Following the standard rules of test-discovery your tests will be
   found, run, and hopefully pass.

:file:`setup.cfg` で作成されたエイリアスを呼び出します。
:file:`setup.py` スクリプトが呼び出された時に、``pytest-runner`` 経由で ``pytest`` を実行します。
(:file:`setup.py` の `setup_requires` 引数を思い出して下さい)
一般的なテストディスカバリーのルールに従って、テストファイルを探し、実行し、期待通りにテストをパスします。

.. This is one possible way to run and manage testing.  Here ``pytest`` is
   used, but there are other options such as ``nose``.  Integrating testing
   with ``setuptools`` is convenient because it is not necessary to actually
   download ``pytest`` or any other testing framework one might use.

テストを実行して、管理する一つの方法です。
ここでは、``pytest`` を使いましたが、``nose`` のような他の方法もあります。
``setuptools`` とテストを統合することは、``pytest`` や他のテストフレームワークを実際にダウンロードする必要がないので簡単です。
