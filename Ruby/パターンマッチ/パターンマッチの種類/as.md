# as
asパターンは、パターンマッチでマッチしたオブジェクトを変数に代入する利用パターン。  

例えば以下のようなパターンマッチの場合マッチはするが、値が取得できない。
~~~
case {name: "Alice", age: 20, gender: :female }
in {name: String, age: 18..}
  puts "name = #{name}, age = #{age}"
end

=> undefined local variable or method `name' for main:Object (NameError)
name の中身 Aliceや、ageの中身 20が取得できていない。
~~~
***

# `=> 変数名`
[ココ](https://github.com/Tarara33/TIL/blob/main/Ruby/%E3%83%91%E3%82%BF%E3%83%BC%E3%83%B3%E3%83%9E%E3%83%83%E3%83%81/%E3%83%91%E3%82%BF%E3%83%BC%E3%83%B3%E3%83%9E%E3%83%83%E3%83%81%E3%81%AE%E7%A8%AE%E9%A1%9E/array.md#-%E6%8C%87%E5%AE%9A%E3%81%97%E3%81%9F%E6%9D%A1%E4%BB%B6%E3%81%AE%E4%B8%AD%E8%BA%AB%E3%82%82%E4%BD%BF%E3%81%84%E3%81%9F%E3%81%84)でも少し登場したが、`=> 変数名`として変数に代入するとマッチした値が取得できる。
~~~
case {name: "Alice", age: 20, gender: :female }
in {name: String => get_name, age: 18.. => get_age}
  puts "name = #{get_name}, age = #{get_age}"
end

=> name = Alice, age = 20
~~~
***

# オブジェクト全体が欲しい時
マッチした値を　それぞれ nameや ageなど個別ではなく、ハッシュごと欲しい時は、一番外側に`=> 変数名`を書く。
~~~
case {name: "Alice", age: 20, gender: :female }
in {name: String, age: 18..} => get_person
  puts "person = #{get_person}"
end

=> person = {:name=>"Alice", :age=>20, :gender=>:female}
~~~
***
