# axiosとは
HTTP通信（データの更新・取得）を簡単に行うことができる、JavaScriptのライブラリ。    
APIを提供するクラウドサービスに対して、データを送受信することができる。
***

## インストール
~~~
$ npm install axios
~~~
***

## CDNの使用
CDNは「Content Delivery Network」の略で、コンテンツを効率良く配信するためのネットワークのこと。    
Webサイトの画像やJavaScript、CSSなどの静的リソースをサーバーから取得する代わりに、    
CDNを通して取得することで、ユーザーがリソースを早く読み込めるようになる。    
使用するライブラリによって変わるので今回はaxiosのCDN。
~~~
<script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
~~~
💡 HTMLファイルに書く
***

# メソッド紹介
使えるようになるメソッドをいくつか紹介

## axios.get(url)
HTTP通信(API通信)でサーバーからデータを取得するには、axios.get()関数を使用することで実現可能。
~~~
// GETリクエスト（通信）
const url = axios.get("http://localhost:3000/")

    // thenで成功した場合の処理
    .then(() => {
        console.log("ステータスコード:", status);
    })
    // catchでエラー時の挙動を定義
    .catch(err => {
        console.log("err:", err);
    });

結果
ステータスコード：200　//http通信でデータ取得の成功
~~~
***

## axios.post(url)
データをサーバーへ情報を送るときは、axios.post()メソッドで可能。
~~~
//POSTリクエスト（通信）
const data = { firstName : "Taro", lastName : "Yamada" }
const url = axios.post("http://localhost:3000/user/123", data)

        .then(() => {
            console.log(url)
        })

        .catch(err => {
        console.log("err:", err);
        });

結果
Yamada Taro //サーバーへYamada Taroを送信
~~~
***

## axios.delete(url)
データを削除する場合は、axios.delete()メソッドを使う。    
ただし、データIDで識別するため、IDを指定しないといけない。
~~~
const url = axios.delete("http://localhost:3000/user/123")

        .then(() => {
            console.log('削除ID:',url)
        })

        .catch(err => {
        console.log("err:", err);
        });

結果
削除ID:123 
~~~
***

## axios.put(url)
データを更新する時は、axios.put()メソッドを使いますが、こちらもIDを指定しないといけない。
~~~
const data = { firstName : "Taro", lastName : "Qiita" }
const url = axios.put("http://localhost:3000/user/123",data)

        .then(() => {
            console.log('データ更新:',url)
        })

        .catch(err => {
        console.log("err:", err);
        });

結果
データ更新：Qitta Taro
~~~
***

# configオブジェクト
configオブジェクトは、多くのJavaScriptライブラリやフレームワークで見られる一般的な概念。    
特にHTTPリクエストを送信する際に使用されるオブジェクト。    
    
configオブジェクトは、HTTPリクエストの設定やオプションを含むオブジェクト。    
これにより、リクエストの詳細なカスタマイズや特定の動作を指定することができる。
~~~
const axios = require('axios');

const config = {
  method: 'get',
  url: 'https://api.example.com/data',
  headers: {
    'Authorization': 'Bearer xxxxxxxx' // トークンを設定する例
  }
};

axios(config)
  .then(response => {
    console.log(response.data);
  })
  .catch(error => {
    console.error(error);
  });
~~~
***


