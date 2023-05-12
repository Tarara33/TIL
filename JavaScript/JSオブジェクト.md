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
