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

# 引数
- 第一引数...配列の中身
- 第二引数...インデックス
- 第三引数...配列自体
~~~
const array = [1,2,3];
const newArray = array.map((val, i, array) => {
  console.log(i, val, array)
});

=> 
0 1 [1, 2, 3]
1 2 [1, 2, 3]
2 3 [1, 2, 3]
~~~
***
