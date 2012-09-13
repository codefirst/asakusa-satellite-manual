API
=======================

本章では、AsakusaSatellite が提供している API について説明します。
外部のアプリケーションから、AsakusaSatellite に
発言を投稿したり、発言を取得したりすることができます。

API キー
-----------------------

API キーは、API を利用するために使用する認証用のキーです。
キーを利用することで、AsakusaSatellite は
ユーザを認証することができます。
そのため、API キーは他人に渡してはいけません。

API キーを取得するには、
画面右上のログインユーザ名のリンクをクリックし、
個人情報画面を開きます。

.. image:: images/api_key.png

画面に表示されているランダムな文字列が、
ログインユーザを認証するための API キーです。

**Generate** ボタンをクリックすると再生成します。

RESTful API
-----------------------

メソッドについて
^^^^^^^^^^^^^^^^^^^^^^^

PUT 及び DELETE に対応していないライブラリでは、
パラメータに **_method=put** または **_method=delete** を
付加して POST することで同じ動作をさせることができます。

発言の操作 API
^^^^^^^^^^^^^^^^^^^^^^^

* 発言の表示

  * URL: **/api/v1/message/id.format**

    * **id** (必須)  … メッセージの ID を指定します
    * **format** (必須)  … 指定した形式で結果を返します。現在は **json** のみが有効です。

  * メソッド: **GET**
  * パラメータ:

    * **api_key** (private room の場合必須) … ユーザの API キーを指定します。private room のメッセージの取得は 0.8.0 以降の対応となります。

* 発言の作成

  * URL: **/api/v1/message.format**

    * **format** (必須)  … 指定した形式で結果を返します。現在は **json** のみが有効です。

  * メソッド: **POST**
  * パラメータ:

    * **room_id** (必須)  … 発言を作成する対象となる部屋の ID を指定します
    * **message** (必須)  … 発言内容の文字列を指定します
    * **api_key** (必須)  … ユーザの API キーを指定します

* 発言の更新

  * URL: **/api/v1/message/id.format**

    * **id** (必須)  … 更新対象の発言の ID を指定します。自分の発言のみが指定できます
    * **format** (必須)  … 指定した形式で結果を返します。現在は **json** のみが有効です。

  * メソッド: **PUT**
  * パラメータ:

    * **message** (必須)  … 更新後の発言内容の文字列を指定します
    * **api_key** (必須)  … ユーザの API キーを指定します

* 発言の削除

  * URL: **/api/v1/message/id.format**

    * **id** (必須)  … 削除対象の発言の ID を指定します。自分の発言のみが指定できます
    * **format** (必須)  … 指定した形式で結果を返します。現在は **json** のみが有効です。

  * メソッド: **DELETE**
  * パラメータ:

    * **api_key** (必須)  … ユーザの API キーを指定します

* 部屋の発言の取得

  * URL: **/api/v1/message/list.format**

    * **room_id** (必須)  … 部屋の ID を指定します。
    * **format** (必須)  … 指定した形式で結果を返します。現在は **json** のみが有効です。

  * メソッド: **GET**
  * パラメータ:

    * **until_id** (任意)  … 発言を取得する基準となる ID を指定します。この ID よりも以前の発言が取得されます。指定しなければ最新の発言を取得します
    * **since_id** (任意)  … 発言を取得する基準となる ID を指定します。この IDより後の発言が取得されます。指定しなければ最新の発言を取得します。 **until_id** とどちらか片方しか指定できません
    * **count** (任意) … 取得する発言の個数を指定します。指定しなければ、20個の発言を取得します
    * **api_key** (private room の場合必須) … ユーザの API キーを指定します。

部屋の操作 API
^^^^^^^^^^^^^^^^^^^^^^^

* 部屋の一覧を取得

  * URL: **/api/v1/room/list.format**

    * **format** (必須)  … 指定した形式で結果を返します。現在は **json** のみが有効です。

  * メソッド: **GET**
  * パラメータ:

    * なし

* 部屋の作成

  * URL: **/api/v1/room.format**

    * **format** (必須)  … 指定した形式で結果を返します。現在は **json** のみが有効です。

  * メソッド: **POST**
  * パラメータ:

    * **name** (必須)  … 作成する部屋の名前を指定します
    * **api_key** (必須)  … ユーザの API キーを指定します

* 部屋の更新

  * URL: **/api/v1/room/id.format**

    * **id** (必須)  … 名称を変更する部屋の ID を指定します
    * **format** (必須)  … 指定した形式で結果を返します。現在は **json** のみが有効です。

  * メソッド: **PUT**
  * パラメータ:

    * **name** (必須)  … 変更後の部屋の名前を指定します
    * **api_key** (必須)  … ユーザの API キーを指定します

* 部屋の削除

  * URL: **/api/v1/room/id.format**

    * **id** (必須)  … 削除する部屋の ID を指定します
    * **format** (必須)  … 指定した形式で結果を返します。現在は **json** のみが有効です。

  * メソッド: **DELETE**
  * パラメータ:

    * **api_key** (必須)  … ユーザの API キーを指定します

ユーザの操作 API
^^^^^^^^^^^^^^^^^^^^^^^

* ログインユーザ情報の取得

  * URL: **/api/v1/user.format**

    * **format** (必須)  … 指定した形式で結果を返します。現在は **json** のみが有効です。

  * メソッド: **GET**
  * パラメータ:

    * **api_key** (必須)  … ユーザの API キーを指定します

WebSocket API
-----------------------

WebSocket サーバに接続することで、
発言の追加や変更、削除のタイミングを知ることが出来ます。
接続は WebSocket のプロトコルに対応したライブラリを利用してください。

* WebSocket サーバへの接続

  * URL: **ws://hostname:port/room**

    * **hostname** … WebSocket のサーバのアドレスを指定します
    * **port** … WebSocket のサーバのポートを指定します。config/websocket.yml の **websocketPort** で設定します。デフォルトは **18081** です。

  * パラメータ:

    * **id** (必須) … 部屋の ID を指定します。

* 送信されるデータ

  * 発言の作成

    * { "event" : 'create', "content": "<message の JSON 形式>" }

  * 発言の変更

    * { "event" : 'update', "content": "<message の JSON 形式>" }

  * 発言の削除

    * { "event" : 'delete', "content" : { "id": "<message の ID>" } }

bot の作成例
-----------------------

以下は、部屋番号と発言をコマンドラインオプションで指定して
発言を行うプログラムの例です。

.. code-block:: ruby

   #! /user/bin/env ruby
   # -*- mode:ruby; coding:utf-8 -*-

   # ------------------------------
   # example for bot
   # ------------------------------

   # Get from http://$AS_ROOT/account/index
   ApiKey   = "YOUR_API_KEY"

   # EntryPoint
   EntryPoint = "http://localhost:3000/api/v1"

   # ------------------------------
   require 'net/http'

   if ARGV.size != 2 then
     puts "#{$0} <room_id> <message>"
     exit 0
   end

   room_id, message = *ARGV
   uri = URI(EntryPoint)

   Net::HTTP.start(uri.host, uri.port) do| http |
     # post message
     p http.post(uri.path + "/message.json",
                 "room_id=#{room_id}&message=#{message}&api_key=#{ApyKey}")
   end


