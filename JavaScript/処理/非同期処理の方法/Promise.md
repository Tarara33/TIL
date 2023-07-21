# Promise
ES6から登場した。    
Promiseとは、非同期処理の結果を保持するオブジェクトのこと。    
    
Promiseは、以下の3つのステータスを持っている。  
- Pending...初期の状態、または処理待ちの状態    
- Fullfilled...処理が成功して完了した状態    
- Rejected...処理が失敗した状態
    
これらのPromiseオブジェクトのステータスによって、処理を分岐していくことが特徴。
***

## Promiseオブジェクトの生成
~~~
const promise = new Promise((resolve, reject) => {});
~~~
処理が成功ならresolve、失敗ならrejectの Promiseオブジェクトを返す。

~~~
const promise = new Promise((resolve, reject) => {
  // 何かしらの処理
  const result = something();
  // 処理に成功ならresolve、失敗ならreject
  if (result === 'success') {
    resolve(result);
  } else {
    reject('fail')
  }
});
~~~
***

# thenメソッド
thenメソッドは、Fullfilledステータス(処理成功・完了)、      
または Rejectedステータス(処理失敗)の Promiseオブジェクトを受け取ることができる。    
resolveされた時にはその成功した結果を受け取り、rejectされた時にはそのエラーを受け取る。    
***

### resolveされた時
resolveの場合は、resolveの引数が thenの第一引数に渡ります。
つまり、resolveの引数'成功しました'と言う文字列が、thenの引数 resultに引き継がれることになる。
~~~
const promise = new Promise((resolve) => {
  resolve('成功しました');
}).then(result => console.log(result)); #resolveの引数'成功しました'が resultに入る。
// 成功しました
~~~
***

### rejectされた時
一方、rejectの場合は、thenの引数に2つの関数を用意することが必要。
しかし、実際に実行されるのは2番目の関数。
rejectの引数'失敗しました‘と言う文字列が、thenの第二引数 errorに引き継がれることになります。
~~~
const promise = new Promise((resolve, reject) => {
  reject('失敗しました');
}).then(
  result => console.log(result),  #第一引数resultさん,成功した場合こちらに　resolveの引数入る。
  error => console.log(error)     #第二引数　errorさん　,失敗した場合こちらに　rejectの引数が入る。　今回失敗なのでこちら。
);
// 失敗しました
~~~
***

## resolve・rejectされた時の処理をまとめて書く場合
~~~
[例]
const validation = (password) => {
  return new Promise((resolve, reject) => {
    if (password.length >= 10) {
      resolve({
        password: password,
      });
    } else {
      reject('パスワードは10文字以上で入力してください');
    }
  });
};

let password = '123456789';

validation(password).then(
  result => console.log(result), // バリデーションが成功した時の処理（resolveの引数 password: passwordが resultに入る）
  error => console.log(error)　// バリデーションが失敗した時の処理(rejectの引数'パスワードは10文字以上で入力してください'が errorに入る)
);
// 'パスワードは10文字以上で入力してください'
~~~
***

# catchメソッド
catchメソッドは、rejectedステータスの Promiseオブジェクトを受け取る。    
つまりかんたんに言うと、エラー処理専用のメソッド。   
    
thenメソッドと実質的に同じだが、    
⭐️ 新しいPromiseオブジェクトも作るところが違う。(thenメソッドは作らない)
~~~
const promise = new Promise((resolve, reject) => {
  reject('失敗しました');
}).catch(error => console.log(error));
// 失敗しました
~~~
***

### 新しいPromiseオブジェクト
catchで作成された Promiseオブジェクトはまだ「pending（未解決）」の状態にある。    
それが「resolve（解決）」するか「reject（拒否）」するかは、そのPromiseの中に書かれた処理による。
***

