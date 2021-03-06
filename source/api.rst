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

    * **room_id** (必須)  … 発言を作成する対象となる部屋の ID もしくはニックネームを指定します
    * **message** (必須)  … 発言内容の文字列を指定します
    * **files** (任意)  … 添付するファイルのパスを指定します
    * **api_key** (必須)  … ユーザの API キーを指定します

   (例)

   .. code-block:: ruby
   
      # Usage:
      # ruby ./uploader.rb http://hoge.com/api/v1/message.json hoge.jpg
      #
      require 'rest-client'
      
      url   = ARGV.shift
      files = ARGV
      
      RestClient.log = 'stdout'
      data = {}
      data["api_key"] = "(API Key)"
      data["room_id"] = "(Room ID)"
      data["message"] = "message"
      files.each do |file|
        data["files[#{file}]"] = File.new(file, 'rb')
      end
      RestClient.post(url, data)

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

    * **until_id** (任意)  … 発言を取得する基準となる ID を指定します。
      この ID よりも以前の発言が取得されます。指定しなければ最新の発言を取得します。
      **older_than** と同時には指定できません。
    * **older_than** (任意)  … 発言を取得する基準となる ID を指定します。
      この ID よりも古い発言が取得されます。指定しなければ最新の発言を取得します。
      **until_id** と同時には指定できません。
    * **since_id** (任意)  … 発言を取得する基準となる ID を指定します。
      この ID より以降の発言が取得されます。指定しなければ最新の発言を取得します。
      **newer_than** と同時には指定できません。
    * **newer_than** (任意)  … 発言を取得する基準となる ID を指定します。
      この ID よりも新しい発言が取得されます。指定しなければ最新の発言を取得します。
      **since_id** と同時には指定できません。
    * **order** (任意) … メッセージのソート順序を指定します。
      **asc** を指定した場合には作成日時の昇順、 **desc** を指定した場合には作成日時の降順になります。
    * **count** (任意) … 取得する発言の個数を指定します。指定しなければ、20個の発言を取得します。
    * **api_key** (private room の場合必須) … ユーザの API キーを指定します。

部屋の操作 API
^^^^^^^^^^^^^^^^^^^^^^^

* 部屋の一覧を取得

  * URL: **/api/v1/room/list.format**

    * **format** (必須)  … 指定した形式で結果を返します。現在は **json** のみが有効です。

  * メソッド: **GET**
  * パラメータ:

    * **api_key** (任意) … 指定した場合、 private room も含めて一覧します。

* 部屋の作成

  * URL: **/api/v1/room.format**

    * **format** (必須)  … 指定した形式で結果を返します。現在は **json** のみが有効です。

  * メソッド: **POST**
  * パラメータ:

    * **name** (必須)  … 作成する部屋の名前を指定します
    * **api_key** (必須)  … ユーザの API キーを指定します

* 部屋の更新

  * URL: **/api/v1/room/id.format**

    * **id** (必須)  … 名称を変更する部屋の ID またはニックネームを指定します
    * **format** (必須)  … 指定した形式で結果を返します。現在は **json** のみが有効です。

  * メソッド: **PUT**
  * パラメータ:

    * **name** (必須)  … 変更後の部屋の名前を指定します
    * **api_key** (必須)  … ユーザの API キーを指定します

* 部屋の削除

  * URL: **/api/v1/room/id.format**

    * **id** (必須)  … 削除する部屋の ID またはニックネームを指定します
    * **format** (必須)  … 指定した形式で結果を返します。現在は **json** のみが有効です。

  * メソッド: **DELETE**
  * パラメータ:

    * **api_key** (必須)  … ユーザの API キーを指定します

* private room へのメンバの追加

  * URL: **/api/v1/room/add_member.format**

    * **format** (必須)  … 指定した形式で結果を返します。現在は **json** のみが有効です。

  * メソッド: **POST**
  * パラメータ:

    * **id** (必須)  … メンバを追加する部屋の ID またはニックネームを指定します
    * **api_key** (必須)  … ユーザの API キーを指定します
    * **user_id** (必須)  … 追加するユーザのIDを指定します。

ユーザの操作 API
^^^^^^^^^^^^^^^^^^^^^^^

* ログインユーザ情報の取得

  * URL: **/api/v1/user.format**

    * **format** (必須)  … 指定した形式で結果を返します。現在は **json** のみが有効です。

  * メソッド: **GET**
  * パラメータ:

    * **api_key** (必須)  … ユーザの API キーを指定します

AsakusaSatelliteの操作 API (0.8.1以降)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* 設定情報の取得。 現在は :doc:`websocket` に関する設定のみが取得できます。

  * URL: **/api/v1/service/info.format**

    * **format** (必須)  … 指定した形式で結果を返します。現在は **json** のみが有効です。

  * メソッド: **GET**
  * パラメータ: なし

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
                 "room_id=#{room_id}&message=#{message}&api_key=#{ApiKey}")
   end


