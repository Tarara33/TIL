# Before (After)アクション
コントローラーにはメソッド形式でさまざまなアクションがかかれるが、        
それとは別にそれぞれのメソッドアクションの前後にして欲しいアクションがあるときは、このように書く。    
    
この場合、index, edit, update, destroyアクションの前にプライベートメソッドで描かれてる    
logged_in_userメソッドを実行するという意味。  
~~~
[app/controllers/user_controller.rb]

class User < ActionController
  before_action :logged_in_user, only: [:index, :edit, :update, :destroy]

  private

  # beforeフィルタ
  # ログイン済みユーザーかどうか確認
  def logged_in_user
    unless logged_in?
      store_location
      flash[:danger] = "Please log in."
      redirect_to login_url, status: :see_other
    end
  end
~~~
***

# before_action と skip_before_action

## 違い
- before_action...それぞれのアクションをする前にするアクション。
applicationコントローラーで設定すると全てのコントローラーに適応される。
      
- skip_before_action...before_actionを実行しない。
applicationコントローラーで設定したbefore_actionを    
他のコントローラーでスキップするときに使うこと多い。
***

## 定義
コントローラー内でする
~~~
例[app/controllers/application_controller.rb]
class ApplicationController < ActionController::Base
  add_flash_types :success, :info, :warning, :danger
  before_action :require_login
=> sorceryメソッド

  private

  def not_authenticated
=> require_loginメソッドの時に呼ばれるsorceryメソッド

    flash[:warning] = t('defaults.message.require_login')
    redirect_to login_path
  end
end
~~~
⚠️このままだと全てのアクションでroot_pathに永遠とリダイレクトされてしまうので、    
ユーザー登録のアクションでは require_loginアクションをスキップする。
~~~
[app/controllers/user_controller.rb]
class UsersController < ApplicationController
  skip_before_action :require_login, only: %i[new create]

  def new
    @user = User.new
  end
...省略
end
~~~
***



