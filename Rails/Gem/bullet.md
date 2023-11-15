[公式](https://github.com/flyerhzm/bullet)

# bulleet
[N+1](https://github.com/Tarara33/TIL/blob/main/Rails/%E3%83%87%E3%83%BC%E3%82%BF%E3%83%BC/N%2B1.md)問題が発生している箇所があったら教えてくれる gem。 

ただ、全てを見つけるわけではないので、頼りすぎは良くない！
***

# 使い方
## Gem導入
~~~
[gemfile]
group :development do
  gem 'bullet'
end

$ bundle
~~~
***

## インストール
~~~
$ rails g bullet:install
~~~
インストールすると「テスト環境にも bulletを入れますか？」と質問されるが、今回は noにして見送った。
***

# config/environments/development.rbの編集
先ほどのコマンドで「config/environments/development.rb」にコードが追加される。
~~~
[config/environments/development.rb]

Rails.application.configure do
  config.after_initialize do
    ①Bullet.enable        = true
    ②Bullet.alert         = true
    ③Bullet.bullet_logger = true
    ④Bullet.console       = true
    ⑤Bullet.rails_logger  = true
    ⑥Bullet.add_footer    = true
  end
~~~

### ① 
Bulletの gemを利用可能にする。  
ただ、これだけ trueでも何も起こらない。  
可能にするだけ。
***

### ②
ブラウザにJSのアラートを出す。
こんな感じで N+1が発生したところで出してくれる。

[![Image from Gyazo](https://i.gyazo.com/9226df0b2e862a4892514886596030fe.png)](https://gyazo.com/9226df0b2e862a4892514886596030fe)
***

### ③
「Rails.root/log/bullet.log」に bulletのログファイルを出す。 
***

### ④
console.logに警告を出す。
検証のコンソールにこんな感じで出してくれる。

[![Image from Gyazo](https://i.gyazo.com/436ba58cbfa3305721ac0fc9fe34fca8.png)](https://gyazo.com/436ba58cbfa3305721ac0fc9fe34fca8)
***

### ⑤
railsのログに警告を出す。
***

### ⑥
画面左下にメッセージを出す。
こんな感じ。  

[![Image from Gyazo](https://i.gyazo.com/0b65afacdea1832a9c10da980ce68abf.png)](https://gyazo.com/0b65afacdea1832a9c10da980ce68abf)
***
