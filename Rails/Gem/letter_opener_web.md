# 機能
開発環境で送信したメールをブラウザで確認することができる。    
似ている　Gemで letter_openerがある。[違い](https://rubyandrails.hatenablog.com/entry/letter_opener_web)    
***

# 使い方
## 導入
~~~
[gemfile]
group :development do
 gem 'letter_opener_web'
end

$ bundle
~~~
***

## ルーティング編集
ブラウザ上で確認するため、ルーティングにLetterOpenerWebにアクセスするために必要な記述を追記。
~~~
[config/routes.rb]
if Rails.env.development?
=> 現在のRailsの実行環境がdevelopment環境であるか?

   mount LetterOpenerWeb::Engine, at: '/letter_opener'
=> LetterOpenerWeb::Engine のエンジンを「/letter_opener」というパスに組み込ん(mount)で、
   「localhost:3000/letter_opener」にアクセスすると送信メールが見れるようになる。
 end
~~~
***

# 開発環境ファイルの編集
この二行を追加
~~~
[config/environments/development.rb]
config.action_mailer.delivery_method = :letter_opener_web
=> メールの配信方法を指定していて、 letter_opener_web使うよう設定している。

config.action_mailer.perform_deliveries = true
=> メールの実際の配信を有効または無効にするためのオプション。
   trueに設定されている場合、メールは実際に送信される。
~~~
[細かい設定はこちらを参考に](https://railsguides.jp/configuring.html#action-mailer%E3%82%92%E8%A8%AD%E5%AE%9A%E3%81%99%E3%82%8B)
***

