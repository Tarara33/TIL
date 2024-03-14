# retry
例外処理を beginからもう一度行う。  

使う場面はネットワークエラーのように一時的なエラーが起きそうな場所や、  
APIからデータを取得する処理で一時的なエラー（タイムアウトや一時的な接続エラーなど）が発生した場合。  
その場合、エラーを捕捉して少し待ってからもう一度同じ処理を試みるために retryを使うことがある。
しかし、例外が解決しない限り無限ループになることもあるので、カウンターつけた方がいい。
~~~
retry_count = 0
begin
  1 / 0
rescue 
  retry_count += 1
  if retry_count <= 3
    puts "retryします。 #{retry_count}回目"
    retry
  else
    puts "retry上限に達しました。"
  end
end

【実行結果】
retryします。 1回目
retryします。 2回目
retryします。 3回目
retry上限に達しました。
~~~
***
