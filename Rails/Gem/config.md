# 機能
環境ごとに違う定数を使える。    
ここでいう環境とは、ローカル環境・開発環境・本番環境・テスト環境など。        
    
たとえばホストの情報を管理したい時などに使える。    
メールを送る下準備としてホスト情報を設定しておく必要があるが、    
ホスト情報は開発環境、ステージング環境、本番環境で異なる。    
    

- 開発環境...localhost:3000    
- ステージング環境...localhost:3000    
- 本番環境...exumple.jp

***

# 使い方
## 導入
~~~
[gemfile]
gem 'config'
$ bundle
~~~
***

## configファイルの生成
~~~
$ rails g config:install
~~~
作成されたファイルと役割    
[![Image from Gyazo](https://i.gyazo.com/4478d915fee1d7a129cc41fe79a15cfe.png)](https://gyazo.com/4478d915fee1d7a129cc41fe79a15cfe)
***

## 試しに設定
