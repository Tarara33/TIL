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

-  自テーブルが対象を1つ持っている(一対一)...`has_one(関連モデル名 [, scope ,オプション])`   
自分のテーブルが対象テーブルを1つ持っている(複数持たない)場合に使う。対象テーブル側に自分のidのカラムがある場合に使う。

- 自テーブルが対象に所属...`belongs_to(対象モデル名 [, scope, オプション])`   
自分のテーブルが対象テーブルのレコードに所属する(対象テーブルのidカラムがある)場合に使う。

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

