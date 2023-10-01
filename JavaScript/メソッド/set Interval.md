# set/clear Interval()
指定した時間毎に繰り返し指定した関数を呼びだすことができる
setInterval(関数, 時間(1000で１秒))   
⚠️渡す関数に()つけない！callback関数と同じで実行結果が欲しいわけではなく関数の処理をしてほしいため    
clearIntervalで止める
~~~
let i = 0;
function showTime() {
  console.log(new Date());
  i ++;
  if (i > 2) {
  clearInterval(timer)
  }
}
const timer = setInterval(showTime, 1000); 
~~~
clearしないと永遠なので、もし３回showTime関数をしたら止めるというコード
***
