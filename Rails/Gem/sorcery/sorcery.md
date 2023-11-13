[公式](https://github.com/Sorcery/sorcery)

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

## モデル作成
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

## View作成
saltカラムと crypted_passwordカラムはユーザーが直接変更・表示できないようにするためビューには書かない。    
その代わり、ビューではpasswordとpassword_confirmationを利用する。
~~~
[app/views/users/new.html.erb]
<%= form_with model: @user, local: true do |f| %>
  <%= f.label :email %>
  <%= f.text_field :email %>
  
  <%= f.label :password %>
  <%= f.password_field :password %>
  
  <%= f.label :password_confirmation %>
  <%= f.password_field :password_confirmation %>
  
  <%= f.submit "登録" %>
<% end %>
~~~
***

## コントローラー作成
~~~
$ rails g controller users
で作ったら

[app/controllers/users_controller.rb]
class UsersController < ApplicationController
  # ...
  private

  def user_params
    params.require(:user).permit(:email, :password, :password_confirmation)
  end
end
~~~
paramsで受け取るカラムはformと同じくpasswordとpassword_confirmationを利用する。
***

## モデルの編集
~~~
[app/models/user.rb]
class User < ActiveRecord::Base
  ①authenticates_with_sorcery!

  ②validates :password, length: { minimum: 3 }, if: -> { new_record? || changes[:crypted_password] }
  ②validates :password, confirmation: true, if: -> { new_record? || changes[:crypted_password] }
  ②validates :password_confirmation, presence: true, if: -> { new_record? || changes[:crypted_password] }

  validates :email, uniqueness: true
end
~~~
①記述することでUserモデルにsorceryによる認証機能を持たせる。    
    
②passwordやpassword_confirmationのようなモデルにないカラムについては、    
「if: ->...」を記述することでsorceryに、それがcrypted_passwordカラムの内容であることを認識させる。
***

# 使えるようになる主なメソッド    
[参考](https://blog.aiandrox.com/posts/tech/2020/01/18/)

## require_login
ログインをしていないユーザーをアクション単位で弾く。    
以下のようにbefore_actionで指定する。    
アクセスしようとしたURLをセッションに格納し、not_authenticatedを実行するメソッド。
~~~
[コントローラーに定義]
before_action :require_login
~~~
***

## logged_in?
現在ログイン中かどうか、true or falseで返す。    
コントローラ、ビューで使える。    
ログインしているかどうかによって場合分けをしたいときに使うことが多い。    
~~~
[view.html.erb]
<% if logged_in? %>
  <%= link_to 'プロフィール', user_url(current_user) %>
<% else %>
  <%= link_to 'ログイン', login_url %>
<% end %>
~~~
***

## not_authenticated
先ほどのrequire_login内で、このメソッドも実行される。    
デフォルトではredirect_to root_path（自動的にルートに飛ばされる）と定義されているが、    
カスタマイズしたい場合はapplication_controllerで上書きをする。
~~~
[application_controller]
class ApplicationController < ActionController::Base
  protected

  def not_authenticated
    redirect_to login_url, alert: 'ログインしてください'
  end
end
~~~
***

## current_user
現在ログイン中のuserを返す。コントローラ、ビューで使える。    
***

## auto_login()
その名の通りオートログイン。  
メールアドレスやパスワードを使わず userとしてログインする。  
使い道は、ユーザー登録後にログインさせる、ゲストでログインするなど
~~~
[ユーザー登録後にログイン]

class UsersController < ApplicationController
  def create
    @user = User.new(user_params)
    if @user.save
      auto_login(@user)
      redirect_back_or_to items_path, success: t('.success')
    else
      flash.now[:danger] = t('.fail')
      render :new, status: :unprocessable_entity
    end
  end
end
~~~
~~~
[ゲストでログイン]

class UserSessionsController < ApplicationController
  def guest_login
    guest_user = User.find_by!(role: 'guest')
    auto_login(guest_user)
    redirect_to root_path, notice: 'ログインしました'
  end
end
~~~
***



