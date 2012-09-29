プラグインの作成
======================================

AsakusaSatellite はプラグインを作成することにより、
機能を拡張することができます。
以下の二種類の拡張方法があります。

メッセージフィルタ
    メッセージを変換するフィルタ機能を追加する拡張方法です。
    例えば、URL をリンクに変換することができます。
ビューフック
    チャットページの決められた場所に、HTML を追加する拡張方法です。
    例えば、各発言に好きなボタンを追加することができます。

メッセージフィルタ
--------------------------------------

フィルタプラグイン生成コマンド
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

ジェネレータが用意されています。

::

    $ rails g as_filter フィルタ名

とすることで plugins/as_フィルタ名_filter に以下に以下のように生成されます。

* init.rb: プラグインの初期設定
* lib/フィルタ名_filter.rb: フィルタプログラム
* spec/lib/フィルタ名_filter_spec.rb: フィルタのテスト

フィルタの登録
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
フィルタの登録は config/filter_infra.yml で行います。
登録されたフィルタは、上から順番に適用されます。

.. code-block:: yaml

  - name: フィルタ名_filter

任意の名前で引数を与えることもできます。

.. code-block:: yaml

  - name: フィルタ名_filter
    url: xxx

フィルタプログラム
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
フィルタプログラムは AsakusaSatellite::Filter::Base を継承したクラスを作成し、
process メソッドを実装します。
(ジェネレータで生成した場合は lib/フィルタ名_filter.rb に生成されています。) 
フィルタ設定で与えた引数はAsakusaSatellite::Filter::Base を継承したクラスでは
config 変数で保持しています。

.. code-block:: ruby

  class XxxFilter < AsakusaSatellite::Filter::Base
  
    # メッセージを処理します
    # text: メッセージの本文
    # return: フィルタしたメッセージ
    def process(text)
      %[<a target="_blank" href="#{config.url}">#{text}</a>]
    end
  
  end


ビューフック
--------------------------------------
ビューフックとは
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

ビューフックは AsakusaSatellite のビュープログラムに
あらかじめ設定されたフックポイントにプラグインからHTMLを
埋め込んで画面を拡張する仕組みです。

ビューフックを使用して画面を拡張するには AsakusaSatellite::Hook::Listener
を継承したクラスを実装します。

AsakusaSatellite::Hook::Listener を継承したクラスはRailsの起動時に読み込まれ、
自動的に登録されます。

ビューフックリスナの作成
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
plugins/ 以下にプラグインフォルダを作成します。
ここでは plugins/test_listener とします

plugins/test_listener/lib を作成し、リスナクラスを実装します。

.. code-block:: ruby

    class TestListener < AsakusaSatellite::Hook::Listener
    
      def message_buttons(context)
        if context[:message].body
          context[:message].body.size.to_s + "文字"
        else
          "0文字"
        end
      end

    end


リスナクラスでAsakusaSatelliteに設置されているフックポイント名と同名の
メソッドを実装します(上記の例ではmessage_buttons) 。引数(context)はビューフックから
渡されてくるオブジェクトを保持するハッシュが渡されます。

フックポイントの調べ方
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

フックポイントを調べるには、view の中で call_hook を呼んでいる
箇所を検索することができます。
具体的には、以下のコマンドが利用できます。

::

    $ git grep call_hook app/views

現在のバージョンでは、以下のフックポイントが有効です。

chat_room_top
    チャットルームの上部に HTML を差し込みます
chat_room_bottom
    チャットルームの下部に HTML を差し込みます
message_buttons
    各発言のボタンが表示される箇所に HTML を差し込みます

