# Google Search Console
ウェブサイトが Googleの検索結果でどのように表示されているかを理解するためのツール。  
検索クエリ、サイトの検索順位、クリックスルーレート、インデックスの問題などをチェックできる。
***

### ❓ つまり何がわかる？
例えば、「リンゴ天国」という色んな種類のリンゴを載せているサイトがある場合 Google Search Consoleを使うと、  
ユーザーたちは Googleで何と検索してこの「リンゴ天国」を見つけているのかがわかる。  
「リンゴ」で検索された、「果物」で検索された、「赤い」で検索されたなど...。  

また、検索結果の中での順位もわかる。  
「リンゴ」で検索された場合、検索結果一覧の134番目にあるよ！みたいな。

また、「リンゴ天国」サイトの中でどのページが人気なのかも知ることができる。  
「ジョナゴールド」のページが人気とか、「ふじリンゴ」のページが人気とか。  

また、ウェブサイトに問題があるかどうかも教えてくれる。  
例えば、ページが見つからない場合や、検索結果に表示されない場合など、改善すべき点を知ることができる。
***

# 設定方法[(参考)](https://www.onamae.com/column/howto/2/)
今回は お名前.comにて独自ドメイン取得をして、それを使って設定する。  
Googleアカウント登録は割愛。

# Google Search Consoleのサイトに行く
[Google Search Console](https://search.google.com/search-console/about?hl=ja)に行って、「今すぐ開始」を押す。  

そうすると「ドメイン or URL プレフィックス」が選べるので、「ドメイン」を選択。  
[![Image from Gyazo](https://i.gyazo.com/30e6fc6720055de8da95cbe21dd2d72f.png)](https://gyazo.com/30e6fc6720055de8da95cbe21dd2d72f)

すると、レコードタイプが選べるので、「TXT」にして、「TXTレコード」をコピーする。
[![Image from Gyazo](https://i.gyazo.com/0c64229c5bff4d50c7e277a0abd3159e.png)](https://gyazo.com/0c64229c5bff4d50c7e277a0abd3159e)
***

## お名前.comに移動
DNS設定にいく。  
[![Image from Gyazo](https://i.gyazo.com/3a9fe9d05849e38f0452dfedff7fef81.png)](https://gyazo.com/3a9fe9d05849e38f0452dfedff7fef81)

「DNSレコードの設定を利用する」の「設定する」をクリック。
[![Image from Gyazo](https://i.gyazo.com/50112a75553b69bf6d01dc0850f8b11a.png)](https://gyazo.com/50112a75553b69bf6d01dc0850f8b11a)

ホスト名は空欄、タイプ「TXT」を選択して VALUEに先ほどコピーした TXTレコード入れて追加する。
[![Image from Gyazo](https://i.gyazo.com/f9f7aa3a0353e6a48f13c013eefccdbf.png)](https://gyazo.com/f9f7aa3a0353e6a48f13c013eefccdbf)

  
画面下の「確認画面へ進む」をクリック。  
設定変更の確認画面が表示されるので「設定する」をクリック。  

Google Search Consoleの画面に戻り、DNSの変更が浸透するまで待つ。  
「確認」をクリックして「所有権を証明しました」の画面が表示されれば登録は完了!
[![Image from Gyazo](https://i.gyazo.com/cf60589cffbfda99e14bf010853d0f4e.png)](https://gyazo.com/cf60589cffbfda99e14bf010853d0f4e)
***

