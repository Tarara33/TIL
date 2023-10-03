# rjust
`rjust(width, padding = ' ') -> String`  
  
長さ widthの文字列に selfを右詰めした文字列を返す。    
selfの長さが widthより長い時には元の文字列の複製を返す。  
また、第2引数 paddingを指定したときは空白文字の代わりに paddingを詰める。  
⚠️ 第二引数は数値でもクオーテーションで囲って文字列にする。
~~~
[irb]

"foo".rjust(5)
=> "  foo"

"foo".rjust(1)
=> "foo"

"foo".rjust(5, '1')
=> "11foo"
~~~
***
