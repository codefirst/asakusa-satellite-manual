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

TODO

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

