# returnの使い所
~~~
[js]
function sum(a,b) {
  console.log (a + b);
  //return undefined;
}
sum(300,700); //1000
console.log (sum(300,700) * 3); //NaN
~~~
returnを省略すると表示はされないがreturn undefined;が入るため、最終的にsumはundefinedが戻り値で入っている    
そのため、戻り値を使ってさらに計算したい時にundefined * 3となりNaNとなる
***

# 仮引数のデフォルト値
仮引数 = デフォ値　で設定できる
~~~
[js]
function totalPrice(price, amount, rate = 1.1) {
  return price * amount * rate;
}
console.log (totalPrice(100,10)); //1100
~~~
***

# 早期return
関数内で特殊なケースを先に篩い落とす技 
~~~
[js]
function Total(price, amount, rate = 1.1) {
  if (amount >= 100) {
    return price * amount;
  }
  return price * amount * rate;
}
~~~

# 関数宣言　と　関数式
~~~
[関数宣言]
function double(num) {
  return num * 2;
}

[関数式]
const double = function(num) {
  return num * 2;
}; //⚠️セミコロンつける
~~~
***
