# 環境構築の時の DB作り方
##  rails db:create → rails s(サーバー起動)
データベースの作成のみを行う。      
つまり、rails sしても直近の DBの状態は反映されてない状態でサーバーが立ち上がる。  
マイグレーションや seedデータのロードは別途行う必要がある。  
  
最新状態にしたいなら、以下の順でする。
~~~
$ rails db:create
$ rails db:migrate
$ rails db:seed
$ rails s
~~~
***

## rails db:setup → rails s(サーバー起動)
アプリケーションのDB周りの初期設定をしてくれる。     
rails sすると、最新状態の　DBが反映される。  
以下３つのコマンドを実行してくれる。  
~~~
$ rails db:create   
$ rails db:schema:load    
$ rails db:seed   
~~~
***
