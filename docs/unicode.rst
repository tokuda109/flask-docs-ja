.. Unicode in Flask
   ================

Flaskにおけるユニコード
=======================

.. Flask like Jinja2 and Werkzeug is totally Unicode based when it comes to
   text.  Not only these libraries, also the majority of web related Python
   libraries that deal with text.  If you don't know Unicode so far, you
   should probably read `The Absolute Minimum Every Software Developer
   Absolutely, Positively Must Know About Unicode and Character Sets
   <http://www.joelonsoftware.com/articles/Unicode.html>`_.  This part of the
   documentation just tries to cover the very basics so that you have a
   pleasant experience with Unicode related things.

Jinja2とWerkzeugのようにFlaskは文字を処理する時に完全にユニコードをベースにしています。
これらのライブラリは、文字を扱うウェブ関連のPythonライブラリの大部分だけではなく
ユニコードをそんなに知らないなら、 `The Absolute Minimum Every Software Developer Absolutely, 
Positively Must Know About Unicode and Character Sets
<http://www.joelonsoftware.com/articles/Unicode.html>`_ を読んで下さい。
ドキュメントのこの章では、ユニコードに関連する事で楽しい経験があるので、基本的なところをカバーしようと思います。

.. Automatic Conversion
   --------------------

自動変換
-----------

.. Flask has a few assumptions about your application (which you can change
   of course) that give you basic and painless Unicode support:

Flaskはアプリケーションに対していくつかの仮定をしています(もちろん変更することができます)。
基本的で簡単なユニコードをサポートしています。 :

.. the encoding for text on your website is UTF-8
.. internally you will always use Unicode exclusively for text except
   for literal strings with only ASCII character points.
.. encoding and decoding happens whenever you are talking over a protocol
   that requires bytes to be transmitted.

- ウェブサイトの文字のエンコードはUTF-8です。
- 内部では常にユニコードが使われています。リテラル文字列
- エンコードとデコードは

.. So what does this mean to you?

これは何を意味しているのでしょうか?

.. HTTP is based on bytes.  Not only the protocol, also the system used to
   address documents on servers (so called URIs or URLs).  However HTML which
   is usually transmitted on top of HTTP supports a large variety of
   character sets and which ones are used, are transmitted in an HTTP header.
   To not make this too complex Flask just assumes that if you are sending
   Unicode out you want it to be UTF-8 encoded.  Flask will do the encoding
   and setting of the appropriate headers for you.

HTTPはバイトが基本です。
プロトコルだけではなく、システムもサーバー上のファイルの場所(URIやURLと言われている)もそうです。
しかし、HTTPで通信されるHTMLは、様々な文字コードをサポートしていて、どの文字コードかを、HTTPヘッダーで通信されます。
難しくしないためにFlaskはユニコード以外を送信してきた場合に、自動的にUTF-8にエンコードします。
Flaskは適切なヘッダーにセットしてエンコードします。

.. The same is true if you are talking to databases with the help of
   SQLAlchemy or a similar ORM system.  Some databases have a protocol that
   already transmits Unicode and if they do not, SQLAlchemy or your other ORM
   should take care of that.

SQLAlchemyや同じようなORMシステムがデータベースと通信するときも同じです。
いくつかのデータベースは、もう既にユニコード通信サポートしています。
サポートしていない場合は、SQLAlchemyや他のORMを使う時に注意して下さい。

.. The Golden Rule
   ---------------

ゴールデンルール
------------------

.. So the rule of thumb: if you are not dealing with binary data, work with
   Unicode.  What does working with Unicode in Python 2.x mean?

経験論として、バイナリデータを扱う以外はユニコードで作業します。
Python 2.x でユニコードで作業することは何を意味しているのでしょうか?

.. as long as you are using ASCII charpoints only (basically numbers,
   some special characters of latin letters without umlauts or anything
   fancy) you can use regular string literals (``'Hello World'``).
.. if you need anything else than ASCII in a string you have to mark
   this string as Unicode string by prefixing it with a lowercase `u`.
   (like ``u'Hänsel und Gretel'``)
.. if you are using non-Unicode characters in your Python files you have
   to tell Python which encoding your file uses.  Again, I recommend
   UTF-8 for this purpose.  To tell the interpreter your encoding you can
   put the ``# -*- coding: utf-8 -*-`` into the first or second line of
   your Python source file.
