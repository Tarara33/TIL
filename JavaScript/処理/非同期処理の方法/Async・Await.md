# Async/Await
Async/Awaitとは、Promiseによる非同期処理をより効率良く記述することができる構文。
***

# Async
asyncを関数の前に置くことで、asyncファンクションを生成することができる。    
⭐️ asyncファンクションは、常に resolveされた Promiseオブジェクトを返す。
~~~
async function sample() {
  return 123;
}

sample().then(function(value) {
  console.log(value)
});
// 123


[明示的にPromise resolve書いた場合]
async function sample() {
  return Promise.resolve(123);
}

sample().then(function(value) {
  console.log(value);
});
// 123
~~~
***
    
アロー関数でもかける
~~~
const sample = async () => {
  return 123;
};

sample().then(value => console.log(value));
// 123
~~~
***

# Await
⭐️ awaitは、【async関数内のみ】で使用できる予約語。     
普通の関数で使うとエラーになる。
Promiseが結果を返すまで処理を待機させることが特徴的で、thenメソッドのような役割がある。
~~~
①async function sample() {
  console.log(1)
  return 123;
}

② async function sample_2() {
  console.log(2)
  ④const result = await sample(); // Promiseの結果が分かるまで待機
  console.log(result);
}

③ sample_2();
// 2 1 123
~~~
**【処理の流れ】**  
③で sample_2()実行して②の処理が始まる。    
④で ①sample()を呼び出しているが、awaitで処理が終わるまで待つ。    
sample()の処理が終わったら、sample2の処理の続きへ。
***
