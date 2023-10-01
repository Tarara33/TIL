# new Date()
現在の日時が表示される
~~~
[現在日時]
const d = new Date();　//現在の日時

[指定日時]
const d = new Date(2000,6)
console.log (d); //2000,6,1 00:00:00
~~~
⚠️指定するとき「年、月」は指定する

## new Dateした定数（変数）に使う
- get/set FullYear()...2023などと表示   
- get/set Month()...０〜11で月を表示（１月が０）   
`console.log('${d.getMonth() + 1}')`と０〜11で月を表示するので＋１するといい
- get/set Date()...１〜31で日にちを表示 
- get/ser Day()...０〜６で曜日を表示（sun:0, mon:1...）
- get/set Hours()...０〜23で時を表示
- get/set Minutes()...０〜59で分を表示
- get/set Seconds()...０〜59で秒を表示
- get/set Milliseconds()...０〜999で 1/1000秒を表示
~~~
[get]
const d = new Date();
console.log(d.getMonth()); //0(一月)
console.log(d.getDay()); // 2(火曜日)

[set]
const d = new Date(2000,6)
console.log (d); //2000,6,1 00:00:00
d.setDate(25);
console.log(d); //2000,6,25 00:00:00
d.setHours(12,15,30);
console.log(d); //2000,6,25 12:15:30
~~~
setはまとめて複数の引数を渡すとより小さい単位までセットできる
***
