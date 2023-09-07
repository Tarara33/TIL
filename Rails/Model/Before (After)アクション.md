# Before (After)アクション(モデルの場合)
コントローラーには、before_actionなど指定のアクションを行う前にするアクションなどを定義する    
beforeがあるが、モデルにも同じように定義づけができる。  
***

# Before
例えば、Userモデルのインスタンスをsaveするときに、emailカラムの入力を小文字にしたいとき    
(emailに一意性を持たせ、TARARA@exsample.comも　tarara@exsample.comも同一に扱いたいため、小文字で保存する)    
saveする前にこの形式でDBに保存してね！という意味で使う。
~~~
[app/models/user.rb]
class User < ApplicationRecord
  before_save ::downcase_email

 private

  # メールアドレスをすべて小文字にする
  def downcase_email
    self.email = email.downcase
  end
end
~~~
***
