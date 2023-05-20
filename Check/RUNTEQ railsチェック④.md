# Chapter4

## マイグレーションってどういうものでどんなことが出来ますか？
開発状況に応じてデータベース内にテーブルやカラム、インデックスなどを追加/削除したり、   
既存データに修正を加えることを楽に行えるもの。
マイグレーションにおいては、１つのマイグレーションファイルが1つのバージョンとして扱われるので、    
バージョンの上げ下げを細かくできる。  
⚠️ファイルの作成だけでは変更は適応されない
~~~
$ bin/rails g migration マイグレーション名
マイグレーションファイル作成

ファイルに変更など記述した後
$ bin/rails db:migrate
=> 変更が適応される
~~~
現在のマイグレーションを確認したい時は「db/schema.rb」に書いてある
***

### マイグレーションコマンド
- `bin/rails or rails db:migrate`...最新までマイグレーションを適応する
- `bin/rails or rails db:migrate　VERSION=ファイル名の数字`...特定バージョンまでマイグレーションされた状態にする
例：「20220505120641_create_tasks.rb」だったら「20220505120641」をVERSION=に入れる
- `bin/rails or rails db:rollback`...バージョンを１つ戻す
- `bin/rails or rails db:rollback STEP=数字`...バージョンを指定したステップ数だけ戻す
- `bin/rails or rails db:migrate:redo`...バージョンを１つ戻してから上げる
（バージョンは最終的に変化しないが、バージョンを戻す処理が想定通り動くか簡単に確認できる）
***

## db/schema.rbってどういうファイルですか？
現在のデータベース構造を記入されてるファイル。   
自動出力される。
***

## モデルの「検証」(バリデーション)はどういうときに行われますか？
検証とはデータの内容が正しいかチェックすることを指す。   
railsにおけるモデルの検証とは、非常に自由度の高いチェックを行った上でユーザーにエラーをわかりやすく伝えたい時に使う。   
※データベースでも内容を空（null）で入れない（null: false）などバリデーションを定義できるが、    
種類は少ない＆エラー画面がユーザーにわかりにくい。   
ただ、モデルの検証は直接データベースに入力する時やプログラミング不備などで動かない時もあるので、    
どちらかだけではなく、モデルとデータベース両方のバリデーションを設定する
***

## モデルが「検証」に引っかかってから画面にエラーメッセージが表示される流れについて説明してください。
railsにおけるモデルの検証は基本的にモデルオブジェクトをデータベースに登録/更新する前に行い、   
エラーがあれば登録/更新をせずに差し戻す仕組みである。   
ただ、検証（validates）を設定しただけでは、エラーが出たときビューには表示されない。    
画面に出すには（例えば、フォーム画面で:nameの空欄NG,１０文字以内）
~~~
[app/models/tasks.rb]
class Task < ApplicationRecord
  validates :name, presence: true, length: {maximum: 30}
end

[app/views/tasks.html]
フォーム欄の上に
<% if tasks.errors.present? %>
  <ul id = "error_explanation">
    <% task.errors.full_messages.each do |message| %>
      <li> message </li>
  </ul>
~~~
⚠️エラーメッセージが１件とは限らないの（今回の例題の場合、空欄です。と文字数オーバーですが出る可能性ある）で、    
present?メソッドを使いeach文で取得する。（presentは全データ取得、any?は１件だけ）   
***

## セッションってどういうものですか？
ステートフルな通信を実現するための仕組み。 
保存場所：ブラウザ、サーバー    
保存期間：ユーザーがブラウザを閉じるか、サイトを離れると情報が削除される。   
無視できるか：できない
***

### ステートフル　と　ステートレスの違い
- ステートフル...以前のセッション状態を保持して、その後の処理結果に反映させる通信プロトコルやアプリケーション
- ステートレス...以前のセッション情報を保持せず、「前後の状況に関係なくその都度いつも同じレスポンスを返すようなプロトコル」

人間の会話で例え見ると、    
1回目でやりとりした相手を2回目に会ったとき、覚えていることをステートフル   
1回目でやりとりした相手を2回目に会ったとき、覚えていないことをステートレス、と例える
***

