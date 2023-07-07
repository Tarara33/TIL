# 機能
sorceryに追加できるサブモジュールの１つ。    
ユーザーがパスワードを忘れた時などにリセットして再設定できるようにする。
***

# 導入
~~~
$ rails g sorcery:install reset_password(サブモジュール名) --only-submodules
$ rails db:migrate
~~~
***

### すでに「rails g sorcery:install」　使ったけどいいの？？
復習：「rails g sorcery:install」するとログイン認証に必要なカラムを持ったUserモデルができる。    
今回も同じコマンド含まれているがいいのか？　結論としてはOK。      
    
「--only-submodules」オプションを使用することで、Sorceryの他のサブモジュール（機能）はインストールされず、    
リセットパスワードモジュールに関連するファイルや設定のみが生成される。    
したがって、既にログイン機能などを含むSorceryのインストールが完了している場合でも、    
このコマンドを実行することでリセットパスワード機能を追加することができる。
***

## 新しくできたカラムの意味
~~~
[マイグレーションファイル]
class SorceryResetPassword < ActiveRecord::Migration[5.2]
  def change
    ①add_column :users, :reset_password_token, :string, default: nil
    ②add_column :users, :reset_password_token_expires_at, :datetime, default: nil
    ③add_column :users, :reset_password_email_sent_at, :datetime, default: nil
    ④add_column :users, :access_count_to_reset_password_page, :integer, default: 0

    add_index :users, :reset_password_token
  end
end
~~~
①トークン文字列を保持するカラム      
②ークンの有効期限が切れる時刻を記録するカラム        
③パスワードリセット用のメールを送信した時刻を保持するためのカラム        
④パスワードリセット画面にアクセスした回数を記録するために用意されたカラム        
N回以上はパスワードリセット画面に移動できないようにするなどの機能を実装する時に用いられるカラム。
***

# Userモデルのバリデーション
トークンにユニーク制約をかける。
~~~
[app/models/user.rb]
class User < ApplicationRecord
    authenticates_with_sorcery!
    validates :reset_password_token, uniqueness: true, allow_nil: true
end
~~~
***

