[参考](https://zenn.dev/airiswim/articles/040640884120ab)

# 環境別で seedファイルを分ける
例えば、開発環境と本番環境で入れたい初期情報が違う時などに便利。
***

# やり方
## 環境別のファイル作成
db/配下に`seeds`ディレクトリを追加して、中に`development.rb`、`production.rb`などを作る。  
なので、元からある seeds.rbファイルと合わせると
- db/seeds.rb
- db/seeds/development.rb
- db/seeds/production.rb

ができる。
***

## 環境ファイルの編集
それぞれ環境ごとに作りたい情報を入れる。
~~~
[db/seeds/development.rb]

User.create!(
  name: "花子",
)


[db/seeds/production.rb]

User.create!(
  name: "太郎",
)
~~~
***

## db/seeds.rbの編集
~~~
[db/seeds.rb]

load(Rails.root.join("db", "seeds", "#{Rails.env.downcase}.rb"))
~~~
Rails.root.joinメソッドは、  
Railsアプリケーションのルートディレクトリを基準として、指定された相対パスを絶対パスに変換する。

そして、loadメソッドを使用してその絶対パスにあるファイルを読み込み込む。

なので、環境(Rails.env)が `Development`なら、  
`db/seeds/development.rb`となる。
***

## 最後に
これらの設定をしたらターミナルで `$ rails db:seed`を実行するとそれぞれの環境の seedファイルが読み込まれる。 

先ほどの例だと、開発(development)なら User　"花子"が作られて、  
本番(production)環境だと User　"太郎"が作られる。
***
