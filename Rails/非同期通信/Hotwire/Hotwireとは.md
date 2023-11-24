[参考](https://zenn.dev/shita1112/books/cat-hotwire-turbo/viewer/abstract)  
  
# Hotwireとは
Rails7から Railsのデフォルトになった、モダンな Webアプリケーションを作るための新しいアプローチ。  
Hotwireを使うと、フォーム・リンクからのリクエストは全て Fetch APIを利用した非同期リクエストになる。  
この fetchに対して、サーバーは HTMLをレスポンスする。
  
- Turbo  
Hotwireの中核となる JavaScriptのライブラリ

- Stimulus  
Turboと相性が良い JavaScriptのライブラリ

- Strada  
Hotwireを使ってモバイルアプリケーションを開発する際に利用するライブラリ
  
という3つの技術から構成される。  
***

### ❓ Ajaxとの違い
どちらもサーバーからデータを非同期に取得するための手段。  
  
Hotwireは HTMLを直接返すのに対して、  
Ajaxは JSONなどのデータ形式を返す点で違う。  
また、Hotwireは Turbolinksや Stimulus.jsなどのフレームワークを組み合わせて、    
SPA(シングル　ページ　アプリケーション)のようなユーザ体験を提供する。  
***

# Turbo
Turboを使うと JavaScriptを書かずに SPA風のアプリケーションを実現できるようになる。
Turboは以下の3つの技術から構成される。

- Turbo Drive
body要素全体を置換する。

- Turbo Frames
body要素の一部のみを更新する（特定の HTMLタグなど）

- Turbo Streams
`Turbo Frames`が一箇所のみの更新なのに対し、複数箇所更新できる

- (Turbo Native)
これは Nativeアプリとのブリッジで利用するもので、Web開発では利用しない。
***

# 使い方
## 導入
JSのライブラリ importmapか esbuildで導入が違う。  
rails7系は デフォルトで gem入ってるので特にインストール必要ないが、必要なものはこんな感じ。
~~~
💛 importmapの場合
[Gemfile]
gem 'importmap-rails'
gem 'turbo-rails'
gem 'stimulus-rails'


🧡 esbuildの場合
[Gemfile]
gem 'turbo-rails'
gem 'stimulus-rails'
+
[app/javascript/application.js]
import "@hotwired/turbo-rails"
~~~
***

# コードの書き方
書き方は何通りかあるので[こちら](https://github.com/Tarara33/TIL/tree/main/Rails/%E9%9D%9E%E5%90%8C%E6%9C%9F%E9%80%9A%E4%BF%A1/Hotwire/%E6%9B%B8%E3%81%8D%E6%96%B9)で。
***
