セットアップ
=======================
動作環境
-----------------------
以下のソフトウェアのインストールが必要です。

* Ruby 1.8.7
* RubyGems 1.4.2 or later
* Bundler 1.0.7 or later

インストール
-----------------------

以下では、3つの場合のインストール方法について説明します。

* Windows 以外の OS
* Passenger
* Windows

Windows 以外の OS
~~~~~~~~~~~~~~~~~~~~

ダウンロードリンク_ から最新版をダウンロードし、適当なディレクトリに展開してください。
展開したディレクトリを AsakusaSatellite にリネームし、以下のコマンドを実行してください。

.. _ダウンロードリンク: http://github.com/codefirst/AsakusaSatellite/archives/master

::

    $ cd AsakusaSatellite
    $ cp config/filter.yml.example config/filter.yml
    $ cp config/websocket.yml.example config/websocket.yml
    $ cp config/settings.yml.example config/settings.yml
    $ bundle install --path vendor/bundle
    $ bundle exec rake groonga:migrate
    $ ruby websocket/server.rb &
    $ bundle exec rails server

注意事項

* unsupported column エラーが発生する場合がありますが、問題ないので無視してください。
* MeCab がインストールされている環境では、正しく動作しない場合があります。 vendor/bundle 以下に groonga をインストールし直してください。

Passenger
~~~~~~~~~~~~~~~~~~~~
gem から passenger をインストールします。
::

  $ gem install passenger

Apacheモジュールをビルドします。
::

  $ passenger-install-apache2-module

Apacheの設定ファイルへの記述が表示されるのでメモを取ります。

必要なパッケージのインストールを促された場合、
指示にしたがってインストールの後再実行してください。

例)
::

  LoadModule passenger_module /usr/lib/ruby/gems/1.8/gems/passenger-2.2.11/ext/apache2/mod_passenger.so
  PassengerRoot /usr/lib/ruby/gems/1.8/gems/passenger-2.2.11
  PassengerRuby /usr/bin/ruby1.8

Apache への設定に以下を記述します。
::

  RailsEnv production
  RailsBaseURI /as

DocumentRoot に AsakusaSatellite の public ディレクトリへのシンボリックリンクを作成します。

例)

* DocumentRoot が /var/www
* AsakusaSatellite が /var/AsakusaSatellite

にインストールされている場合

::

  $ cd /var/www
  $ sudo ln -s /var/AsakusaSatellite/public as

AsakusaSatellite の WebSocket の設定を変更します。
config/websocket.yml の "roots" の値を環境に応じて編集します。

::

  roots: localhost/as/

Apacheの再起動の後、http://hostname/as でアクセスできるようになります。

Windows
~~~~~~~~~~~~~~~~~~~~

以下の Ruby での動作を確認しています。

* Ruby 1.8.7 (http://rubyinstaller.org/downloads/)

また、以下のアドオンのインストールが必要です。

* DevKit (http://rubyinstaller.org/downloads/)

それ以外は、Windows 以外の OS の場合と同じです。

.. _browser:

対応ブラウザ
-----------------------

AsakusaSatellite は以下のブラウザをサポートしています。

* Google Chrome

また、制限付きで以下のブラウザをサポートしています。

* Safari
* Mozilla Firefox
* Opera

Google Chrome
~~~~~~~~~~~~~~~~~~~~

すべての機能をご利用可能です。

Safari
~~~~~~~~~~~~~~~~~~~~

以下の機能がご利用いただけません。

* デスクトップ通知

Mozilla Firefox
~~~~~~~~~~~~~~~~~~~~

バージョン 4 からのサポートです。
WebSocket を有効にするために、以下の設定を行ってください。

1. アドレスバーに "about\:config" と入力します。
2. network.websocket.override-security-block の値を "true" に変更します。

以下の機能がご利用いただけません。

* デスクトップ通知

Opera
~~~~~~~~~~~~~~~~~~~~

バージョン 11 からのサポートです。
WebSocket を有効にするために、以下の設定を行ってください。

1. アドレスバーに "about\:config" と入力します。
2. "User Prefs" の "Enable WebSockets" をチェックします。
3. "保存" をクリックします。

以下の機能がご利用いただけません。

* デスクトップ通知
* ファイルアップロード

