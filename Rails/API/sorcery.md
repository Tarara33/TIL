# APIを使った[sorcery](https://github.com/Tarara33/TIL/tree/main/Rails/Gem/sorcery)の実装
APIを使った sorcery実装の場合、通常の sorceryの実装と少し変わることがある。  
例えば、 sorceryをインストールしただけでは current_userメソッドが使えない。など。
***

## ❓ sorceryを使う開発の APIが使うと何がいい？？
Sorceryを使った開発で APIがある場合とない場合の違いは、  
主にユーザーの状態（ログインしてるかどうか）をどう管理するかにある。  

**【APIを使わない場合】**    
Sorceryの loginメソッドでログイン処理を行って、セッションでユーザーの状態を保持する。  
これは Webページをリロードしてもユーザーがログインしたままでいられるから便利だよ。  

**【APIを使う場合】**   
フロントエンドとバックエンドが分かれてるから、セッションを使って状態を保持するのが難しい。    
だからトークンを発行して、そのトークンを使ってユーザーが誰かを識別する方法を取ることが多い。  
確かにトークンを発行したり管理したりするのは少し面倒だけど、これによって色んなデバイスから安全にアクセスできるようになる。  
また、APIを使ったアプリの方がスケールしやすいとか、フロントエンドを自由に作れるといったメリットもある。
***

# トークンの発行
ログイン時 / 新規登録時のレスポンスに access_tokenを付与することで APIでも current_userを使えるようにする。

## テーブルを作る
Userテーブルとは別にトークン用のテーブルを作る。  
今回は「ApiKeyテーブル」とする。
~~~
$ rails g model ApiKey user:references access_token:string expires_at:datetime
~~~
`expires_at`は有効期限を表してる。

⭐️ トークンは一意性のあるものにしたいので、マイグレーションファイルにバリデーション追加する。
~~~
class CreateApiKeys < ActiveRecord::Migration[6.0]
  def change
    create_table :api_keys do |t|
      t.references :user, null: false, foreign_key: true
      t.string :access_token, null: false
      t.datetime :expires_at

      t.timestamps
    end
  ⭐️add_index :api_keys, :access_token, unique: true
  end
end
~~~
***

## テーブル設定
- トークンは一意性のあるものにする。
- 有効期限は一週間にする。  
もしトークン発行済み & 有効期限内ならすでにあるトークンを使い、持ってない or 期限切れなら新しく発行する。
~~~
[app/models/api_key.rb]

class ApiKey < ApplicationRecord
  belongs_to :user

  validates :access_token, presence: true, uniqueness: true

  🩵DEFAULT_EXPIRES_WEEK = 1.week

  💙scope :active, -> { where('expires_at >= ?', Time.current) }

  💛def initialize(attributes = {})
    ❓super
    self.access_token = SecureRandom.uuid
    self.expires_at = DEFAULT_EXPIRES_WEEK.after
  end
end
~~~
💙 スコープの内容は、expires_atが現在時刻より未来のものを取得する。

💛 [initialize](https://github.com/Tarara33/TIL/blob/main/Ruby/%E3%82%AF%E3%83%A9%E3%82%B9/%E3%83%A1%E3%83%A2/initialize.md)メソッドは、インスタンス作成時に実行される。  
- self = ApiKeyクラスのこと。  
- `SecureRandom.uuid`は SecureRandomモジュールのメソッドで暗号学的に安全なランダムなデータを生成する。
- 「一週間」をわざわざ🩵変数に入れてるのは、有効期限をやっぱり二ヶ月とかに変更した時に修正しやすいから。
***

## ❓ superはなんのため？？必要??
まず [super](https://github.com/Tarara33/TIL/blob/main/Ruby/%E3%82%AF%E3%83%A9%E3%82%B9/%E7%B6%99%E6%89%BF.md)の役割は、サブクラスのメソッドから同名の親クラスのメソッドを呼び出すこと。  
つまりここでは、`class ApiKey < ApplicationRecord`だから、ApplicationRecordが親クラス。

superがなかった場合ApplicationRecordの initializeメソッド（結局のところ ActiveRecord::Baseの initialize）が呼び出されなくなる。  
これは、attributesハッシュを使って新しいレコードの属性を設定する Railsのデフォルトの挙動をスキップすることを意味している。

具体的には、新しく ApiKeyのインスタンスを作成するときに渡される属性が無視され、アクセストークンと有効期限のみがセットされることになる。  
もし渡された属性に user関連の情報など他の重要なデータがあっても、それらは無視されてしまう。  
つまり今回の場合、外部キー設定している user_idの情報が無視されてしまうので、  
トークンや有効期限はできたけど、どのユーザーのものかわからない状態になる。

⭐️ だから、通常は superを使って親クラスの initializeメソッドを呼び出すことが重要!
***

## Userモデル編集
~~~
[app/models/user.rb]

class User < ApplicationRecord
  authenticates_with_sorcery!
  has_many :api_keys, dependent: :destroy

  def activate_api_key!
    return 💚api_keys.active.fast if 🧡api_keys.active.exists?

    api_keys.create
  end
end
~~~
🧡 もし、api_keysの有効期限が有効なインスタンスが存在([exists](https://github.com/Tarara33/TIL/blob/main/Ruby/DB%E9%96%A2%E4%BF%82%E3%83%A1%E3%82%BD%E3%83%83%E3%83%89.md))したら  

💚 スコープ api_keysの有効期限が有効なインスタンスの最初のものを返す。

そうでなければ、api_keyインスタンスを作る。
***

