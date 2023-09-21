# redirect_to　と　redirect_back_or_to
- redirect_to...指定したページに行かせる
- redirect_back_or_to...次のように元々遷移しようとしていたURLにリダイレクトされる    
【redirect_back_or_toを使用したときの処理の流れの例】    
①掲示板一覧ページのURLを入力して遷移する    
②ログインしていなかったため、ログイン画面に強制遷移される   
③ログインする   
④(元々掲示板一覧ページに遷移しようとしてたため)掲示板一覧ページに遷移する    
***

# request.referrer
referrerとは、前のページのURLのこと。  
現在のリクエストを送信する前にブラウザが最後に訪れた URLを表す。 
つまり、ユーザーがどのページから現在のページに来たかを示す情報。
  
~~~
redirect_to request.referrer
~~~
このように書くと、元のページに戻る感じになる。
***

## ❓ redirect_back_or_to と redirect_to request.referrer
どちらも、元のページに戻るが違いとしては、  
デフォルトを指定できる(redirect_back_or_to)かできない(redirect_to request.referrer)か。  

### redirect_back_or_to
指定されたデフォルトの URLにリダイレクトする代わりに、    
リクエストのリファラー（request.referrer）が存在する場合にはリファラーの URLにリダイレクトする。  
リファラーが存在しない場合にはデフォルトの URLにリダイレクトする。
***

### redirect_to request.referrer
request.referrer を直接使用して、現在のリクエストのリファラー（前のページの URL）にリダイレクトする。    
リダイレクト先をデフォルトの URLではなく、必ずリファラーに設定したい場合に使用。
***
