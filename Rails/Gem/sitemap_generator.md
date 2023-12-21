# sitemap_generator
サイトマップを作る gem。  
サイトマップってのは、ウェブサイトの地図みたいなもの。  
ウェブサイトにはたくさんのページがあるけど、サイトマップにはそのすべてのページがどこにあるかが書いてある。
***

# 使い方
## Gem導入
~~~
[gemfile]
gem 'sitemap_generator'

$ bundle
~~~
***

## 設定ファイルの作成
~~~
$ rails sitemap:install
~~~
これで「config/sitemap.rb」ができる。
***

## サイトマップの保存ファイルを作成
~~~
$ rails sitemap:create
~~~
作成したサイトマップはここに保存される。
***

## gitignoreの追加
サイトマップはサイズが大きくなりやすいので sitemap.xml.gzは git管理から外しておく。
~~~
[.gitignore]
#  サイトマップファイル
public/sitemap.xml.gz
~~~
***

# config/sitemap.rbの編集
このファイルでは、サイトマップを作成するためのロジックや、各ページの優先度や変更頻度などの情報を書く。  

例えば、config/routes.rbが以下の内容のとき
~~~
[config/routes.rb]

Rails.application.routes.draw do
  root   'homes#top'
  get    'login',             to: 'user_sessions#new'
  post   'login',             to: 'user_sessions#create'
  delete 'logout',            to: 'user_sessions#destroy'
  get    'privacy_policy',    to: 'homes#privacy_policy'
  get    'terms_of_service',  to: 'homes#terms_of_service'

  resources :users, only: %i[new create]
  resources :items do
    get :search, on: :collection
  end
  resources :favorites, only: %i[create destroy]
end
~~~
***

サイトマップの設定はこのようにする。  
#### ⭐️ サイトマップに書くページの条件
- ウェブサイトの公開されているコンテンツやページを書く(ログインしてないと見れないページなどは含めない！)
- indexアクションなど Viewがあるページのパスを書く。(createなどページがないのは含めない)

今回の routingアクションではログインしていないと、   
ユーザー登録画面と アイテム一覧ぐらいしか見れないので以下のようになる。
~~~
[config/sitemap.rb]

🌞SitemapGenerator::Sitemap.default_host = "http://gift-compass.com"

SitemapGenerator::Sitemap.create do
 # ルートページ
  add root_path, 🩵priority: 1.0, 💚changefreq: 'daily'

  # 静的ページ
  add privacy_policy_path, changefreq: 'yearly'
  add terms_of_service_path, changefreq: 'yearly'

  # ユーザー登録ページ
  add new_user_path, changefreq: 'monthly'

  #ログインページ
  add login_path, priority: 0.7, changefreq: 'monthly'

  # アイテム一覧ページ
  add items_path, priority: 0.7, changefreq: 'daily'
end
~~~
##### 🌞 サイトのホストを書く。
***

##### 🩵 priority(ページの優先度)
パラメータが、「0.0 から 1.0」 の範囲で指定され、そのページの相対的な重要度を示す。(デフォルトの優先度は通常 0.5)    
値が 1.0に近いほど高い優先度を示し、検索エンジンはそのページをより重要視する。  
※ ただし、これは検索エンジンがこれを必ずしも遵守するものではない。  
***

💚 changefreq（変更頻度）
そのページがどれくらいの頻度で変更されるかを示す。  
有効な値としては、
- always...常に変更
- hourly...毎時間変更
- daily...毎日変更
- weekly...毎週変更
- monthly...毎月変更
- yearly...毎年変更
- never...ほとんど変更なし
***

## ❓ アイテム詳細ページも見られる場合は？？
もし、ログインしなくてもアイテム一覧ページが見られる場合の書き方は？？
１ページではなく、商品ごとにページを追加する必要があるので
~~~
[config/sitemap.rb]

