.. _template-inheritance:

テンプレートの継承
==============================

.. Template Inheritance
   ====================

.. The most powerful part of Jinja is template inheritance. Template inheritance
   allows you to build a base "skeleton" template that contains all the common
   elements of your site and defines **blocks** that child templates can override.

Jinjaの最も強力なところとしてテンプレートの継承が挙げられます。
テンプレートの継承はサイトの一般的な全ての要素を含む
ベースとなるスケルトンのテンプレートを生成することができます。

.. Sounds complicated but is very basic. It's easiest to understand it by starting
   with an example.

複雑なように聞こえるかもしれませんがとても基本的なことです。
サンプルをやってみると理解するのは容易になります。


.. Base Template
   -------------

ベーステンプレート
--------------------------

This template, which we'll call ``layout.html``, defines a simple HTML skeleton
document that you might use for a simple two-column page. It's the job of
"child" templates to fill the empty blocks with content:

.. ``layout.html`` というファイル名のテンプレートは、簡単なHTMLドキュメントのスケルトンです。

.. sourcecode:: html+jinja

    <!doctype html>
    <html>
      <head>
        {% block head %}
        <link rel="stylesheet" href="{{ url_for('static', filename='style.css') }}">
        <title>{% block title %}{% endblock %} - My Webpage</title>
        {% endblock %}
      </head>
    <body>
      <div id="content">{% block content %}{% endblock %}</div>
      <div id="footer">
        {% block footer %}
        &copy; Copyright 2010 by <a href="http://domain.invalid/">you</a>.
        {% endblock %}
      </div>
    </body>

.. In this example, the ``{% block %}`` tags define four blocks that child templates
   can fill in. All the `block` tag does is tell the template engine that a
   child template may override those portions of the template.

このサンプルで、 ``{% block %}`` タグは子テンプレートが挿入される4つのブロックを定義しています。
全ての `block` タグは子テンプレートがテンプレートのこれらの部分を上書きすることをテンプレートエンジンに伝えます。

.. Child Template
   --------------

子テンプレート
----------------------

.. A child template might look like this:

子テンプレートは以下のようになります。 :

.. sourcecode:: html+jinja

    {% extends "layout.html" %}
    {% block title %}Index{% endblock %}
    {% block head %}
      {{ super() }}
      <style type="text/css">
        .important { color: #336699; }
      </style>
    {% endblock %}
    {% block content %}
      <h1>Index</h1>
      <p class="important">
        Welcome on my awesome homepage.
    {% endblock %}

.. The ``{% extends %}`` tag is the key here. It tells the template engine that
   this template "extends" another template.  When the template system evaluates
   this template, first it locates the parent.  The extends tag must be the
   first tag in the template.  To render the contents of a block defined in
   the parent template, use ``{{ super() }}``.

``{% extends %}`` タグはこのセクションのキーです。
テンプレートエンジンに他のテンプレートに "拡張する" ことを伝えます。
テンプレートシステムはこのテンプレートを評価する際に、まず親テンプレートの
extendsタグはテンプレートの最初に出てくるタグにしなければいけません。
親テンプレートで定義されているブロックのコンテンツをレンダリングするためには、
``{{ super() }}`` を使って下さい。
