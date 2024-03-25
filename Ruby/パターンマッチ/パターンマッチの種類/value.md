# value
in節に 数値や文字列など直接指定できるパターン。  
case/when文に近い。
~~~
country = 'italy'

case country
  in 'japan'
    puts 'こんにちは'
  in 'us'
    puts 'Hello'
  in 'italy'
    puts 'Ciao'
end

=> Ciao
~~~
***

## 変数に代入できる
case/when文と同じで結果を変数に入れられる
~~~
country = 'italy'

message =
  case country
    in 'japan'
      'こんにちは'
    in 'us' 
      'Hello'
    in 'italy'
      'Ciao'
  end

puts message
=> Ciao
~~~
***

## thenも使える
case/when文と同じで thenも使える。
~~~
country = 'italy'

case country
  in 'japan'  then puts 'こんにちは'
  in 'us' then puts 'Hello'
  in 'italy' then puts 'Ciao'
end

=> Ciao
~~~
***

# ❓ case/whenとの違いは??
パターンマッチではパターンがマッチしないとエラーが出る。
~~~
【case文】
country = 'italy'

case country
  in 'japan'
    puts 'こんにちは'
  in 'us'
    puts 'Hello'
end

=> nil
--------------------------

【パターンマッチ】
country = 'italy'

case country
  in 'japan'
    puts 'こんにちは'
  in 'us'
    puts 'Hello'
end

=> : italy (NoMatchingPatternError)
~~~

### エラーを発生させたくない場合は `else`つける。
~~~
country = 'italy'

case country
  in 'japan'
    puts 'こんにちは'
  in 'us'
    puts 'Hello'
  else
  puts 'UnKnown'
end

=> UnKnown
~~~
💡 もし当てはまらない場合に例外起こしたいなら raiseでわざわざ例外起こさなくてもパターンマッチなら自動的に起きる。
***
