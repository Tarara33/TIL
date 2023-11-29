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

# Object.keys　と　forEachの組み合わせ
オブジェクトのキーと値を出す方法
~~~
const point = {x: 10, y: 50, r:30, s:900};
const keys = Object.keys(point);
keys.forEach(key => {
  console.log(`key: ${key}, value: ${point[key]}`);
});
//
key: x value: 10
key: y value: 50
key: r value: 30
key: s value: 900
~~~
⚠️forEachでは文字列で配列要素を取得しているため`value: ${point[key]}`のkeyは`${point.key}`とできない
***

# オブジェクトの合計値を出す
①オブジェクトの中身をObject.entriesで配列にする。 
②変数sumを定義（合計値入れる用）    
③できた配列に対してforEachする。    
④でた合計値を叫ぶ＋平均値も出せる。
~~~
①const scores = {
  math: 80,
  english: 30,
};
const entries = Object.entries(scores);
console.log(entries);
// ['math', 80], ['english', 30]

②let sum = 0;

③entries.forEach((entry) => {
  sum += entry[1];
  console.log(`${entry[0] : ${entry[1]}`)
})
//math: 80
  english: 30
  
④console.log(`SUM: ${sum}`)
 console.log(`AVG: ${sum/entries.length}`)
//SUM: 110
  AVG: 55
~~~
なぜ「entry[1]」に足していくのか？   
=> 配列の中のインデックス番号[1]に足していくため。[0]に足すと「math + 」のようになるため。   
***

# ⭐️ 省略記法
オブジェクトに値を入れる時、キー名が同じなら省略できる。
~~~
const name = 'TARARA';
const age = 29;
=> これらの値を myProfileオブジェクトに入れる

[省略なし]
const myProfile = {
  name: name,
  age: age
};

[省略あり]
const myProfile = {
  name,
  age
};

↓

const myProfile = { name, age };
~~~
***
