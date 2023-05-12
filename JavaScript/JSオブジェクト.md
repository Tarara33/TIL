# オブジェクト
定数（変数）名　= {キー: 値, ...};　で作る   
`const point = {x:10, y:50};`   
見やすく書くと
~~~
const point = {
x:10, 
y:50,
};
~~~
最後のメンバー（y:50）には「,」つけなくてもいいがつけておくと追加した時に「,」がないミスがなくなる

# 変更
- 定数（変数）名.キー名　=　変更内容
- 定数（変数）名["キー名"]　=　変更内容
~~~
const point = {x:10, y:50};
point.x = 100;
point["y"] = 5;
console.log(point); // {x: 100, y:5}
~~~
***

# 追加
- 定数（変数）名.newキー名　=　new値
- 定数（変数）名["newキー名"]　=　new値
~~~
const point = {x:10, y:50};
point.s = 100;
point["a"] = 5;
console.log(point); // {x: 10, y:50, s:100, a:5}
~~~
***

#　削除
delete 定数（変数）名.キー名
~~~
const point = {x:10, y:50};
delete point.x;
console.log(point); // {y:50}
~~~
***

# スプレット構文
- ...配列名　とすることで配列中に別の配列を入れてくれる
~~~
const point = {x: 10, y: 50};
const other = {r: 30, s: 900, ...point};
console.log(other); //{r: 30, s: 900, x: 10, y: 50}
~~~
***

# 分割代入
オブジェクトを作ったけど、やっぱりこれらの値を別々の定数にしたかったとする   
定数（変数） {オブジェクトのキーと同じ定数名} = 元のオブジェクト名
~~~
const point = {x: 10, y: 50};
const {x, y}= point;
console.log(x); //10
console.log(y); //50
~~~
***

# レスト構文
オブジェクトを作ったがやっぱりこれらの値を別々の定数にしたいかつ、   
定数に入れたいのは最初の２つだけで残りはオブジェクトのままでいいという場合   
定数（変数） {オブジェクトのキーと同じ定数名,...新しいオブジェクト名} = 元のオブジェクト名
~~~
const point = {x: 10, y: 50, r:30, s:900};
const {x, r, ...other}= point;
console.log(x); //10
console.log(r); //30
console.log(other); //{y: 50, s: 900}
~~~
***


