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
