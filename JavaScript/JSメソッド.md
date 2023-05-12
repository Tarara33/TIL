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
