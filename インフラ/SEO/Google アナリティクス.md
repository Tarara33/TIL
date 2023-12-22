# Google アナリティクス
Google アナリティクスは、ウェブサイトやアプリの運営者が、そのサイトやアプリがどのように使われているかを理解するためのツール。
***

### ❓ つまり...??
Googleアナリティクスはウェブサイトやアプリのお医者さんで、  
使い方や人気度、改善のヒントを提供してくれる頼りになるツール！

ウェブサイトやアプリはたくさんの人が使うが、その使い方や訪れる人のことを調べてくれる。  
どのページが人気か、どのくらいの人が訪れたか、どの国や地域から来たかなど、さまざまな情報を教えてくれる。

また、ウェブサイトやアプリが健康かどうかを調べてくれる。  
例えば、たくさんの人が見ているページがあれば、そのページは元気ですね！  
逆に、あまり見られていないページは、より良くなるようにアドバイスをくれる。  
もっとたくさんの人に見てもらうためにはどうしたらいいか、どのページがもっと良くなるかなどを教えてくれる。
***

# アカウント作成
## [Googleアナリティクス](https://analytics.google.com/analytics/web/provision/#/provision)にいく

## アカウント登録
アカウント名には、計測するサイトやアプリを管理する組織の名称を入力する。  
例） 株式会社TCD

[![Image from Gyazo](https://i.gyazo.com/ec1eee249a2624d73440c87113884153.png)](https://gyazo.com/ec1eee249a2624d73440c87113884153)
***

## プロパティ名を入れる
プロパティ名（サイトやアプリの名称）を入力し、「レポートのタイムゾーン」と「通貨」を日本に合わせて「次へ」をクリック。  

プロパティは計測するサイトやアプリごとに作成するため、プロパティ名にサイトの名称やドメイン等を入力すると管理しやすい。
例） TCD公式サイト、tcd-theme.com等

[![Image from Gyazo](https://i.gyazo.com/6158fb9f652737b051f26b5e7a69005c.png)](https://gyazo.com/6158fb9f652737b051f26b5e7a69005c)
***

## サイトの詳細・目標を入れる
次は「業種」、「ビジネスの規模」、「利用目的」を埋めて「作成」をクリック。    
ここでは、運営するサイトやアプリにできるだけ近いものを選択する。
***

## 同意する
[![Image from Gyazo](https://i.gyazo.com/e067753ba9f91bab87daf8e305930f7a.png)](https://gyazo.com/e067753ba9f91bab87daf8e305930f7a)
***

# データ収集の設定
## データーストリームの設定
どこから収集したいのか決める。  
今回は、ウェブサイトのデータを収集するので、「ウェブ」を選択する。
[![Image from Gyazo](https://i.gyazo.com/404a8f0a54f80a97105825bd899fe1bc.png)](https://gyazo.com/404a8f0a54f80a97105825bd899fe1bc)
***

サイトの URLと名称を入れる。

[![Image from Gyazo](https://i.gyazo.com/a871d83970e7301f27e30e49fa0a6db2.png)](https://gyazo.com/a871d83970e7301f27e30e49fa0a6db2)
***

# タグの設定
タグの設定終わってないよと出るので、アプリにタグを入れていく。

[![Image from Gyazo](https://i.gyazo.com/8b567db97c5a8105355c812b6fc98d85.png)](https://gyazo.com/8b567db97c5a8105355c812b6fc98d85)
***

## タグの実装手順を手動でインストールにしてコピーする
[![Image from Gyazo](https://i.gyazo.com/db2c94684df72f4adac43e0c9b7ad534.png)](https://gyazo.com/db2c94684df72f4adac43e0c9b7ad534)
***

## app/views/layouts/applicayion.rbの HEADタグにタグを入れる
HEADタグに直接書いてもいいし、パーシャルにして本番環境なら読み込むというようにしてもいい。

後者の場合
~~~
[app/views/layouts/_google_analytics.html.erb]

<!-- Google tag (gtag.js) -->
<script async src="https://www.googletagmanager.com/gtag/js?id=G-NM6QZG2P9V"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());

  gtag('config', 'G-NM6QZG2P9V');
</script>
~~~
~~~
[app/views/layouts/applicayion.rb]

<!DOCTYPE html>
<html>
  <head>
    <%= display_meta_tags(default_meta_tags) %>
    <meta name="viewport" content="width=device-width,initial-scale=1">
    <%= csrf_meta_tags %>
    <%= csp_meta_tag %>

    <%= stylesheet_link_tag "application", "data-turbo-track": "reload" %>
    <%= javascript_include_tag "application", "data-turbo-track": "reload", defer: true %>

  ⭐️<% if Rails.env.production? %>
      <%= render 'layouts/google_analytics' %>
    <% end %>
  </head>
~~~
***

## デプロイ後計測が始まる
デプロイしてから数分かかった。

[![Image from Gyazo](https://i.gyazo.com/b7e7343c6a3585f9539842f75ec42819.png)](https://gyazo.com/b7e7343c6a3585f9539842f75ec42819)
***
