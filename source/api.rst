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
* 発言の表示

  * URL: **/api/v1/message/show**
  * メソッド: **GET**
  * パラメータ:

    * **id** (必須)  … メッセージの ID を指定します

* 発言の作成

  * URL: **/api/v1/message**
  * メソッド: **POST**
  * パラメータ:

    * **room_id** (必須)  … 発言を作成する対象となる部屋の ID を指定します
    * **message** (必須)  … 発言内容の文字列を指定します

* 発言の更新

  * URL: **/api/v1/message/update**
  * メソッド: **POST**
  * パラメータ:

    * **id** (必須)  … 更新対象の発言の ID を指定します。自分の発言のみが指定できます
    * **message** (必須)  … 更新後の発言内容の文字列を指定します

* 発言の削除

  * URL: **/api/v1/message/destory**
  * メソッド: **POST**
  * パラメータ:

    * **id** (必須)  … 削除対象の発言の ID を指定します。自分の発言のみが指定できます

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
     p http.post(uri.path + "/message",
                 "room_id=#{room_id}&message=#{message}&api_key=#{ApyKey}")
   end


