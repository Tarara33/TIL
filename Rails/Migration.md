# change_column と　add_column
## add_column
add_columnでは、バージョンを上げる際のコードからバージョンを戻す際のコードも自動生成できるためup,downメソッドで分けなくていい
~~~
[元々のデータベースにカラムを追加したいマイグレーションファイル]
class AddDetailsToTitles < ActiveRecord::Migration
  def change
    add_column :titles, :price, :integer

    add_column :titles, :author, :string
  end
end
~~~
***
## change_column
change_columnではバージョンを上げる際のコードからバージョンを戻す際のコード自動生成できない    
そのためup,downメソッドで分ける必要がある
~~~
[カラムのタイトル名を帰るマイグレーションファイル]
class AddColumnTitles < ActiveRecord::Migration
  def up
    add_column :titles, :place, :string
  end

  def down
    remove_column :titles, :place, :string
  end
end
~~~
それぞれmigrateした時（up）、rollbackした時（down）の時の動きを書いている
***

# NOT NULL制約
NULL（nil）で入れて欲しくないカラムにつける   
`データ型　：カラム名, null: false` ⚠️（true）にするとnull　OKになる    
***

# 文字数制限つける
例えばTwitterの１４０文字以内みたいな感じ    
`データ型　：カラム名, limit: 30`
***

# ユニークインデックスをつける
データベース内でデータを重複させない （タスクアプリで言えば:nameにつけると同じタイトルはつけられなくなる）
使い所大事   
⚠️NULL同士は常に異なる値と判断され重複扱いされない    
`データ型　：カラム名, unique: true` (falseだと発動しない)
 ***
  
