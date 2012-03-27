.. Pocoo Styleguide
   ================

Pocooのスタイルガイド
==========================

.. The Pocoo styleguide is the styleguide for all Pocoo Projects, including
   Flask.  This styleguide is a requirement for Patches to Flask and a
   recommendation for Flask extensions.

PocooスタイルガイドはFlaskを含む全てのPocooプロジェクトで使用するスタイルガイドです。
このスタイルガイドはFlaskへのパッチには必須で、Flaskのエクステンションにも適用することを推奨しています。

.. In general the Pocoo Styleguide closely follows :pep:`8` with some small
   differences and extensions.

Pocooスタイルガイドは :pep:`8` に従っていて、少し違う部分があったり、拡張していたりしていますが基本的にはとても近いものです。

.. General Layout
   --------------

一般的なレイアウト
--------------------

.. Indentation:
     4 real spaces.  No tabs, no exceptions.

インデント :
  スペース4つ。これに例外はなくタブは使ってはいけません。

.. Maximum line length:
     79 characters with a soft limit for 84 if absolutely necessary.  Try
     to avoid too nested code by cleverly placing `break`, `continue` and
     `return` statements.

一行の長さ :
  ソフトウェアの限界があるので、79文字にして下さい。どうしても必要なら84文字にして下さい。
  何度もネストしているコードは避けるようにして、 `break` 、 `continue` 、 `return` 文を上手く使って下さい。

.. Continuing long statements:
     To continue a statement you can use backslashes in which case you should
     align the next line with the last dot or equal sign, or indent four
     spaces::

長い文を続く場合 :
  文が長くなる場合はバックスラッシュを使って改行し、次の行でドットやイコールの記号で揃えるかスペース4つでインデントするかしてください。 ::

    this_is_a_very_long(function_call, 'with many parameters') \
        .that_returns_an_object_with_an_attribute

    MyModel.query.filter(MyModel.scalar > 120) \
                 .order_by(MyModel.name.desc()) \
                 .limit(10)

  .. If you break in a statement with parentheses or braces, align to the
     braces::

  改行に括弧やブレースがある場合はブレースで揃えて下さい。 ::

    this_is_a_very_long(function_call, 'with many parameters',
                        23, 42, 'and even more')

  .. For lists or tuples with many items, break immediately after the
     opening brace::

  リストやタプルにたくさんの要素がある場合は、最初のブレースの直後で改行してください。 ::

    items = [
        'this is the first', 'set of items', 'with more items',
        'to come in this line', 'like this'
    ]

.. Blank lines:
     Top level functions and classes are separated by two lines, everything
     else by one.  Do not use too many blank lines to separate logical
     segments in code.  Example::

空白行 :
  トップレベルの関数やクラスは二行空けて、他のものは一行にして下さい。
  コードの論理的なセグメントを分けるのに空白行をたくさん入れるべきではない。例として ::

    def hello(name):
        print 'Hello %s!' % name


    def goodbye(name):
        print 'See you %s.' % name


    class MyClass(object):
        """This is a simple docstring"""

        def __init__(self, name):
            self.name = name

        def get_annoying_name(self):
            return self.name.upper() + '!!!!111'

.. Expressions and Statements
   --------------------------

式と分
---------

.. General whitespace rules:
     - No whitespace for unary operators that are not words
       (e.g.: ``-``, ``~`` etc.) as well on the inner side of parentheses.
     - Whitespace is placed between binary operators.

一般的なスペースのルール :
  - 括弧の内側と同じように(例: ``-`` 、 ``~`` 等)、文字列ではない単項演算子には空白を入れてはいけません。
  - 二項演算子の間には空白を入れてください。

  .. Good

  良い例 ::

    exp = -1.05
    value = (item_value / item_count) * offset / exp
    value = my_list[index]
    value = my_dict['key']

  .. Bad

  悪い例 ::

    exp = - 1.05
    value = ( item_value / item_count ) * offset / exp
    value = (item_value/item_count)*offset/exp
    value=( item_value/item_count ) * offset/exp
    value = my_list[ index ]
    value = my_dict ['key']

.. Yoda statements are a no-go:
     Never compare constant with variable, always variable with constant:

ヨーダ文(ヨーダシンタックス)はよくない :
  定数を変数に対して比較してはいけません。常に変数を定数に対して比較して下さい。 :

  .. Good

  良い例 ::

    if method == 'md5':
        pass

  .. Bad

  悪い例 ::

    if 'md5' == method:
        pass

.. Comparisons:
     - against arbitrary types: ``==`` and ``!=``
     - against singletons with ``is`` and ``is not`` (eg: ``foo is not
       None``)
     - never compare something with `True` or `False` (for example never
       do ``foo == False``, do ``not foo`` instead)

比較演算子 :
  - 任意の型に対しては、 ``==`` や ``!=`` を使って下さい。
  - シングルトンに対しては、 ``is`` や ``is not`` を使って下さい(例: ``foo is not None`` )。
  - `True` や `False` は比較してはいけません(例: ``foo == False`` とせずに、代わりに ``not foo`` として下さい)。

