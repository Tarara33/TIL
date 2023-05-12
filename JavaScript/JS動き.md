# ダイアログ
入力を受け取る (文字列型で受け取る)  
- alert...警告などに使う、OKのみしか押せない
- confirm...確認に使う、OKとキャンセルが押せる
- prompt...入力に使う、OKとキャンセルと入力ができる
~~~
[OK、キャンセルにそれぞれ押した後メッセージを設定する]
const answer = confirm("削除しますか？");
if (answer) {
  console.log("削除しました");
} else {
  console.log("キャンセルしました");
}
~~~
***
