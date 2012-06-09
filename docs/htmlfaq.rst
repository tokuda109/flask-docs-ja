.. HTML/XHTML FAQ
   ==============

HTMLとXHTMLのよくある質問
===============================

.. The Flask documentation and example applications are using HTML5.  You
   may notice that in many situations, when end tags are optional they are
   not used, so that the HTML is cleaner and faster to load.  Because there
   is much confusion about HTML and XHTML among developers, this document tries
   to answer some of the major questions.

FlaskのドキュメントとサンプルのアプリケーションはHTML5が使われています。
HTMLをよりきれいに、そしてロードするために速くなるように、多くの状況で、終了タグが省略可能である場合、それらが使用されないことがあります。
開発者の間でHTMLとXHTMLについてかなりの混乱があるので、このドキュメントでは、代表的な質問にいくつか答えます。

.. History of XHTML
   ----------------

XHTMLの歴史
----------------

.. For a while, it appeared that HTML was about to be replaced by XHTML.
   However, barely any websites on the Internet are actual XHTML (which is
   HTML processed using XML rules).  There are a couple of major reasons
   why this is the case.  One of them is Internet Explorer's lack of proper
   XHTML support. The XHTML spec states that XHTML must be served with the MIME
   type `application/xhtml+xml`, but Internet Explorer refuses to read files
   with that MIME type.
   While it is relatively easy to configure Web servers to serve XHTML properly,
   few people do.  This is likely because properly using XHTML can be quite
   painful.

しばらくの間、HTMLはXHTMLによって置き換えられようとしています。
しかし、実際にはXHTMLはインターネット上でかなり少数のウェブサイトでしか使われていません。
この
その理由の内の一つが、インターネットエクスプローラーがXHTMLを正しくサポートしていないことです。
XHTMLの仕様は、MIMEタイプが `application/xhtml+xml` で配信するように定められていますが、
インターネットエクスプローラーは、そのMIMEタイプだとファイルの読み込みを止めてしまいます。


.. One of the most important causes of pain is XML's draconian (strict and
   ruthless) error handling.  When an XML parsing error is encountered,
   the browser is supposed to show the user an ugly error message, instead
   of attempting to recover from the error and display what it can.  Most of
   the (X)HTML generation on the web is based on non-XML template engines
   (such as Jinja, the one used in Flask) which do not protect you from
   accidentally creating invalid XHTML.  There are XML based template engines,
   such as Kid and the popular Genshi, but they often come with a larger
   runtime overhead and, are not as straightforward to use because they have
   to obey XML rules.

一番重要な原因の内の一つがXMLの厳格(strict and ruthless)なエラー処理です。
XMLのパースでエラーが発生した時、エラーを回避しようとしたり、何ができるかを表示する代わりに、
ブラウザは酷いエラーメッセージをユーザーに表示するようになっています。


The majority of users, however, assumed they were properly using XHTML.
They wrote an XHTML doctype at the top of the document and self-closed all
the necessary tags (``<br>`` becomes ``<br/>`` or ``<br></br>`` in XHTML).
However, even if the document properly validates as XHTML, what really
determines XHTML/HTML processing in browsers is the MIME type, which as
said before is often not set properly. So the valid XHTML was being treated
as invalid HTML.

XHTML also changed the way JavaScript is used. To properly work with XHTML,
programmers have to use the namespaced DOM interface with the XHTML
namespace to query for HTML elements.

.. History of HTML5
   ----------------

HTML5の歴史
-------------------

.. Development of the HTML5 specification was started in 2004 under the name
   "Web Applications 1.0" by the Web Hypertext Application Technology Working
   Group, or WHATWG (which was formed by the major browser vendors Apple,
   Mozilla, and Opera) with the goal of writing a new and improved HTML
   specification, based on existing browser behavior instead of unrealistic
   and backwards-incompatible specifications.

HTML5の仕様の策定は、HTMLの仕様を改良したり新しく作成したりするのを目的として、
Web Hypertext Application Technology Working GroupやWHATWG(AppleやMozillaやOperaなどの主要なブラウザベンダーに構成される)
によって "Web Applications 1.0" として2004年に始まりました。
後方互換性のない仕様で

For example, in HTML4 ``<title/Hello/`` theoretically parses exactly the
same as ``<title>Hello</title>``.  However, since people were using
XHTML-like tags along the lines of ``<link />``, browser vendors implemented
the XHTML syntax over the syntax defined by the specification.

例えば、HTML4で ``<title/Hello/`` は ``<title>Hello</title>`` と同じようにパースされます。
しかし、XHTML

In 2007, the specification was adopted as the basis of a new HTML
specification under the umbrella of the W3C, known as HTML5.  Currently,
it appears that XHTML is losing traction, as the XHTML 2 working group has
been disbanded and HTML5 is being implemented by all major browser vendors.

2007年に、仕様は

.. HTML versus XHTML
   -----------------

HTML対XHTML
--------------------

