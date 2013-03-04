Heroku へのデプロイ
=============================
AsakusaSatellite は Heroku (Cedar Stack) 上で動作させることができます。
以下、Heroku へのデプロイの手順について説明します。

なお、手順では以下を前提とします。

* heroku gem および git がインストールされていること
* `Heroku <http://www.heroku.com/>`_ のアカウントを持っていること

1. `Githubのページ <https://github.com/codefirst/AsakusaSatellite>`_ から最新の ZIP をダウンロードし展開します。

2. AsakusaSatellite を git の管理下に置きます。

AsakusaSatellite のトップディレクトリにて以下のコマンドを発行します。

::

    $ git init
    $ git add .
    $ git commit -a -m "heroku"

3. Heroku にアプリケーションを作成します。

AsakusaSatellite のトップディレクトリにて以下のコマンドを発行します。

::

    $ heroku create
    Creating nameless-sands-3102... done, stack is cedar
    http://nameless-sands-3102.herokuapp.com/ | git@heroku.com:nameless-sands-3102.git
    Git remote heroku added

4. MongoDB Add-on を追加します。

::

    $ heroku addons:add mongohq:sandbox
    ----> Adding mongohq:sandbox to nameless-sands-3102... done, v2 (free)

5. MongoDB の設定をします。

追加された MongoDB の設定を確認します。

::

    $ heroku config | grep MONGOHQ_URL
    MONGOHQ_URL => mongodb://heroku:e027fb5a44f9bb34beeb7318f0f1bd7f@staff.mongohq.com:10025/app8004619

確認した設定を元に、以下の環境変数を設定します。

 * MONGODB_HOST
 * MONGODB_PORT
 * MONGODB_DATABASE
 * MONGODB_USERNAME
 * MONGODB_PASSWORD

::

    $ heroku config:add MONGODB_HOST=staff.mongohq.com MONGODB_PORT=10025 MONGODB_DATABASE=app8004619 MONGODB_USERNAME=heroku MONGODB_PASSWORD=e027fb5a44f9bb34beeb7318f0f1bd7f

6. Pusher Add-on を有効にします。

::

    $ heroku addons:add pusher:sandbox
    ----> Adding pusher:sandbox to nameless-sands-3102... done, v4 (free)

7. Pusher の設定をします。

追加された Pusher の設定を確認します。

::

    $ heroku config | grep PUSHER_URL
    PUSHER_URL => http://b217da9d38540ccd56fd:a3039bb476ae56d2b631@api.pusherapp.com/apps/28676

以下の環境変数を設定します。

 * MESSAGE_PUSHER_ENGINE
 * MESSAGE_PUSHER_PUSHER_APP_ID
 * MESSAGE_PUSHER_PUSHER_KEY
 * MESSAGE_PUSHER_PUSHER_SECRET

::

    $ heroku config:add MESSAGE_PUSHER_ENGINE=pusher MESSAGE_PUSHER_PUSHER_APP_ID=28676 MESSAGE_PUSHER_PUSHER_KEY=b217da9d38540ccd56fd MESSAGE_PUSHER_PUSHER_SECRET=a3039bb476ae56d2b631

8. Heroku へデプロイします。

::

    $ git push heroku master


9. デプロイが完了したアプリケーションにアクセスします。

::

    $ heroku open