.. Jinja is configured to decode the template files from UTF-8.  So make
   sure to tell your editor to save the file as UTF-8 there as well.

- ASCII文字列(基本的に数字やウムラウトや特殊な文字以外のラテン文字の特殊文字列)しか使っていないなら、
  ``'Hello World'`` のように文字列リテラルを使えます。
- もし文字列にASCII以外を使う場合は、この文字を (``u'Hänsel und Gretel'``)のように小文字の
  `u` を文字列の前に付けることでユニコード文字列とします。
- もしPythonファイルでユニコード以外の文字を使っている場合は、Pythonにファイルで使っている文字コードを
  教えないといけません。何度も言いますが、だからUTF-8を使うことをお勧めしています。
  Pythonファイルの一行目か二行目に ``# -*- coding: utf-8 -*-`` と入力してエンコーディングを
  インタープリターに教えないといけません。
- JinjaはUTF-8からテンプレートファイルのデコードが設定されています。

.. Encoding and Decoding Yourself
   ------------------------------

エンコードとデコード
-------------------------

.. If you are talking with a filesystem or something that is not really based
   on Unicode you will have to ensure that you decode properly when working
   with Unicode interface.  So for example if you want to load a file on the
   filesystem and embed it into a Jinja2 template you will have to decode it
   from the encoding of that file.  Here the old problem that text files do
   not specify their encoding comes into play.  So do yourself a favour and
   limit yourself to UTF-8 for text files as well.

ユニコードをベースにしていないファイルシステムとかで実行している場合、
ユニコードで作業している時適切にデコードしているか確認する必要があります。
ファイルシステムにファイルを読み込み
Jinja2テンプレートに埋め込む
そのファイルのエンコードからデコードする必要があります。
エンコーディングを指定していないテキストファイルが古い問題として出てきました。
テキストファイルをUTF-8に自身で制限して下さい。

.. Anyways.  To load such a file with Unicode you can use the built-in
   :meth:`str.decode` method::

とにかく、そのようなファイルをユニコードで読み込むためには組み込みの :meth:`str.decode` を使って下さい。 ::

    def read_file(filename, charset='utf-8'):
        with open(filename, 'r') as f:
            return f.read().decode(charset)

.. To go from Unicode into a specific charset such as UTF-8 you can use the
   :meth:`unicode.encode` method::

ユニコードからUTF-8のような特定の文字コードにするには :meth:`unicode.encode` メソッドを使って下さい。 ::

    def write_file(filename, contents, charset='utf-8'):
        with open(filename, 'w') as f:
            f.write(contents.encode(charset))

.. Configuring Editors
   -------------------

エディタの設定
------------------

.. Most editors save as UTF-8 by default nowadays but in case your editor is
   not configured to do this you have to change it.  Here some common ways to
   set your editor to store as UTF-8:

ほとんどのエディタは最近は標準でUTF-8で保存しますが、
エディタがそのように設定されていない場合は変更しないといけません。
ここでは、エディタをUTF-8として保存するように設定する一般的な方法を紹介します。

.. Vim: put ``set enc=utf-8`` to your ``.vimrc`` file.

- Vim: ``.vimrc`` ファイルに ``set enc=utf-8`` と入力します。

.. Emacs: either use an encoding cookie or put this into your ``.emacs``
   file::

- Emacs: エンコーディングのクッキーを使うか、 ``.emacs`` ファイルに以下のように入力します。 ::

        (prefer-coding-system 'utf-8)
        (setq default-buffer-file-coding-system 'utf-8)

.. Notepad++:

- ノートパッド等:

    .. 1. Go to *Settings -> Preferences ...*
    .. 2. Select the "New Document/Default Directory" tab
    .. 3. Select "UTF-8 without BOM" as encoding

    1. 「Settings」の「Preferences」へ
    2. 「New Document/Default Directory」タブを選択
    3. エンコードのところを「UTF-8 without BOM」を選択

    .. It is also recommended to use the Unix newline format, you can select
       it in the same panel but this is not a requirement.

    Unixの改行形式を使用することをお勧めします。同じパネルで選択することができますが必須ではありません。
