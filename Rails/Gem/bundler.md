[公式](https://github.com/rubygems/bundler)

# 機能
gemの依存関係とバージョンを管理するためのツール。    
複数のジェムのパッケージみたいな。
***

# 使い方
## Gem導入
~~~
＄ gem install bundler

インストール後
$ rbenv rehash
~~~
rehashが何をやっているかというと、    
新規にインストールされたbundlerの実行ファイルへの実行フォルダをrubyが気にすることのないようにしている。
***

# bundler自体のアップデート
- 現在のバージョン確認
~~~
$ bundle -v
~~~
複数あると、一番最新がデフォルトになるっぽい。
***

- 現在持っているバージョン確認
~~~
$ gem list bundler

*** LOCAL GEMS ***

bundler (2.3.26, 1.16.2)
~~~
***

- アップデート(バージョン指定してインストール)
~~~
$ gem install bundler -v バージョン
~~~
***

# Bundleコマンド
gemファイルインストールするときにターミナルで打つやつ。    
bundlerをインストールすることで使えるようになる。    
[詳しくはここに](https://ruby.studio-kingdom.com/bundler/bundle_install/)
***

# 種類
## Gemfile作る
~~~
$ bundle init
~~~
実行したフォルダまたはディクレトリに管理ファイル「Gemfile」が作られる。
***

## Gemインストールする
~~~
$ bundle install
$ bundle
~~~
Gemfileに記載されたGemをインストールする
***

### オプション
- 指定グループをインストール対象から外す
~~~
--without=<list>

[例]
＄ bundle install --without=development
~~~
Gemfileに記載している developmentグループ内の Gemをインストールしない。    
💡この --withoutオプションは記憶されるため、    
次にbundle installを実行した時は　--withoutオプションがなくても先に指定したgroupのGemをインストールしない。
***

## Gemのアップデート
~~~
$ bundler update
~~~
Gem　のアップデートがあれば行う。
***

## bundle install したGemを使う
~~~
$ bundle exec

[例]
$ bundle exec rails s
~~~
Gemfile.lockに書かれているバージョンのgemが動く。
***

### つけなくてもいい？？
Railsでは利便性や一貫性のために、        
bundle execをつけなくても実行できるコマンドを binディレクトリに用意している。    
このようなファイルをbinstubという。    
各ファイルは実行権限が付与されていて、bundle execなしで直接実行できる。

[![Image from Gyazo](https://i.gyazo.com/1769ec894ba13ee5f63e36038f47475b.png)](https://gyazo.com/1769ec894ba13ee5f63e36038f47475b)
***

## Bundlerの設定
~~~
$ bundle config set
~~~
Bundlerの設定を変更するためのコマンド。    
オプション(--local)なしだとグローバルなBundlerの設定を変更する。    
あんま良くない。
***

### オプション
- 設定変更を現在のプロジェクトにのみ適用。
~~~
--local

[例]
$ bundle config set --local
~~~
現在のプロジェクトのみに対して設定を変更するという意味になる。    
つまり、「.bundle/configファイル」がプロジェクトのルートディレクトリに作成され、そこに設定が保存される。
***

## Gemfile.lock作成
~~~
$ bundle lock
~~~
⭐️gemをインストールせずに Gemfile.lockだけを更新する。    
プロジェクトで使用されるすべてのGemパッケージとそのバージョンの情報が含まれる。    
Gemfile.lockは、Gemのバージョンを固定するために使用され、他の開発者との一貫性を保つために共有される。
***

### オプション
- プラットフォームの指定    
プラットフォーム...システムやサービスを動かすための「土台」や「基盤となる環境」のこと。
~~~
--add-platform プラットフォーム名

[例]
--add-platform x86_64-linux
~~~
このオプションを使用することで、特定のプラットフォームに依存するGemのバージョンが正しくロックされ、    
プロジェクトの実行環境に合わせた正確な依存関係が確立される。    
Gemfile.lockの PLATFORMS 欄に、bundle lock --add-platform xxxで指定したプラットフォームが追記され、    
そのプラットフォームで必要な gem の情報がGemfile.lockに追記される。
***

### なぜこれを行う？？
M1 Macで CPUが Intel製から ARM製に変わり、CPUアーキテクチャも変わった。    
M1 Macをローカル開発環境に利用していて、本番サーバが Intelアーキテクチャの場合、    
CPUアーキテクチャの違いにより、bundle installで失敗することがある。   
それを防ぐためにプラットフォームを指定してあげる。
***
