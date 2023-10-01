# Math.floor()　と　　Math.ceil()　と　round()
- fllor...小数点以下切り捨て
- ceil...小数点以下切り上げ
- round...四捨五入
~~~
const x = 12.33333;
console.log(Math.floor(x)); //12
console.log(Math.ceil(x)); //13
console.log(Math.round(x)) //12
~~~
***

# toFixed()
指定した桁数になるように数値を丸めるかつ、指定桁数のところを四捨五入する場合に使う
~~~
const x = 12.33391;
console.log(x.toFixed(3)); //12.334
~~~
***

# Math.random()
0以上１未満のランダムな数値を生成してくれる
~~~
console.log(Math.random()); //0.5430438839381262
~~~
***
