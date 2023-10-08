# each文
オブジェクトに含まれる要素を順番に取得する。  
~~~
[配列の場合]

array = [1,2,3]
array.each do |x|
  puts x
end

[実行結果]
1
2
3


[ハッシュの場合]

hash = {sam: 28, yusuke: 29, misa: 20}
hash.each do |key, value|
puts "#{key}は#{value}歳です"
end

[実行結果]
samは28歳です
yusukeは29歳です
misaは20歳です
~~~
***

## ２つの each
２つの each文で breakがあったとき、どちらのループ(1..3)or(7..9)が止まるのか分からなかった。
~~~
(1..3).each do |num1|
    (7..9).each do |num2|
      sum = num1 + num2
      break if sum == 9
      puts sum
    end
  end

=> 8
~~~
処理の順番は  
1. num1(1) + num2(7) = 8  
2. num1(1) + num2(8)= 9  
3. num1(1) + num2(9)= 10 putsされず、ここでnum1(1)のループがbreakによって止まり、  
    
次の num1(2)の処理に入る  
1. num1(2) + num2(7) = 9   
2. num1(2) + num2(8) = 10 今回はここでnum1(2)のループが止まり、  
  
次の num1(3)の処理に入る
1. num1(3) + num2(7) = 10 ここでnum1(3)のループは終わり。
***
