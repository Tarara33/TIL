# rails g model　の内容
- string型...文字列
- text型... 長い文字列
- integer...整数
- boolean...true or false(0 or 1)
- references...新しく作成するテーブルのカラムに、作成済みのテーブルを指定する場合に使う   
PRIMARY KEYを持つカラムを参照する（だいたいIDにつけてる）
~~~
$ rails g model post title:string body:text user:references
=> userモデルがPRIMARY KEYをつけている「id」カラムを参照する。
~~~
⚠️「references」使うカラム名は〇〇_idとしない、〇〇とテーブル名だけで、     
そのテーブルのPRIMARY KEY(主キー)がforeign_keyとして追加される。
***

## rails g 実行後
postモデルにて
~~~
[app/model/post.rb]
class Post < ApplicationRecord
  belongs_to :user
end
~~~
アソシエーション（モデルの関連づけ）のための「belongs_to」が自動的に入る。    
⚠️親側の方に「has_many」はつかないので自分で入れる。
***

マイグレーションファイルにて
~~~
class CreatePosts < ActiveRecord::Migration[5.2]
  def change
    create_table :posts do |t|
      t.string :title, null: false
      t.text :body, null: false
      t.references :user, foreign_key: true, null: false

      t.timestamps
    end
  end
end
~~~
「foreign_key（外部キー）」とは指定したカラムの自由な記述を許可せず、指定したカラムの値のみしか使えないようにする制限のこと。    
これが自動でつく。   
「null:false」は自動ではない。
***

# validates　と　validate
- validates...標準のバリデーションを書くとき
- validate...オリジナルのバリデーションを書くとき

# パスワードをもつ（ハッシュで暗号化する）model
~~~
class User < ApplicationRecord
  has_secure_password
~~~
と`has_secure_password`と入れる    
するとデータベースカラムには対応しない、「password」「password_confirmation」と言う２つの属性が追加される   
- 「password」...ユーザーが入力した生のパスワードを一時的に格納する場所    
- 「password_confirmation」...確認としてもう一度パスワードを入れてもらい先ほど入れたものと合っているか    
一時的に確認用パスワードを格納する   
***

## has_secure_passwordを使う際の注意
⚠️データベースにテーブルを作る時にパスワードを格納するカラム名は`password_digest`とする   
passwordなど他の名前にするとhas_secure_passwordが発動されなくて暗号化されずに生のままカラムに入ってしまう    
⚠️gem bcryptをダウンロードする必要ある
***

# モデルの関連づけ
モデルの「関連付け」というのは、あるモデルAのレコードを別のモデルBが参照している状態を指す。   

- 自テーブルが対象を複数持っている(一対多)...`has_many(対象モデル名 [, scope ,オプション])`をつける    
自分のテーブルが対象テーブルを複数もつ場合に使う。対象テーブル側に自分のidのカラムがある場合に使う。

-  自テーブルが対象を1つ持っている(一対一)...`has_one(対象モデル名 [, scope ,オプション])`   
自分のテーブルが対象テーブルを1つ持っている(複数持たない)場合に使う。対象テーブル側に自分のidのカラムがある場合に使う。

- 自テーブルが対象に所属...`belongs_to(対象モデル名 [, scope, オプション])`   
自分のテーブルが対象テーブルのレコードに所属する(対象テーブルのidカラムがある)場合に使う。   

- 自テーブルが第三のテーブルを介在して対象テーブルをもつ(多対多)...`has_many 介在するモデル名, through: :対象モデル名   
2つのモデルの間に「第3のモデル」（joinモデル）が介在し、   
それを経由（through）して相手のモデルの「0個以上」のインスタンスとマッチする。
[![Image from Gyazo](https://i.gyazo.com/6f13de9a372223b5066b1cba16ca5aeb.png)](https://gyazo.com/6f13de9a372223b5066b1cba16ca5aeb)   

- 自テーブルが第三のテーブルを介在して対象テーブルをもつ(一対一)...`has_one 介在するモデル名, through: :対象モデル名      
2つのモデルの間に「第3のモデル」（joinモデル）が介在し、   
それを経由（through）して相手のモデルの1個のインスタンスとマッチする。 
[![Image from Gyazo](https://i.gyazo.com/af1f6242cff3c4013c0e746cb79a005d.png)](https://gyazo.com/af1f6242cff3c4013c0e746cb79a005d)   

- 自テーブルが対象テーブルをもつ(多対多)...'has_and_belongs_to_many 対象テーブル名`    
「第3のモデル」（joinモデル）がない。   
（⚠️join用のテーブルは必要なので`rails g migrastion`でテーブルは作るが、モデルはいらない(カラムとか作らない))
[![Image from Gyazo](https://i.gyazo.com/a348c0f377f90013cc36f59a716ac0ec.png)](https://gyazo.com/a348c0f377f90013cc36f59a716ac0ec) 
***

## オプション
- dependent: :destroy
親モデルを削除する際に、その親モデルに紐づく「子モデル」も一緒に削除できる
~~~
[User model.rb]
has_many :posts

[Post model.rb]
belongs_to :user
~~~
このままだとユーザーが退会して消えてもユーザーの投稿は消えない
~~~
[User model.rb]
has_many :posts, dependent: :destroy

[Post model.rb]
belongs_to :user
~~~
これでとユーザーが退会したらユーザーの投稿も消える
***

- through: :中間テーブル名
has_manyでも、has_oneでも2つのモデルの間に「第3のモデル」（joinモデル）が介在し、   
それを経由（through）して相手のモデルの「0個以上」のインスタンスとマッチする時に使う
例えば、医師（doctor）、患者(patients)テーブルの間に予約(appointments)をとるテーブルがあるとすると
~~~
[Doctor model.rb]
has_many :appointments
has_many :patients, through: :appointments

[Patients model.rb]
has_many :appointments
has_many :doctor, through: :appointments

[Appointments model.rb]
belongs_to :patients
belongs_to :doctor
~~~
Appointmentsテーブルはdoctor_id、patients_idを含むテーブルになる
***

