# Swiper
スライダー機能(画像の切り替わりが使える)プラグイン。  
似たようなプラグインに slickというものがある。  
二つの違いは jQueryに依存する(slick)かしない(swiper)かである。
***

# ダウンロード
三種類の方法がある。

## CDNを使う場合
CDNとは「Content Delivery Network（コンテンツデリバリーネットワーク）」の略称で、  
外部ファイルをリンク1つで使用することができる仕組みのこと。  
  
- [公式サイト](https://swiperjs.com/)のヘッダーにある「Docs」から「Getting Started」を選択。
- 「Use Swiper from CDN」の目次を選んでそこのコードを swiperを使いたいファイルの headタグに貼り付ける。
~~~
<link
  rel="stylesheet"
  href="https://cdn.jsdelivr.net/npm/swiper@10/swiper-bundle.min.css"
/>

<script src="https://cdn.jsdelivr.net/npm/swiper@10/swiper-bundle.min.js"></script>
~~~
***

## npmを使う場合
