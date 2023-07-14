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

# g 実行後の意味
- identical...同一の
- exist...存在する
- invoke...呼び出す
- conflict...競合している
- force...強制的に
- create...作成
***

# 間違えて作成して取り消したい
rails g で作ったものは　rails destroyで消せる。    
[参考](https://qiita.com/histori/items/7b76aaaa69f2e3ab4f06)
~~~
$ rails destroy controller コントローラー名　 viewファイル名
$ rails destroy model モデル名(カラム名いらない)


[scaffoldとかも削除できる]
$rails destroy scaffold モデル名
~~~
***

# Scaffold 
モデル（データベーステーブル）を含めたファイルを作成してくれる。     
CRUD(Create, Read, Update, Delete)を備えてくれる。    
大きく９つに分けると    
- マイグレーションファイル
- モデルファイル
- コントローラファイル
- ビューファイル（index, edit, show, new, _form）
- ヘルパーファイル
- SASSファイル
- jbuilderファイル
- ルーティングファイル
- テスト用ファイル
~~~
$ rails g scaffold モデル名 <カラム名1:型1> <カラム名2:型2>...

=> モデルを生成する、rails g modelの「model」が「scaffold」に置き換わっただけ
~~~
***

## scaffold_controller 
アプリケーションの基本的な機能の一覧(index)、詳細(show)、新規作成(new/create)、編集(edit/update)、削除(destroy)   
するために必要なコントローラやビューをまとめて生成。    
scaffoldとの違いはモデルやアセットが作られないこと。
~~~
$ rails generate scaffold_controller コントローラ名 <カラム名:型>...
~~~
***
