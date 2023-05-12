# try...catch
tryに例外が起こりうる処理
catchに例外が起きた時の処理
~~~
const name = "ringo"
console.log(name.toUpperCase());
console.log("Finish!");
~~~
もしも、定数nameに文字列以外を入れるとエラーになり、Finish!は表示されないが    
途中で止めずに処理したい時に例外処理を作る
~~~
const name = 5;
try {
  console.log(name.toUpperCase());
} catch(e) {
  console.log(e);
}
 console.log("Finish!");
 //エラーメッセージの後にFinish!が出る
 ~~~
 catchに引数を渡しているとそこにエラーの情報を受け取ってくれる    
 console.logでエラー内容を表示したい時とかに使える
 ***
