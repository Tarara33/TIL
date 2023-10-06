[参考](https://zenn.dev/donchan922/articles/b08a66cf3cbbc5#%E7%92%B0%E5%A2%83)  

# Node.jsとは
Node.jsとはサーバサイド側で動く JavaScriptのこと。  
  
ダウンロード方法は
- nodenvを使う  
- nodeのサイトからダウンロード  

などなど。
***

## 自分の環境
nodenvを使って Node.jsをダウンロードしている。
~~~
$ where node
=> /Users/Tarara/.nodenv/shims/node
~~~
~~~
$ node -v
=> v15.14.0
~~~
***

# 👀 バージョン確認
基本的に、Node,yarnなどは一度システムにダウンロードしたらそれで OK！  
ただ、バージョンを変えたいときは指定バージョンを入れて使う必要がある。 

## nodenv自体のバージョン確認
~~~
$ nodenv -v
=> nodenv 1.4.1
~~~
***

## nodenvで管理している Node.jsのバージョン確認
~~~
$ node -v
=> v15.14.0
~~~
***

## すでに持っている Node.jsのバージョン確認
~~~
$ nodenv versions
* 15.14.0 (set by /Users/tarara/.nodenv/version)
  16.16.0
  16.17.0
  18.15.0
  18.16.0
~~~
⚠️ 私の場合　nodenv使って管理しているので「node vertions」だと一覧でない。
***

# 🔧 新しくバージョンをインストールする
## インストールできる一覧見たい時
~~~
$ nodenv install -l
=> 0.1.14
   0.1.15
   0.1.16
   0.1.17
　　　　　　...省略
~~~
***

## インストールするバージョンを指定してインストールする
~~~
$ nodenv install 16.3.0
$ nodenv rehash   #nodenvに認識させる
~~~
***

# 🔄 バージョンの切り替え
~~~
[ローカル（カレントディレクトリ配下）で利用する Node.jsのバージョンを設定する]
$ nodenv local 15.14.0

[グローバル（システム全体）で利用する Node.jsのバージョンを設定する]
$ nodenv global 16.3.0
~~~
***
