# upto / downto
## upto
`スタートの数.upto(最大数) do~end`	
指定した開始数から最大数まで 1回ずつ増加しながら繰り返しを行いたいときに使用する。
~~~
3.upto(5) do |num|
  puts "LOVE"
end

[実行結果]
LOVE
LOVE
LOVE
~~~
***

## downto
`スタートの数.upto(最小数) do~end`	
uptoの逆で開始数から最小数まで 1回ずつ減らしながら繰り返しを行いたいときに使用する。
~~~
5.downto(3) do |num|
  puts "LOVE"
end

[実行結果]
LOVE
LOVE
LOVE
~~~
***
