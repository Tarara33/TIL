# Chapter1

## nilガードとは？？  
~~~
number || = 10
~~~
「もしもnumberにはいっているものがあればそれを、numberがnil(空っぽ)だったら10を代入した上でのnumber」と言うように   
nilの代わりに何らかのデフォルト値を入れる
***

## ぼっち演算子とは？？
`&.`を使って`レシーバ&.メソッド`と使う   
レシーバがnilでもエラーが発生しにくくなる
~~~
class User
  def name
    "名前"
  end
end

user = User.new
user.name
=> "名前"

object = nil
object.name
=> レシーバがnilなのでエラー！
object&.name
=> "名前"
~~~
***

### ぼっち演算子を使った簡潔な書き方
「もしobjectにnameがあればそれを、なければnilを返してください」
~~~
[if文]
if object <=省略されてるがobject=true、つまり何かしら代入されてる状態
  object.name
else
  nil
end

[三項演算子]
name = object ? object.name : nil

[ぼっち演算子]
name = object&.name
=> nameがあればそれを返すし、なければnil返す
~~~
***

## %記法
例えば、配列を作るとき本来は
~~~
array = ["りんご", "バナナ", "いちご"]
puts array
=> ["りんご", "バナナ", "いちご"]
~~~
などと書くが、％記法を使うと
~~~
array = %w(りんご, バナナ, いちご)
puts array
=> ["りんご", "バナナ", "いちご"]
~~~
""でわざわざ囲わなくても文字列の配列にしてくれる
***

### ％の種類

- %q(Q)...文字列にする
~~~

- %i...シンボル（：）にしてくれる
~~~
array = %i(りんご, バナナ, いちご)
puts array
=> [:りんご, :バナナ, :いちご]
~~~
