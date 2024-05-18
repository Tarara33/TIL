# chars 
文字列の各文字を文字列の配列で返す。(self.each_char.to_a と同じ)
~~~
"hello世界".chars
=> ["h", "e", "l", "l", "o", "世", "界"]
~~~
***

# each_char
文字列の各文字に対して繰り返す。
~~~
"hello世界".each_char{ |str| print str, '' }
=> h e l l o 世 界
~~~
***
