[公式](https://github.com/kjvarga/sitemap_generator)  
[参考](https://qiita.com/momo1010/items/27fa9290d0a8ed79bc24)

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
 # ⭐️ルートページ
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

##### ⭐️ ルートパス
root_path はデフォルトで追加されているので書かなくてもOK
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
items#show ページは一意のアイテムごとに異なる URLを持つため、アイテム数分だけ addメソッドでサイトマップに追加する必要がある。  
一つ一つ addするのは大変なので each文で回す。
~~~
[config/sitemap.rb]

# items#show
Item.find_each do |item|
  add item_path(item), 🧡lastmod: item.updated_at
end
~~~
##### 🧡 lastmod
サイトマップエントリにおいて各ページの最終更新日を指定している。    
これにより、検索エンジンに対して各ページが最後に変更された日時を提供している。  

この場合検索エンジンに対して、各アイテムが最後に変更された日時を伝えている。
***

# サイトマップの更新
ページが増えたらサイトマップを更新する必要がある。  
アプリ自体のページが増えなくても、例えば投稿型などならユーザーが新しい投稿するたびに urlが増えるので更新する必要がある。  
~~~
$  rails sitemap:refresh
~~~
***

# サイトマップの自動更新
サイトマップは定期的に更新する必要があるので自動更新されるようにする場合。

## [whenever](https://github.com/Tarara33/TIL/blob/main/Rails/Gem/whenever.md)で更新
スケジュールファイルに以下のようにかく。
~~~
[config/schedule.rb]
every 1.day, at: '3:00 am' do
  rake 'sitemap:refresh'
end
~~~
***

## サイトマップ生成のトリガーをアプリ内のイベントにする
サーバーによっては cronジョブはお金かかることあるので、直接コードに書くのもアリ。

例えば、items#showもサイトマップに入れるなら、itemが作成、更新するたびにサイトマップを更新するようにする。  
その場合、itemsコントローラーの create/updateアクション内にサイトマップ更新コードをかく。
~~~
[items_controller.rb]

def create
@item = current_user.items.new(item_params)

  if @item.save
    ⭐️regenerate_sitemap
    redirect_to item_path(@item), success: t('.success')
  else 
    flash.now[:danger] = t('.fail')
    render :new, status: :unprocessable_entity
  end
end

private
  ⭐️サイトマップを再生成するメソッド
  def regenerate_sitemap
    ①SitemapGenerator::Interpreter.run
    ②SitemapGenerator::Sitemap.ping_search_engines
  end
~~~
##### ① SitemapGenerator::Interpreter.run
「config/sitemap.rb」に設定した内容に基づいてサイトマップを生成するコードを実行するメソッド。

##### ② SitemapGenerator::Sitemap.ping_search_engines
生成されたサイトマップを検索エンジンに通知するためのメソッド。  
オプションなのつけなくてもいいけど、SEOには有効かもしれない。
***

# ローカル環境でサイトマップを確認する
`http://localhost:3000/sitemap.xml.gz`にアクセスして解凍した xmlファイルを VSCodeなどで見る。
***

# ❓ サイトマップが本番サーバーで見れない
`$  rails sitemap:refresh`して出てきた URLに飛んだものの、404エラーで見れない。  
=> そのコマンドを VSCodeなど、ローカル環境でしても本番サーバーでは反映されないから！

例えば [render](https://github.com/Tarara33/TIL/tree/main/%E3%82%B5%E3%83%BC%E3%83%90%E3%83%BC/render)でデプロイしてるなら、環境構築用ファイルにこのコマンドを書く必要がある。
~~~
[bin/render-build.sh]

#!/usr/bin/env bash
# exit on error
set -o errexit

bundle install
bundle exec rake assets:precompile
bundle exec rake assets:clean
bundle exec rake db:migrate
⭐️bundle exec rails sitemap:refresh
~~~
これでデプロイする！
***

## ❓ ダウンロードしちゃうけどいいの？
ローカルURLや、本番URLでサイトマップを開いた時に 圧縮ファイル形式(.gz)だからダウンロードしちゃうけどいいの？  
=> これは Googleなどの検索エンジンが認識できる形式だから、ダウンロードされるのは問題ない。  
ただ、人間が内容を確認する場合は、ダウンロードした.gzファイルを解凍して.xmlにする必要があるので、解凍して VSCodeなどで見る！  

検索エンジンがサイトマップをちゃんと読み取れているかどうかは、  
サーチコンソールの[「サイトマップ」](https://github.com/Tarara33/TIL/blob/main/%E3%82%A4%E3%83%B3%E3%83%95%E3%83%A9/SEO/%E3%82%B5%E3%82%A4%E3%83%88%E3%83%9E%E3%83%83%E3%83%97.md)セクションで確認して「成功」なら大丈夫。
***
