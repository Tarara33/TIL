# map()
新しい配列を作る
~~~
const array = [1,2,3];
const newArray = array.map(a => a * 2);
console.log (newArray); //[2,4,6]
~~~
⚠️引数ひとつなので()省略    
⚠️処理がreturnのみなので{}とreturn省略
***

# filter()
要素のうち条件に合うものを抽出する
~~~
const array = [1,11, 56, 104];
const evenNumber = array.filter((number) => {
  if (number % 2 === 0) {
    return true;
  }
  return false;
});
console.log(evenNumber); // [56,104]
~~~
短くもかける
~~~
const array = [1,11, 56, 104];
const evenNumber = array.filter(number => number % 2 === 0);
console.log(evenNumber); // [56,104]
~~~
***

# Object.keys()
オブジェクトからキーを配列として抽出する  
Object.keys(キーを抽出したいオブジェクト名)
~~~
const point = {x:10, y:20, s:30, r:40};
const keys = Object.keys(point);
console.log(keys); //[x,y,s,r]
~~~
***

# substring()
部分文字列を取得する    
substring(開始インデックス,終了インデックス)
~~~
const str = "hello";
console.log(str.substring(2,4)); //ll
~~~
***

# join()
配列を文字列にする
join("繋ぎに使う文字")
~~~
const d = [2022, 5, 12];
console.log(d.join("/")); //2022/5/12
console.log(d.join("")); //2022512
~~~
***

# split()
文字列を配列にする
split("区切る文字")
~~~
const t = "12:30:09";
console.log(t.split(":")); //["12", "30", "09"]
~~~
それぞれ区切ったものを別々の定数（変数）に入れたかったら分割代入する
~~~
const t = "12:30:09";
const [hour, minutes, second] = t.split(":");
console.log(hour); //12
console.log(minutes); //30
console.log(second); //09
~~~
***

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

# new Date()
現在の日時が表示される
`const d = new Date();`
***

# getFullYear()
2023などと表示   
`console.log(d.getFurrYear())`　とnew Dateした定数（変数）に使う
***

# getMonth()
０〜11で月を表示（１月が０）
`console.log(d.getMonth())`　とnew Dateした定数（変数）に使う    
~~~
`console.log(`${d.getMonth() + 1}`)　
~~~
０〜11で月を表示するので＋１するといい
***

# getDate()
１〜31で日にちを表示   
`console.log(d.getDate())`　とnew Dateした定数（変数）に使う
***

# getDay()
０〜６で曜日を表示（sun:0, mon:1...）
`console.log(d.getDay())`　とnew Dateした定数（変数）に使う
***

# getHours()
０〜23で時を表示
`console.log(d.getHours())`　とnew Dateした定数（変数）に使う
***

# getMinutes()
０〜59で分を表示
`console.log(d.getMinutes())`　とnew Dateした定数（変数）に使う
***

# getSeconds()
０〜59で秒を表示
`console.log(d.getSeconds())`　とnew Dateした定数（変数）に使う
***

# getMilliseconds()
０〜999で 1/1000秒を表示
`console.log(d.getMilliseconds())`　とnew Dateした定数（変数）に使う
***
