セットアップ
=======================
動作環境
-----------------------

* Ruby 1.8.7
* RubyGems 1.4.2 or later
* Bundler 1.0.7 or later
* Google Chrome 9 or later (クライアント)


インストール
-----------------------

::

    $ git clone git://github.com/codefirst/AsakusaSatellite.git
    $ cd AsakusaSatellite
    $ cp config/filter.yml.example config/filter.yml
    $ cp config/websocket.yml.example config/websocket.yml
    $ cp config/settings.yml.example config/settings.yml
    $ bundle install --path vendor/bundle
    $ rake groonga:migrate
    $ ruby websocket/server.rb &
    $ bundle exec rails server

Windows 以外の OS
~~~~~~~~~~~~~~~~~~~~

TODO

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

Apacheの再起動の後、http://hostname/as でアクセスできるようになります。

Windows
~~~~~~~~~~~~~~~~~~~~

TODO

対応ブラウザ
-----------------------

TODO

Google Chrome
~~~~~~~~~~~~~~~~~~~~

TODO

Safari
~~~~~~~~~~~~~~~~~~~~

TODO

Mozilla Firefox
~~~~~~~~~~~~~~~~~~~~

TODO

Opera
~~~~~~~~~~~~~~~~~~~~

TODO

