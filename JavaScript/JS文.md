# switch文
caseをひとつではなく〇〇もしくは〇〇だったらという複数条件にしたい場合   
~~~
case "blue":
case "green":
~~~
と
caseを条件分書く
***

# while文　と　do while文
~~~
[while]
let x = Number(prompt("数字を入力して下さい。0で終わります。"));
while (x !== 0) {
  console.log(`${x}が選ばれました。`)
  x = Number(prompt("数字を入力して下さい。0で終わります。"));
}
~~~
~~~
[do while]
let = x;
do {
  x = Number(prompt("数字を入力して下さい。0で終わります。"));
  if (x === 0) {
    console.log("終了します。");
  } else {
    console.log(`${x}が選ばれました。`)
  }
} while (x !== 0);
~~~
while文との違いはdo while文では条件を満たしているかどうかに関わらず必ず一回は繰り返し処理を行う     
例文で見るとwhileは初手0を入力された時console.logに辿り着けないので何もなく終わるが、   
do whileは条件の判断が最後なのでconsole.logされる
***

# 条件演算子
`score > 80 ? "A" : "B";`scoreが80以上ならA、以下ならBを返す文    
また最終的に1つの値になるので、`const result = score > 80 ? "A" : "B";`このように定数や変数に代入して使うことができる
***

# for文
ループ構文   
ある処理をN回繰り返したいときに使える   
配列で要素分だけ繰り返す文はこういう感じ
~~~
const array = [1,2,3,4];
for (let i = 0; i < array.length; i ++) {
  console.log(`array ${i}: ${array[i]}`);
}
//array 0: 1
  array 1: 2
  array 2: 3
  array 3: 4
~~~
***

# forEach()
配列のような「要素に順序を持つオブジェクト」が持つメソッドで、「その要素1つ1つに対して順番にある処理をしたい」ときに使う   
配列に使うことが多く渡す引数によって取り出される内容が変わる
- 第一引数...配列の要素
- 第二引数...インデックス番号
- 第三引数...配列全体
~~~
const array = [1,2,3,4];
array.forEach((a,index) => {
  console.log(`array ${index}: ${a}`);
}); 
//array 0: 1
  array 1: 2
  array 2: 3
  array 3: 4
~~~
***
