# set/clear Timout
指定した時間のあとに１回だけ処理を実行するように予約する命令    
setTimeout(関数, 予約時間(1000で１秒))  
~~~
function showTime() {
  console.log(new Date());
}

setTimeout(showTime, 1000);
~~~
Intervalように繰り返しもできる
~~~
 let i = 0;
function showTime() {
  console.log(new Date());
  const timer = setTimeout(showTime, 1000);
  i ++;
  if (i > 2) {
  clearTimeout(timer)
  }
}
showTime();
~~~
***
