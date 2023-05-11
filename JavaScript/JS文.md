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
