[参考](https://qiita.com/histori/items/7b76aaaa69f2e3ab4f06)

# コントローラーの作成
~~~
$ rails g controller controller名 欲しいviewファイル
~~~
⚠️コントローラー名は複数形
***

# モデルの作成
~~~
$ rails g model model名 データベースにほしいカラム
~~~
⚠️モデル名は単数系
***

# 間違えて作成して取り消したい
rails g で作ったものは　rails destroyで消せる
~~~
$ rails destroy controller コントローラー名　 viewファイル名
$ rails destroy model モデル名(カラム名いらない)


[scaffoldとかも削除できる]
$rails destroy scaffold モデル名
~~~
***

# g 実行後の意味
- identical...同一の
- exist...存在する
- invoke...呼び出す
- conflict...競合している
- force...強制的に
- create...作成
***
