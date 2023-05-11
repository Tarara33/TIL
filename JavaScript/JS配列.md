# 呼び出し
定数名（変数名）[インデックス番号]　とする
~~~
const a = [1,2,3,4];
console.log(a[0]); //1
~~~
***

# 変更
定数名（変数名）[変更したいところのインデックス番号]　= 変更内容　とする
~~~
const a = [1,2,3,4];
console.log(a[0]); //1

a[3] = 10;
console.log(a); //[1,2,3,10]
~~~
***

# 追加
- unshift()...最初に追加
- push()...最後に追加
~~~
const a = [1,2,3,4];
console.log(a); //[1,2,3,4]

a.unshift(0);
a.push(5);
console.log(a); //[0,1,2,3,4,5]
~~~
***

# 削除
- shift...最初を削除
- pop...最後を削除
~~~
const a = [1,2,3,4];
console.log(a); //[1,2,3,4]

a.shift;
a.pop;
console.log(a); //[2,3]
~~~
shiftもpopもひとつずつしか削除できないので()はいらない
***

# 配列中間にある要素の削除と追加
splice(変化が開始するインデックス番号, 削除数, 追加する要素);
~~~
const a = [1,2,3,4];
console.log(a); //[1,2,3,4]

a.splice(1,2);
console.log(a); //[1,4]

a.splice(0,0,10,100);
console.log(a); //[10,100,1,4]

a.splice(1,1,0)
console.log(a); //[10,0,1,4]
~~~
***

# スプレット構文
- ...配列名　とすることで配列中に別の配列を入れてくれる
~~~
const a = [1,2,3];
const b = [6,7,...a];
console.log(b); //[6,7,1,2,3]
~~~
引数としても扱える
~~~
const array = [2,5]
function sum(a,b) {
  console.log(a + b);
}
sum(...array); //7
~~~
⚠️引数よりも配列の要素が多い場合は前から順に代入されるため無視される
***