## Promiseチェーン
例えば失敗した処理の後に別の処理を入れたい時など、    
catch、then、catchとメソッドをつなぎ合わせていくことをPromiseチェーンと呼ぶ。    
⭐️ catchメソッドは thenメソッドと組み合わせて使用できる。    
~~~
const promise = new Promise((resolve, reject) => {
  reject(); #失敗
})
  .then(() => {
    console.log('resolve');　　　#成功なので処理されない
  })
  .catch(() => {
    console.log('error');　　　　　　　#失敗なので処理される　＋　新しいPromiseオブジェクトを返す。
  })
  .then(() => {
    console.log('resolve again');　　#新しいPromiseは resolveステータスなのでこれも処理される。
  });
// error
// resolve again
~~~
💡 catchでできる Promiseはresolveステータス。(厳密には違う)
***

# finallyメソッド
finallyメソッドとは、処理の成功・失敗に関わらず、その先の処理を継続して行うメソッド。        
Promiseチェーンのさいごに必ず呼び出したい処理などを定義することができる。
        
### result・rejectあろうとなかろうと関係ない
~~~
const promise = new Promise((resolve, reject) => {
  const something = true;
　　if (!something) {
　　  resolve('成功');
　　} else {
　　  reject('失敗');
　　}
}).finally(() => console.log('結果に関係なく処理'));
// '結果に関係なく処理'
~~~
***

### finaly - then
finalyは Promiseの成功または失敗のステータスを変えない。        
なのでこの場合そのまま 失敗の処理される。
~~~
const promise = new Promise((resolve, reject) => {
  const something = true;
  if (!something) {
　  resolve('成功');
  } else {
　  reject('失敗');
  }
})
  .finally(() => console.log('結果に関係なく処理'))
  .then( // resolve, またはrejectを扱う
    result => console.log(result),
    error => console.log(error)
  );
// '結果に関係なく処理'
// '失敗'
~~~
***

### finaly - catch
catchでも変わらず処理して、その先の処理の邪魔もしない。
~~~
const promise = new Promise((resolve, reject) => {
  const something = true;
  if (!something) {
    resolve('成功');
  } else {
    reject('失敗');
  }
})
  .finally(() => console.log('結果に関係なく処理'))
  .catch(error => console.log(error)); // rejectを扱う
// '結果に関係なく処理'
// '失敗'
~~~
***

# Promise.allメソッド
指定したすべてのPromiseオブジェクトに対して処理を実行するメソッド。        
【すべて】の Promiseオブジェクトが fulfilledステータスになると、それぞれの結果の値を集めた配列を返す。        
~~~
const promiseA = new Promise((resolve, reject) => {
  resolve(123)
});

const promiseB = new Promise((resolve, reject) => {
  resolve('string')
});

const promiseC = new Promise((resolve, reject) => {
  resolve(true)
});

Promise.all([promiseA, promiseB, promiseC]).then((results) => {
  console.log(results);
});
// [123, 'string', true]
~~~
***

⭐️ 【すべて】なので一つでも rejectあれば実行されない。
~~~
const promiseA = new Promise((resolve, reject) => {
  resolve(123)
});

const promiseB = new Promise((resolve, reject) => {
  resolve('string')
});

const promiseC = new Promise((resolve, reject) => {
  reject(false)
});

// promiseCがrejectされたため、実行されない
Promise.all([promiseA, promiseB, promiseC]).then((results) => {
  console.log(results);
})
  .catch(error => console.log(error)); // rejectされたオブジェクトを返す
// false
~~~
***

## Promise.raceメソッド
allと似ているが【どれかひとつ】でもresolveになったら実行される
~~~
const promise1 = new Promise((resolve) => {
  setTimeout(() => {
    resolve();
  }, 1000);
}).then(() => {
  console.log("promise1おわったよ！");
});

const promise2 = new Promise((resolve) => {
  setTimeout(() => {
    resolve();
  }, 3000);
}).then(() => {
  console.log("promise2おわったよ！");
});

Promise.race([promise1, promise2]).then(() => {
  console.log("どれか一つおわったよ！");
});

// promise1おわったよ！
// どれか一つおわったよ！
// promise2おわったよ！
~~~
***

[参考]（https://qiita.com/cheez921/items/41b744e4e002b966391a）
