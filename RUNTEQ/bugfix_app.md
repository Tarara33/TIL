# リポジトリの取り入れ
ディレクトリ作ったら、そこで
~~~
git clone 取り入れたいリポジトリのSSHでコピーしたURL`
~~~
でクローンさせる
***

# README読む
VSコードか、`cat README.md`で概要を読む
***

# サーバー立ち上げる
railsはサーバーないと動かないため、`rails db:create`でサーバーを作り、    
その後、`rails db:migrate`すると、リポジトリにある内容のmigrationされる
***

# routesの確認
`rails s`でサーバー立ち上げ、`http://localhost:3000`で行ってみる。   
もしrailsのトップページ出るなら`rails routes`でルーティングの確認
***
