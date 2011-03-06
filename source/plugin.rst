プラグイン
=======================
Redmine 連携
-----------------------
機能
^^^^^^^^^^^^^^^^^^^^^^^
#数字 をRedmineのチケットへのリンクに変換します。
また、メッセージをRedmine投稿するためのリンクを付加します。

設定
^^^^^^^^^^^^^^^^^^^^^^^
config/filter.yml に以下を記述します

.. code-block:: ruby

  - name: redmine_ticket_link
    roots: RedmineのルートURL


コードハイライト
-----------------------
機能
^^^^^^^^^^^^^^^^^^^^^^^
ソースコードをハイライトします。
記法は 

* １行目に 言語:: 
* ２行目以降にソースコードを記述します。

例えば,

.. code-block:: ruby

  ruby::
  puts "Hello World!"

と記述することで、２行目がハイライトして表示されます。


設定
^^^^^^^^^^^^^^^^^^^^^^^
config/filter.yml に以下を記述します

.. code-block:: ruby

  - name: code_highlight_filter

汎用リンク
-----------------------
機能
^^^^^^^^^^^^^^^^^^^^^^^
http:// https:// で始まるURLをリンクに変換します。また、
以下のサイトは画像として展開します。

* twitpic.com
* f.hatena.ne.jp
* movapic.com
* yflog.com
* ow.ly
* youtu.be
* img.ly
* plixi.com

設定
^^^^^^^^^^^^^^^^^^^^^^^
config/filter.yml に以下を記述します

.. code-block:: ruby

  - name: auto_link


Twitter リンク
-----------------------
機能
^^^^^^^^^^^^^^^^^^^^^^^
メッセージ中の @username をTwitterアカウントへのリンクに変換します。

設定
^^^^^^^^^^^^^^^^^^^^^^^
config/filter.yml に以下を記述します

.. code-block:: ruby

  - name: twitter_link
