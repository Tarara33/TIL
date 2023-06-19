# Decoratorとは？
Viewで使いたいメソッドがあるけど、モデルで書くと、モデルが肥大化するし、    
でもViewに書いても肥大化してしまう！と言うときに使う。
***

# 導入
gem `draper`をbundleして、    
`$　rails generate draper:install`する。    
そうすると、`$ rails generate decorator ○○(モデル名)`と言うコマンドが使えるようになる。    
~~~
例：　Userモデルのdecoratorなら
$ rails g decorator User
とする
~~~
***

# ファイルの編集
「app/decorators/」以下にファイルが作成されているので編集する
~~~
[app/decorators/user_decorator.rb]

class UserDecorator < Draper::Decorator
  delegate_all => 元から書いてある

  def full_name
    "#{model.first_name} #{model.last_name}"
  end
=> userモデルのカラム「first_name」と「last_name」をくっつけて
full_nameにするメソッドを定義。

end
~~~
***

# メソッドの使用
- 例えばレシーバーがdecoratorモデル源のインスタンスなら、
~~~
[user decoratorしている場合]
user.dacorate.full_name
となる
~~~
***

- 例えばレシーバーが他のモデルのインスタンスなら
~~~
[user decoratorしていて、boardモデルインスタンスで呼びたい]
①モデルをアソシエーションする
②board.user.decorate.full_name
とする
~~~
***

- current_userの場合
~~~
あるuserモデルと紐づいてるので
current_user.decorate.full_name
で使える
~~~
***
