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
`データ型　：カラム名, null: false` （true）にするとnull　OKになる        
⚠️このままだと「""」空文字は入ってしまうためmodelでvalidateする必要ある
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

# execute
 マイグレーションファイルでSQLを実行    
 `execute(SQL文, 名前=nil)` 例`execute "SELECT * FROM pages ORDER BY updated_at DESC LIMIT 10"`
 ***
# add_reference
  指定したテーブルにリファレンスを追加    
  `add_reference(テーブル名, リファレンス名, オプション引数)`    
  例`add_reference :tasks, :user, null:false, index:true`
  ***
# remove_reference
  既存のテーブルのリファレンスを削除   
  `remove_reference(テーブル名, リファレンス名 [, オプション])`
  ***
  
# モデルの関連づけ
モデルの「関連付け」というのは、あるモデルAのレコードを別のモデルBが参照している状態を指す。   

- 自テーブルが対象を複数持っている(一対多)...`has_many(対象モデル名 [, scope ,オプション])`をつける    
自分のテーブルが対象テーブルを複数もつ場合に使う。対象テーブル側に自分のidのカラムがある場合に使う。

- 自テーブルが対象に所属...`belongs_to(対象モデル名 [, scope, オプション])`   
自分のテーブルが対象テーブルのレコードに所属する(対象テーブルのidカラムがある)場合に使う。
