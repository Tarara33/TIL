# Postman
Postmanは、API開発者やテストエンジニアが APIをテストするためのツール。  
Postmanは、APIのエンドポイントを呼び出し、リクエストを送信し、レスポンスを受け取り、解析するための簡単で直感的なインターフェイスを提供する。  
Postmanは、さまざまな HTTPリクエストを作成し、編集し、送信できるため、APIテストに必要なすべての機能を備えている。

たとえ APIを使わないアプリでも、リクエスト確認したいとき Postmanが役立つこともある!  
Googleの検証みたいにも使える。
***

# 使い方
## ワークスペースの作成
[![Image from Gyazo](https://i.gyazo.com/a80dc504a183223ede74c27f3007cdbd.png)](https://gyazo.com/a80dc504a183223ede74c27f3007cdbd)
***

## リクエストタブを開く
＋を押してタブを出す。

[![Image from Gyazo](https://i.gyazo.com/434cc95b55ee0064a385dd187144d696.png)](https://gyazo.com/434cc95b55ee0064a385dd187144d696)
***

# リクエストを送信
⚠️ localhostでのリクエスト送信は webサイト版だとできず、デスクトップ版ならできる(2024年1月現在)

## GETリクエスト
HTTPリクエストに GETを選択して URLを送信。  
下側にそのURLを送信した結果、返ってくるものが表示される。  

例えば、APIのリクエストで返ってくるものが見たいとき。
[![Image from Gyazo](https://i.gyazo.com/bea589c3babb24cdd3f85717bf2d59a7.png)](https://gyazo.com/bea589c3babb24cdd3f85717bf2d59a7)
***

### +a
普通の webアプリで例えば「トップページのリクエストで何が返ってくるか」をみたいとき、  
URLを入れれば検証画面のようなコードが返ってくる。

[![Image from Gyazo](https://i.gyazo.com/21928a5a43ee45476d343db54cdb2355.png)](https://gyazo.com/21928a5a43ee45476d343db54cdb2355)
***

## POSTリクエスト
フォームの入力値をクエリパラメーターとして渡すこともできるが、セキュリティ上良くない。  
そんなときは 「Body」の「form-data」部分を押してから、キーと値の書き方でフォームの入力値を書く。

[![Image from Gyazo](https://i.gyazo.com/3442f968f9c8e243c4b7b7fc16bd1011.png)](https://gyazo.com/3442f968f9c8e243c4b7b7fc16bd1011)
***