.. Negated containment checks:
     use ``foo not in bar`` instead of ``not foo in bar``

Negated containment checks:
  ``not foo in bar`` の代わりに ``foo not in bar`` を使って下さい。

.. Instance checks:
     ``isinstance(a, C)`` instead of ``type(A) is C``, but try to avoid
     instance checks in general.  Check for features.

インスタンスのチェック:
  ``type(A) is C`` の代わりに ``isinstance(a, C)`` を使って下さい。しかし、インスタンスのチェックは避けるようにして下さい。機能をチェックして下さい。

.. Naming Conventions
   ------------------

命名規則
-----------

.. Class names: ``CamelCase``, with acronyms kept uppercase (``HTTPWriter``
   and not ``HttpWriter``)
.. Variable names: ``lowercase_with_underscores``
.. Method and function names: ``lowercase_with_underscores``
.. Constants: ``UPPERCASE_WITH_UNDERSCORES``
.. precompiled regular expressions: ``name_re``

- クラス名: ``キャメルケース`` 、頭文字は大文字( ``HttpWriter`` ではなく ``HTTPWriter`` )
- 変数名: ``lowercase_with_underscores``
- メソッド名や関数名: ``lowercase_with_underscores``
- 定数: ``UPPERCASE_WITH_UNDERSCORES``
- プリコンパイルされた正規表現: ``name_re``

.. Protected members are prefixed with a single underscore.  Double
   underscores are reserved for mixin classes.

プロテクトされたメンバ関数はアンダースコアを一つを前につけて下さい。アンダースコアが二つだとミックスインクラスとして予約されます。

.. On classes with keywords, trailing underscores are appended.  Clashes with
   builtins are allowed and **must not** be resolved by appending an
   underline to the variable name.  If the function needs to access a
   shadowed builtin, rebind the builtin to a different name instead.

キーワード引数を取るクラスについて、末尾にアンダースコアを追加して下さい。
関数が組み込みのプライベートにアクセスする必要がある場合、代わりに違う名前をバインドして下さい。

.. Function and method arguments:
     - class methods: ``cls`` as first parameter
     - instance methods: ``self`` as first parameter
     - lambdas for properties might have the first parameter replaced
       with ``x`` like in ``display_name = property(lambda x: x.real_name
       or x.username)``

関数とメソッドの引数:
  - クラスメソッド: 最初のパラメータは ``cls`` として下さい。
  - インスタンスメソッド: 最初のパラメータは ``self`` として下さい。
  - ラムダのプロパティは ``display_name = property(lambda x: x.real_name or x.username)``
    のように最初のパラメータを ``x`` に置き換えたほうがいいかもしれません。

Docstrings
----------

.. Docstring conventions:
     All docstrings are formatted with reStructuredText as understood by
     Sphinx.  Depending on the number of lines in the docstring, they are
     laid out differently.  If it's just one line, the closing triple
     quote is on the same line as the opening, otherwise the text is on
     the same line as the opening quote and the triple quote that closes
     the string on its own line::

Docstringの規約:
  全てのdocstringは、Sphinxでよく知られているreStructuredTextでフォーマットされています。
  docstringの行数に依存しているので、書き方は異なります。
  一行の場合は、引用符三つで開始した同じ行で閉じて下さい。
  その他は、最初の引用符と同じ行にテキストを書いて、そのテキストの最後に引用符三つの行を作成して下さい。 ::

    def foo():
        """This is a simple docstring"""


    def bar():
        """This is a longer docstring with so much information in there
        that it spans three lines.  In this case the closing triple quote
        is on its own line.
        """

.. Module header:
     The module header consists of an utf-8 encoding declaration (if non
     ASCII letters are used, but it is recommended all the time) and a
     standard docstring::

モジュールヘッダー:
  モジュールのヘッダーは、utf-8エンコーディングの宣言をして下さい。(ASCII文字が使われていない場合、書くことを推奨しています)
  docstringの標準的な書き方は以下のとおりです。 ::

    # -*- coding: utf-8 -*-
    """
        package.module
        ~~~~~~~~~~~~~~

        A brief description goes here.

        :copyright: (c) YEAR by AUTHOR.
        :license: LICENSE_NAME, see LICENSE_FILE for more details.
    """

  .. Please keep in mind that proper copyrights and license files are a
     requirement for approved Flask extensions.

  適切なコピーライトやライセンスファイルはFlaskのエクステンションとして承認されるために必要なものだということを気に停めておいて下さい。

.. Comments
   --------

コメント
-----------

.. Rules for comments are similar to docstrings.  Both are formatted with
   reStructuredText.  If a comment is used to document an attribute, put a
   colon after the opening pound sign (``#``)::

コメントのルールはdocstringsと似ています。どちらもreStructuredTextがフォーマットです。
もしドキュメントのアトリビュートにコメントを使っているなら、ポンド記号(``#``)の後にコロンを付けて下さい。 ::

    class User(object):
        #: the name of the user as unicode string
        name = Column(String)
        #: the sha1 hash of the password + inline salt
        pw_hash = Column(String)
