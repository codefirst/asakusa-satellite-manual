本体の設定
=======================

`config/settings.yml` においてAsakusaSatellite本体の挙動を変更できます。

認証
-----------------------

**omniauth**
    認証方法を指定します。 詳しくは :ref:`auth` を参照してください。

ファイル添付
-----------------------

**attachment_policy**
    添付ファイルの保存方法を指定します。 詳しくは :ref:`watage` を参照してください。
**attachment_path**
    添付ファイルの保存先を指定します。
**attachment_max_size**
    添付ファイルの上限サイズをバイト単位で指定します。 指定しない場合や0を指定した場合は無制限になります。
**watage_xxxx**
    watageプログインの設定を指定します。 詳しくは :ref:`watage` を参照してください。
