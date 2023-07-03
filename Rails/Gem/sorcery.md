# 機能
認証機能を実装できる。    
ログイン機能を実装するのに便利なメソッドなど使える。
***

# 使いかた
## Gem導入
~~~
[gemfile]
gem 'sorcery'
$ bundle
~~~
***

## 使用方法
~~~
$ rails g sorcery:install
$ rails db:migrate
~~~
~~~
[db/schema.rb]
ActiveRecord::Schema.define(version: 2023_07_02_132620) do

  create_table "users", force: :cascade do |t|
    t.string "email", null: false
    t.string "crypted_password"
    t.string "salt"
    t.string "name", null: false
    t.datetime "created_at", null: false
    t.datetime "updated_at", null: false
    t.index ["email"], name: "index_users_on_email", unique: true
  end

end
~~~
💡これでUserモデルができるので「rails g model user...」としなくていい。    
ちなみに"User"というモデル名はオプションで変更することもできる。
***

### salt と crypted_password
salt...ハッシュ関数によってパスワードをハッシュ化する際に使用されるランダムなデータ。    
crypted_password...ユーザーのパスワードを暗号化して保存するためのフィールド。      
⚠️これらの用語は古い実装方法であり、     
現代のセキュリティ標準ではより強力なハッシュ関数とソルトの使い方が推奨されている。    
[has_secure_passwordなど](https://github.com/Tarara33/TIL/blob/main/Rails/Model/%E3%83%A1%E3%83%A2.md)
***



