# return
関数の処理結果を返すときに使う。  
~~~
[js]
function sum(a,b) {
  return a + b;
}
sum(300,700); //1000
console.log (sum(300,700) //1000
~~~
***

## ❓ returnがない場合
~~~
[js]
function sum(a,b) {
  a + b;
  //return undefined;
}
sum(300,700); //undefined
console.log (sum(300,700) * 3); //NaN
~~~
returnを省略すると表示はされないが return undefined;が入るため、最終的に sumは undefinedが戻り値で入っている。      
そのため、戻り値を使ってさらに計算したい時にundefined * 3となり NaNとなる。
***

## ⚠️ returnの注意点
returnの後に続く返したい結果を改行すると、返されない。
~~~
[js]
function sum(a,b) {
  return
    a + b;
}
sum(300,700); //undefined
~~~
なぜかというと、改行すると、
~~~
return ;
a + b;
~~~
このように、セミコロンがついて、return文は終了と JSが判断するため。

なので、改行や複数行の処理をreturnしたい場合
~~~
return (
  a + b
);
~~~
このように `()`で囲む！
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

