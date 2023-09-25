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
qはシングル、Qはダブルクオーテーションで囲む。
~~~
p %q(abc)
=> 'abc'

p %Q(abc)
=> "abc"
~~~

%w(W)...配列にする
~~~
p %w(a b c)
=> ["a", "b", "c"]
~~~

- %s...シンボル（：）にしてくれる
~~~
p %s(a)
=> :a
~~~

- %i(I)...シンボル（：）の配列にしてくれる
~~~
p %i(a b c)
=> [:a, :b, :c]
~~~

- %x...コマンド実行
~~~
p %x(ruby -v)
=>  "ruby 2.5.1p57 (2018-03-29 revision 63029) [-darwin22]\n"
~~~

- %r...正規表現
~~~
p %r(abc)
=> /\/abc\//
~~~
***

### 式展開（#{}）するかどうか
%Q,%W,%I,%s,%x,%r は式展開する    
そのため、
~~~
c = 3
p %W(a b #{c})
=> ["a", b, "3"]

p %w(a b c)
=> ["a", "b", "#{c}]
~~~
