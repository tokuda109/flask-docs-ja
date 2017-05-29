.. _tutorial-schema:

.. Step 1: Database Schema
   =======================

ステップ1: データベース スキーマ
=========================================

.. In this step, you will create the database schema.  Only a single table is
   needed for this application and it will only support SQLite.  All you need to do
   is put the following contents into a file named :file:`schema.sql` in the
   :file:`flaskr/flaskr` folder:

このステップでは、データベーススキーマを作成します。
このアプリケーションでは必要なテーブルは1つだけで、SQLite のみサポートしています。
しなければいけないのは、 :file:`flaskr/flaskr` フォルダに、以下の内容の :file:`schema.sql` ファイルを置いて下さい。

.. sourcecode:: sql

    drop table if exists entries;
    create table entries (
      id integer primary key autoincrement,
      title text not null,
      'text' text not null
    );

.. This schema consists of a single table called ``entries``.  Each row in
   this table has an ``id``, a ``title``, and a ``text``.  The ``id`` is an
   automatically incrementing integer and a primary key, the other two are
   strings that must not be null.

このスキーマは、``entries`` というテーブルから構成されています。
のテーブルの各行は、``id``、``title``、``text`` を持ちます。
``id`` は自動的にインクリメントされる整数のプライマリーキーで、他の2つは null を許容しない文字列です。

.. Continue with :ref:`tutorial-setup`.

続いては、:ref:`tutorial-setup` 。
