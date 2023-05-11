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

# 関数宣言　と　関数式（無名関数）
~~~
[関数宣言]
function double(num) {
  return num * 2;
}
~~~
~~~
[関数式]
const double = function(num) {
  return num * 2;
}; //⚠️セミコロンつける
~~~
変数（もしくは定数）に関数を値として代入し、後からその変数を呼び出すことで関数を間接的に利用する方法   
セミコロン忘れずに
***

# アロー関数
functionを使わず=>で表現   
関数式に使う    
~~~
[関数式]
const double = function(num) {
  return num * 2;
}; //⚠️セミコロンつける
double(3); //6
~~~
~~~
[アロー関数]
const double = (num) => {
  return num * 2
}
double(3); //6
~~~
⚠️引数がひとつだった場合は()をつけなくてもいい
~~~
[アロー関数]
const double = num => {
  return num * 2
}
double(3); //6
~~~
⚠️関数ブロック内で実行する処理がreturn文だけだった場合、ブロックをあらわす {} 、そしてreturnも省略して記述することができる
~~~
const double = num => num * 2;
double(3); //6
~~~
***
