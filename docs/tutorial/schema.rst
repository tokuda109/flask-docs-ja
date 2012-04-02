.. _tutorial-schema:

ステップ1: データベース スキーマ
=========================================

.. Step 1: Database Schema
   =======================

.. First we want to create the database schema.  For this application only a
   single table is needed and we only want to support SQLite so that is quite
   easy.  Just put the following contents into a file named `schema.sql` in
   the just created `flaskr` folder:

最初にデータベーススキーマ作成します。このアプリケーションでは必要なテーブルは一つだけで、簡単に使えるのでSQLiteを使います。
`flaskr` フォルダ内の `schema.sql` という名前のファイルに、以下のコンテンツを入れて下さい。 :

.. sourcecode:: sql

    drop table if exists entries;
    create table entries (
      id integer primary key autoincrement,
      title string not null,
      text string not null
    );

.. This schema consists of a single table called `entries` and each row in
   this table has an `id`, a `title` and a `text`.  The `id` is an
   automatically incrementing integer and a primary key, the other two are
   strings that must not be null.

このスキーマは、 `entries` と呼ばれる一つのテーブルからなり、このテーブルの個々の行には `id` 、 `title` 、 `text` があります。
`id` は主キーで自動的にインクリメントし、他の二つは空白が認められていない文字列です。

.. Continue with :ref:`tutorial-setup`.

続いては :ref:`tutorial-setup` 。
