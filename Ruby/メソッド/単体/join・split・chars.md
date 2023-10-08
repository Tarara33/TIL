# join
文字列を結合する。
~~~
str = ['a', 'b', 'c']
str.join
=> "abc"
~~~
***

## 数値も結合できる
`to_s`メソッドが使われるので、文字列・数値の混ざった配列もOK
~~~
array = ['a', 1, 'Z', 2]
array.join
=> "a1Z2"
~~~
***

## 引数繋げたい文字列を指定
~~~
str = ['a', 'b', 'c']

str.join('')
=> "abc"

str.join(' ')
=> "a b c"

str.join('-')
=> "a-b-c"

str.join('Z')
=> "aZbZc"

str.join(1)
=> エラー
~~~
⚠️ 引数に数値はエラー出る。
***

# split 
引数で渡した区切り文字で文字列を分割する。
~~~
"Ruby on Rails".split(' ')
=> ["Ruby", "on", "Rails"]
~~~
***

# chars
文字列を文字一文字ずつで分割する。
~~~
"Ruby on Rails".chars
=> ["R", "u", "b", "y", " ", "o", "n", " ", "R", "a", "i", "l", "s"]
~~~
***