.. The following table gives you a quick overview of features available in
   HTML 4.01, XHTML 1.1 and HTML5. (XHTML 1.0 is not included, as it was
   superseded by XHTML 1.1 and the barely-used XHTML5.)

以下の表は、HTML 4.01、XHTML 1.1、HTML5で有効な機能を確認することができます。
(XHTML 1.0は、代わりにXHTML 1.1があって、HTML5でほとんど使われていないので含まれていません。)

.. tabularcolumns:: |p{9cm}|p{2cm}|p{2cm}|p{2cm}|

+-----------------------------------------+----------+----------+----------+
|                                         | HTML4.01 | XHTML1.1 | HTML5    |
+=========================================+==========+==========+==========+
| ``<tag/value/`` == ``<tag>value</tag>`` | |Y| [1]_ | |N|      | |N|      |
+-----------------------------------------+----------+----------+----------+
| ``<br/>`` supported                     | |N|      | |Y|      | |Y| [2]_ |
+-----------------------------------------+----------+----------+----------+
| ``<script/>`` supported                 | |N|      | |Y|      | |N|      |
+-----------------------------------------+----------+----------+----------+
| should be served as `text/html`         | |Y|      | |N| [3]_ | |Y|      |
+-----------------------------------------+----------+----------+----------+
| should be served as                     | |N|      | |Y|      | |N|      |
| `application/xhtml+xml`                 |          |          |          |
+-----------------------------------------+----------+----------+----------+
| strict error handling                   | |N|      | |Y|      | |N|      |
+-----------------------------------------+----------+----------+----------+
| inline SVG                              | |N|      | |Y|      | |Y|      |
+-----------------------------------------+----------+----------+----------+
| inline MathML                           | |N|      | |Y|      | |Y|      |
+-----------------------------------------+----------+----------+----------+
| ``<video>`` tag                         | |N|      | |N|      | |Y|      |
+-----------------------------------------+----------+----------+----------+
| ``<audio>`` tag                         | |N|      | |N|      | |Y|      |
+-----------------------------------------+----------+----------+----------+
| New semantic tags like ``<article>``    | |N|      | |N|      | |Y|      |
+-----------------------------------------+----------+----------+----------+

.. This is an obscure feature inherited from SGML. It is usually not
   supported by browsers, for reasons detailed above.
.. This is for compatibility with server code that generates XHTML for
   tags such as ``<br>``.  It should not be used in new code.
.. XHTML 1.0 is the last XHTML standard that allows to be served
   as `text/html` for backwards compatibility reasons.

.. [1] これはSGMLから継承されている曖昧な機能です。
       これは通常、上記の詳細に述べた理由のために、ブラウザによってはサポートされていません。
.. [2] これは、 ``<br>`` などのタグのためにXHTMLを生成するサーバーコードとの互換性のためです。
       新しいコードで使うべきではありません。
.. [3] XHTML 1.0は、後方互換性のために `text/html` として配信することができる最後のXHTML仕様です。

.. |Y| image:: _static/yes.png
       :alt: Yes
.. |N| image:: _static/no.png
       :alt: No

.. What does "strict" mean?
   ------------------------

"strict" が意味するものは?
---------------------------

.. HTML5 has strictly defined parsing rules, but it also specifies exactly
   how a browser should react to parsing errors - unlike XHTML, which simply
   states parsing should abort. Some people are confused by apparently
   invalid syntax that still generates the expected results (for example,
   missing end tags or unquoted attribute values).

HTML5は、パースするルールが厳密に定義されていますが、ブラウザーがパースエラーに対処する
一部の人は、明らかに無効な構文によって混乱しています。
(例えば、終了タグがなかったり、アトリビュートの値に引用符をつけていなかったりします)
HTML5は、厳密に解析ルールが定義されていますが、それはまた、ブラウザがパースエラーに対処する必要があります正確にどのように指定します - 
単に解析は中止すべきと述べXHTMLとは異なり、。
一部の人々はまだ（たとえば、欠落している終了タグや引用符で囲まれていない属性値）期待される結果を生成し、明らかに無効な構文で混乱しています。

.. Some of these work because of the lenient error handling most browsers use
   when they encounter a markup error, others are actually specified.  The
   following constructs are optional in HTML5 by standard, but have to be
   supported by browsers:

これらの作業の一部はマークアップエラーが発生した場合、ほとんどのブラウザが使用する寛大なエラー処理のためには、他のものは実際に指定されています。
以下の構文は標準でHTML5ではオプションですが、ブラウザでサポートされなければならない :

.. Wrapping the document in an ``<html>`` tag
.. Wrapping header elements in ``<head>`` or the body elements in
   ``<body>``
.. Closing the ``<p>``, ``<li>``, ``<dt>``, ``<dd>``, ``<tr>``,
   ``<td>``, ``<th>``, ``<tbody>``, ``<thead>``, or ``<tfoot>`` tags.
