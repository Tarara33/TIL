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