## Cookieってどういうものですか？
一度訪問したWebサイトに再度訪問するときに、IDやパスワードの情報を再入力せずログインできる仕組みのこと。    
保存場所：クライアント側のマシン（主にブラウザ）にのみ保存
保存期間：永続クッキーがあり、情報を保持し続ける  
無視できるか：できる
***

## has_secure_passwordはどういうときに使いますか？
ユーザーから入力されたパスワードをハッシュ化（暗号化）してデータベースに保存したいとき。    
モデルファイルに記述するが、それだけでは適応されない。   
①　モデル（データベースのテーブルを作るときにパスワードのカラム名は⚠️「password_digest」にする）   
②　bcrpt gem installする。    
③ has_secure_passwordの記述
~~~
[password_digestをもつモデルファイル] 例：Userモデル
class User < ApplicationRecord
  has_secure_password
end
~~~
has_secure_passwordを設定するとデータベースカラムには対応しない、    
「password」「password_confirmation」と言う２つの属性が追加される。 
- 「password」...ユーザーが入力した生のパスワードを一時的に格納する場所
- 「password_confirmation」...確認としてもう一度パスワードを入れてもらい先ほど入れたものと合っているか
一時的に確認用パスワードを格納する
***

## current_userはどういう時に定義しますか？
ヘルパーメソッドの一つで、現在ログインしているユーザーをモデルオブジェクトとして利用できる。    
例えば、ログイン中のユーザーに紐づいたcreateアクションを書くには
~~~
def create 
  task = Task.new(task_params.merge(user_id: current_user.id)
  => ユーザーから入力されるparamsにuser_idとしてcurrent_user.id(ログイン中のユーザーid)をmergeして受け取る。
 end
~~~
~~~
def create
  task = current_user.tasks.new(task_params)
  => current_user(ログイン中のユーザー)が新規作成として入力したものをtaskに代入。
end
~~~
どちらの書き方でもcurrent_user.id(ログイン中のユーザーid)をuser_idに入れて登録できる。
***

## ApplicationControllerにbefore_action :login_requiredを定義した時に解決しなければいけない問題はどのようなものがありますか？
まず、before_actionを定義すると、コントローラファイルの全てのアクションを行う前に実行される。   
今回の場合ApplicationControllerなので全てのコントローラファイルのアクション前に行われる。   
:login_requiredは、ユーザーがログイン済みかどうかチェックが入り、ログインしていない場合ログイン画面が表示する動きをする。
~~~
[ApplicationController.rb]
class ApplicationController < ActionController::Base
   before_action :login_required
   
   private
   def login_required
    redirect_to login_url unless current_user
    => unless文なのでcurrent_userがnil or falseなら実行される
   end
end
~~~
⚠️しかし、このままだと「ログイン画面を表示するアクション」にも適応され延々とredirect_toが行われる。    
そのため
~~~
[SessionsController.rb] =>ログインに関するアクションを定義してるコントローラ
class SessionsController < ApplicationController
  skip_before_action :login_required
  ・・・省略
end
~~~
として、SessionsControllerではlogin_requiredメソッドを無効にするとログイン画面が表示される
***

## UserとPostのような別々のモデルを紐付けたい時はどうすればいいですか？
モデルの「関連付け」というのは、あるモデルAのレコードを別のモデルBが参照している状態を指す。

- 自テーブルが対象を複数持っている(一対多)...has_many(対象モデル名 [, scope ,オプション])をつける
自分のテーブルが対象テーブルを複数もつ場合に使う。対象テーブル側に自分のidのカラムがある場合に使う。

- 自テーブルが対象を1つ持っている(一対一)...has_one(関連モデル名 [, scope ,オプション])
自分のテーブルが対象テーブルを1つ持っている(複数持たない)場合に使う。対象テーブル側に自分のidのカラムがある場合に使う。

- 自テーブルが対象に所属...belongs_to(対象モデル名 [, scope, オプション])
自分のテーブルが対象テーブルのレコードに所属する(対象テーブルのidカラムがある)場合に使う。
***

### オプション
- dependent: :destroy 親モデルを削除する際に、その親モデルに紐づく「子モデル」も一緒に削除できる
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

- through: :中間テーブル名 has_manyでも、has_oneでも2つのモデルの間に「第3のモデル」（joinモデル）が介在し、
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
