# Turbo Streams
#### ⭐️ おさらい
Turbo Streamsは画面の**複数箇所**を更新する。

[![Image from Gyazo](https://i.gyazo.com/cc6dc21433c614744b711d7bad95c685.png)](https://gyazo.com/cc6dc21433c614744b711d7bad95c685)
***

# 書き方
## ヘルパーメソッドの場合
`<%= turbo_stream.アクション名 idやオブジェクト名 do %>`を使う。
***

## HTMLタグの場合
~~~
<turbo-stream action="アクション名" target="操作するHTML要素のid">
  <template>
    コンテンツ
  </template>
</turbo-stream>
~~~
***

# アクション一覧
詳しくはこちら([参考](https://qiita.com/NaokiIshimura/items/d91b10587881107b5f39))

[![Image from Gyazo](https://i.gyazo.com/a56854b5dc4b5b1bff44fe36c1394dc4.png)](https://gyazo.com/a56854b5dc4b5b1bff44fe36c1394dc4)

## 追加
- append: targetの末尾に追加
- prepend: targetの先頭に追加
- before: targetの前に追加
- after: targetの後に追加

## 更新
- replace: targetを更新（target要素も含めて更新）
- update: targetを更新（target要素のコンテンツだけ更新）

## 削除
- remove: targetを削除
***

# 更新処理の流れ
1. クライアント側  
`<%= turbo_frame_tag %>`内にある、Turboがフォームの「登録・更新ボタン」を押すと、  
フォームからのリクエストの場合、Acceptリクエストヘッダーに turbo_streamフォーマットを追加して fetchする。  
具体的には`Accept: text/vnd.turbo-stream.html`, `text/html, application/xhtml+xml`となる。  

[![Image from Gyazo](https://i.gyazo.com/06fd3f3289b1e23c90cef88bcf460f7a.png)](https://gyazo.com/06fd3f3289b1e23c90cef88bcf460f7a)

2. サーバー側  
Acceptヘッダーの`text/vnd.turbo-stream.html`により、フォーマットが turbo_streamになる。  
そのため renderで update.turbo_stream.erbなど、拡張子が`.turbo_stream.erb`のビューを利用する。  
リクエスト形式も TURBO_STREAMになっていた。

[![Image from Gyazo](https://i.gyazo.com/10b828b1422ab3276f4f8edec5c343d1.png)](https://gyazo.com/10b828b1422ab3276f4f8edec5c343d1) 

3. クライアント側  
レスポンスされた`<turbo-stream action="replace" target="cat_1">...</turbo-stream>`を  
Turboが処理して、target部分の HTML要素(例: cat_1)を<template>要素のコンテンツでactionの内容(例: replace)する。
***

# ⚠️ 必ずしもフォーマットが Turbo Streamになるわけではない
フォーマットが Turbo Streamになるのは、フォームから **GET以外の HTTPメソッドが利用された時**だけ。    
リンクやフォームから GETする場合には Turbo Streamにはならない。    
Scaffoldが用意する7アクションに関して言うと、  
index/show/edit/newの4つは GETなので Turbo Streamsは使えなくて、  
update/create/destroyは GETではないので Turbo Streamsが使える、という感じになる。

## Turbo Frames
- index
- show
- edit
- new
- update（バリデーションエラー時）
- create（バリデーションエラー時）

## Turbo Streams
- update（成功時）
- create（成功時）
- destroy（成功時）
***
