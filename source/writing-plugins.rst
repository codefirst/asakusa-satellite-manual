プラグインの作成
======================================
メッセージフィルタプラグイン
--------------------------------------

フィルタプラグイン生成コマンド
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

ジェネレータが用意されています。

::

    $ rails g as_filter フィルタ名

とすることで vendor/plugins/as_フィルタ名_filter に以下に以下のように生成されます。

* init.rb: プラグインの初期設定
* lib/フィルタ名_filter.rb: フィルタプログラム
* spec/lib/フィルタ名_filter_spec.rb: フィルタのテスト

フィルタの登録
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
フィルタの登録は config/filter.yml で行います。

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
 :linenos:

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
vendor/plugins/ 以下にプラグインフォルダを作成します。
ここでは vendor/plugins/test_listener とします

vendor/plugins/test_listener/lib を作成し、リスナクラスを実装します。

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



