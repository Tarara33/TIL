# gsub
第一引数にマッチした文字を第二引数に変える。
~~~
text = "あいうえお"

text.gsub('あ', 'a')
=> "aいうえお"
~~~
***

# 正規表現でも使える
~~~
text = "123:456,78-9"

text.gsub(/[:,-]/, ';')
=> "123;456;78;9"
~~~
***

# 変換のルールを指定する
第二引数にハッシュを渡して変換ルールが指定できる。
~~~
text = 'あいうえお　かきくけこ'
hash = {'あ' => '1', 'か' => '2'}

text.gsub(/[あか]/, hash)
=> "1いうえお　2きくけこ"
  ~~~
  ***
  
  ## ブロックで条件出す
  例えば、もしカンマ(,)なら、コロン(:)に変えてそれ以外はスラッシュ(/)にする。
  ~~~
  text = '1,2.3:4-5'
  
  text.gsub(/[,.:-]/) { |matched| matched  == ',' ? ':' : '/'}
  => "1:2/3/4/5"
  ~~~
***

# ⚠️ 文字列にしようするメソッドである
数値にたいしては使えない。
~~~
num = 123
puts num.gsub(1, 9)
=> エラー！
~~~
***
