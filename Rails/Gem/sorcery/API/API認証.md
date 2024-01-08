# sorceryを使っての API認証
【前提】  
- gem sorceryを導入済み。
- エラーハンドリング作成済み。([こちら](https://github.com/Tarara33/TIL/blob/main/Rails/Concerns/%E5%AE%9F%E8%A3%85%E4%BE%8B/%E3%82%A8%E3%83%A9%E3%83%BC%E3%83%8F%E3%83%B3%E3%83%89%E3%83%AA%E3%83%B3%E3%82%B0.md))
- Userモデル作成済み。(カラム => name, email, crypted_password, salt)
~~~
[app/models/user.rb]

class User < ApplicationRecord
  authenticates_with_sorcery!
end
~~~
***

# 目標
`/authentication`にPOST形式のリクエストを送って、  
userが存在したら以下のように JSON形式で user情報を
~~~
{
  "data"=>{
    "id"=>"1", "type"=>"user", "attributes"=>{
      "name"=>"MyString1", "email"=>"MyText1"
    }
  }
}
~~~
存在しなければ以下のように JSON形式で 404エラー情報を返す。
~~~
{
    "message": "Record Not Found",
    "errors": [
        "ActiveRecord::RecordNotFound"
    ]
}
~~~
***

# Userのシリアライズ作成
gem [fast-jsonapi](https://github.com/Tarara33/TIL/blob/main/Rails/Gem/fast-jsonapi.md)で Userのシリアライズを作成する。
***

# authenticationコントローラー作成・ルーティング設定
~~~
$ rails g controller authentications
~~~

ルーティング設定
~~~
[config/routes.rb]

resource :authentication, only: [:create]
~~~
***

# authenticationコントローラーファイル編集
~~~
[app/controllers/authentications_controller.rb]

class AuthenticationsController < BaseController
  def create
    user = login(params[:email], params[:password])
    💙raise ActiveRecord::RecordNotFound unless user

    json_string = UserSerializer.new(user).serialized_json
    render json: json_string
  end

  private
  ⭐️def form_authenticity_token; end
end
~~~
💙 userが　nilなら例外処理を出す。
***

### ⭐️ form_authenticity_token
Railsの CSRF（クロスサイトリクエストフォージェリ）保護機能の一部で、  
フォームを通じて送信されるデータが正規のユーザーからのものであることを確かめるためのトークンを生成する。

コードを見ると、このメソッドがオーバーライドされていて、何もしない（空のメソッド）状態になっている。  
APIコントローラで CSRF保護は通常不要だから、  
このオーバーライドは CSRFトークンのチェックを無効にするために使われている。    
APIはトークンベース認証やセッションベース認証を使用することが多いから、  
このように CSRFトークンを無視することがある。  
***

#### ❓　書かないとダメ？？
書かないと、エラー出た。
Railsでは、通常、フォームからデータを送信する際には CSRFトークンを含める必要がある。  
form_authenticity_tokenメソッドはそのために用意されたもので、  
このメソッドが実装されていないと、CSRF対策が無効になり、フォームの送信時にエラーが発生する。
***

# Postmanでリクエストの確認
[Postman](https://github.com/Tarara33/TIL/blob/main/%E3%83%84%E3%83%BC%E3%83%AB/Postman.md)で`/authentication`にPOSTリクエスト送ってみる。
***