.. Quoting attributes, so long as they contain no whitespace or
   special characters (like ``<``, ``>``, ``'``, or ``"``).
.. Requiring boolean attributes to have a value.

- ドキュメントは ``<html>`` タグで囲んで下さい。
- ヘッダー要素を ``<head>`` で、本文要素を ``<body>`` で囲んで下さい。
- ``<p>`` 、 ``<li>`` 、 ``<dt>`` 、 ``<dd>`` 、 ``<tr>`` 、 ``<td>`` 、
  ``<th>`` 、 ``<tbody>`` 、 ``<thead>`` 、 ``<tfoot>`` タグは終了タグで閉じて下さい。
- アトリビュートの引用符
- 値を持つブーリアンアトリビュートは必須です。

.. This means the following page in HTML5 is perfectly valid:

これは、HTML5で次のページが完全に有効であることを意味しています。 :

.. sourcecode:: html

    <!doctype html>
    <title>Hello HTML5</title>
    <div class=header>
      <h1>Hello HTML5</h1>
      <p class=tagline>HTML5 is awesome
    </div>
    <ul class=nav>
      <li><a href=/index>Index</a>
      <li><a href=/downloads>Downloads</a>
      <li><a href=/about>About</a>
    </ul>
    <div class=body>
      <h2>HTML5 is probably the future</h2>
      <p>
        There might be some other things around but in terms of
        browser vendor support, HTML5 is hard to beat.
      <dl>
        <dt>Key 1
        <dd>Value 1
        <dt>Key 2
        <dd>Value 2
      </dl>
    </div>


.. New technologies in HTML5
   -------------------------

HTML5の新しい技術
----------------------

.. HTML5 adds many new features that make Web applications easier to write
   and to use.

HTML5はウェブアプリケーションを作る時に、より簡単に書けて、より簡単に使える新しい機能がたくさん追加されました。

.. The ``<audio>`` and ``<video>`` tags provide a way to embed audio and
   video without complicated add-ons like QuickTime or Flash.
.. Semantic elements like ``<article>``, ``<header>``, ``<nav>``, and
   ``<time>`` that make content easier to understand.
.. The ``<canvas>`` tag, which supports a powerful drawing API, reducing
   the need for server-generated images to present data graphically.
.. New form control types like ``<input type="date">`` that allow user
   agents to make entering and validating values easier.
.. Advanced JavaScript APIs like Web Storage, Web Workers, Web Sockets,
   geolocation, and offline applications.

- ``<audio>`` タグと ``<video>`` タグはQuickTimeやFlashのような紛らわしいアドオンを使わずに
  オーディオファイルとビデオファイルを埋め込むことができます。
- ``<article>`` 、 ``<header>`` 、 ``<nav>`` 、 ``<time>`` のようなセマンティックな要素
  は内容の理解をより簡単にします。
- ``<canvas>`` タグは、データをグラフィカルに表示するために、
  サーバーで生成された画像を必要としない強力な描画APIをサポートしています。
- ``<input type="date">`` のような新しいフォームは入力値のバリデーションと
  フォームの入力
- ウェブストレージ、ウェブワーカー、ウェブソケット、ジオロケーション、オフラインアプリケーション
  のような高度なJavaScriptのAPIがあります。

.. Many other features have been added, as well. A good guide to new features
   in HTML5 is Mark Pilgrim's soon-to-be-published book, `Dive Into HTML5`_.
   Not all of them are supported in browsers yet, however, so use caution.

他にもたくさんの機能が追加されました。
HTML5の新しい機能の良いガイドラインは、Mark Pilgrimのまもなく出版される `Dive Into HTML5`_ という本です。
全てが、まだブラウザにサポートされているものだけではないので、注意して使って下さい。

.. _Dive Into HTML5: http://www.diveintohtml5.org/

.. What should be used?
   --------------------

どちらを使うべきでしょうか？
-----------------------------

.. Currently, the answer is HTML5.  There are very few reasons to use XHTML
   considering the latest developments in Web browsers.  To summarize the
   reasons given above:

今答えるとしたらHTML5です。ウェブブラウザの最新動向が考慮されているXHTMLを使う理由はいくつかあります。
その理由を要約します。 :

.. Internet Explorer (which, sadly, currently leads in market share)
   has poor support for XHTML.
.. Many JavaScript libraries also do not support XHTML, due to the more
   complicated namespacing API it requires.
.. HTML5 adds several new features, including semantic tags and the
   long-awaited ``<audio>`` and ``<video>`` tags.
.. It has the support of most browser vendors behind it.
.. It is much easier to write, and more compact.

- Internet Explorer(悲しいことに、現在シェアのトップです)はXHTMLをほとんどサポートしていない。
- ほとんどのJavaScriptライブラリは、要求される名前空間が複雑になるためXHTMLをサポートしていない。
- HTML5では、望まれていた ``<audio>`` タグや ``<video>`` タグや
  セマンティックなタグのような幾つかのあたらしいタグが追加されています。
- ほとんどのブラウザベンダーがサポートしています。
- より簡単で、より簡略して書けます。

.. For most applications, it is undoubtedly better to use HTML5 than XHTML.

ほとんどのアプリケーションにおいて、XHTMLよりHTML5を使うほうが明らかにいいです。
