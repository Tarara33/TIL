# hash
[ここ](https://github.com/Tarara33/TIL/blob/main/Ruby/%E3%83%91%E3%82%BF%E3%83%BC%E3%83%B3%E3%83%9E%E3%83%83%E3%83%81/%E3%83%91%E3%82%BF%E3%83%BC%E3%83%B3%E3%83%9E%E3%83%83%E3%83%81%E3%81%A8%E3%81%AF.md#%E3%83%8F%E3%83%83%E3%82%B7%E3%83%A5%E7%B7%A8)で書いたように in節にハッシュ構造を利用するパターン。  
ハッシュパターンはハッシュの各要素が in節で指定したパターン(キーと値、　またはキーのみ)に部分一致すればマッチと判定される。

# in節に キーがあるかのマッチ
~~~
case { name: "Alice", age: 18 }
in { name: name, age: age }
  puts "name = #{name}, age = #{age}"
end

=> name = Alice, age = 18
~~~
***

### 💡 値の変数を省略すると、キー名と同じ名前の変数に値が代入される。
~~~
case { name: "Alice", age: 18 }
in { name:, age: }
  puts "name = #{name}, age = #{age}"
end

=> name = Alice, age = 18
~~~
***

### 💡 in節でかく、キーの順番は caseと同じじゃなくてもOK
~~~
case { name: "Alice", age: 18 }
in {  age:, name: }
  puts "name = #{name}, age = #{age}"
end

=> name = Alice, age = 18
~~~
***

# 値も指定するマッチ
たとえば
- nameキーがあり、値が Alice
- ageキーがあり、値が 18
- genderキーがある(値は問わない)
~~~
case {name: "Alice", age: 18, gender: :female}
in {name: "Alice", age: 18, gender:}
  puts "HIT"
end

=> HIT
~~~
***

# 配列と混在もできる
hashパターンと arrayパターンを混在もできる。  

たとえば、
- nameと childrenキーを持つ
- childrenの値が要素一つ
~~~
case {name: "Alice", children: ["Bob"]}
in {name:, children: [child]}
  puts "name = #{name}, child = #{child}"
end

=> name = Alice, child = Bob
~~~
***

# ⚠️ ハッシュパターンの落とし穴
~~~
humans = [
  {name: "Alice", age: 18, height: 165},
  {name: "Bob", age: 25},
]

humans.each do |human|
  case human
  in {name:, age:, height:}
    puts "ハッシュの要素が３つです"
  in  {name:, age:}
    puts "ハッシュの要素が２つです"
  end
end

=> ハッシュの要素が３つです
=> ハッシュの要素が２つです
~~~
この場合、Aliceのハッシュは最初の in節にマッチして「ハッシュの要素が３つです」と出るが、  
in節の順番を変えると、Aliceのハッシュも「ハッシュの要素が2つです」にマッチしてしまう。
~~~
humans.each do |human|
  case human
  in  {name:, age:}
    puts "ハッシュの要素が２つです"
  in {name:, age:, height:}
    puts "ハッシュの要素が３つです"
  end
end

=> ❗️ハッシュの要素が2つです
=> ハッシュの要素が２つです
~~~
***

## ❓ なぜ？？
`ハッシュパターンはハッシュの各要素が in節で指定したパターン(キーと値、　またはキーのみ)に部分一致すればマッチと判定される。`  
この**部分一致**という特徴を持つので、違う in節にマッチしてしまうこともある。  
(「そのキー(や値)を持っているか」は見るが、「キーの数は合っているか」は見ない。)  

つまり先ほどの例だと、指定した「nameキー、ageキー」があれば 「heightキー」がなくてもマッチしてしまう。    
なので、Aliceハッシュは nameキー、ageキーを含むため、一番目の in節でマッチしてしまった。
***

## ⚠️ 空のハッシュは完全一致が条件となる
通常は部分一致が条件だが、
