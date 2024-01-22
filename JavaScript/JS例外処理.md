# try...catch
~~~
try {
  例外が起こりうる処理
} catch {
  例外が起こった場合の処理
}
~~~
***

## 例: 文字を大文字にする処理
~~~
const name = "ringo"

console.log(name.toUpperCase());
console.log("Finish!");

// RINGO
//Finish!
~~~

もしも、定数 nameに文字列以外を入れるとエラーになり、Finish!は表示されない。
~~~
const name = 5

console.log(name.toUpperCase());
console.log("Finish!");

//TypeError: name.toUpperCase is not a function
~~~

途中で止めずに処理したい時に例外処理を作る。
~~~
const name = 5;

try {
  console.log(name.toUpperCase());
} catch(e) {
  console.log(e);
}

console.log("Finish!");

//TypeError: name.toUpperCase is not a function
//Finish!
~~~
catchに引数を渡しているとそこにエラーの情報を受け取ってくれる。     
console.logでエラー内容を表示したい時とかに使える。
***

# throw
わざと例外を発生させる。
~~~
throw 処理
~~~

~~~
const name = 5;

try {
  if (typeof name === 'number') {
    throw "数字が入力されています";
  }
  console.log(name.toUpperCase());
} catch(e) {
  console.log(e);
}

console.log("Finish!");

//数字が入力されています
//TypeError: name.toUpperCase is not a function
//Finish!
~~~
***

# finally
例外発生の有無にかかわらず必ず実行される処理。
~~~
const name = 5;

try {
  if (typeof name === 'number') {
    throw "数字が入力されています";
  }
  console.log(name.toUpperCase());
} catch (e) {
  console.log(e);
} finally {
  console.log("ご利用ありがとうございました");
}

//数字が入力されています
//TypeError: name.toUpperCase is not a function
//ご利用ありがとうございました
~~~
***
