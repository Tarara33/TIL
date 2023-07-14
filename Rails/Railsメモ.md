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

# デバック
rails では主に    
- byebug    
- binding.irb    
- binding.pry    
がある。
***
デバックがサーバー側なのか、ブラウザ側なのかで書き方変わる。
- サーバー...そのままかく。
- ブラウザ...`<% %>`で囲む
***
