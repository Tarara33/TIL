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
消された値を変数に入れることもできる
~~~
const scores = [10, 20, 30, 40];
const deleted = scores.splice(3,1);
console.log(deleted);
// [40]
~~~
⚠️deleteされた値は一つでも配列で代入される。
***

# 配列にオブジェクトを入れる
~~~
const point = [
{x:10, y:20},
{x:30, y:40},
];
~~~
***

# 配列オブジェクトの呼び出し
定数（変数）名[オブジェクトのインデックス番号].キー名
~~~
const point = [
{x:10, y:20},
{x:30, y:40},
];
console.log(point[1].y); //40
~~~
***

# 配列オブジェクトの追加
- unshift()...最初に追加
- push()...最後に追加
~~~
const point = [
{x:10, y:20},
{x:30, y:40},
];
point.unshift({e:30, s:70});
point.push({b:40, t:70});
console.log(point);
//[
{e:30, s:70},
{x:10, y:20},
{x:30, y:40},
{b:40, t:70}
]
~~~
***

# 配列オブジェクトの削除
splice(変化が開始するインデックス番号, 削除数, 追加する要素);を使う
~~~
const point = [
{x:10, y:20},
{x:30, y:40},
];
point.splice(0,1);
console.log(point);
//[{x:30, y:40}]
~~~
追加オブジェクト入れたら追加される
***
