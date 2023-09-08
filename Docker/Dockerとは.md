# Dockerとは
アプリケーションとその依存関係をパッケージ化して、    
どの環境でも同じように動作するようにするためのオープンソースのプラットフォーム。    
これにより、開発環境と本番環境の違いを気にすることなくアプリケーションを開発・運用できるようになる。    

Docker(デスクトップ)自体も一つのアプリケーションみたいな感じで、私のMacに入っている。
[![Image from Gyazo](https://i.gyazo.com/dcd3610753a535c0724238e555e0e194.png)](https://gyazo.com/dcd3610753a535c0724238e555e0e194)
***

# コンテナ
コンテナとは隔離された仮想環境。    
    
【特徴】    
- コンテナは組み合わせて使うことができる。    
- コンテナ内で開発する。    
        
[![Image from Gyazo](https://i.gyazo.com/918c16d7d07d8ef85553d76ae4c42f5a.png)](https://gyazo.com/918c16d7d07d8ef85553d76ae4c42f5a)
    
イメージとしてはこんな感じで、環境構築に必要なアプリ(rbenvとか)もコンテナ内にあるので、    
例えば、AさんのPCには rbenv入ってるけど、BさんのPCには入ってないからダウンロードしなきゃ！ ということがない。
みんな同じ環境で開発できる！
***

# イメージ
イメージとは、コンテナの設計のようなもの。
「Dockerfile」と言うところに書いていく。
~~~
[Dockerfile]

FROM ruby:3.1.4
=> ruby 3.1.4を使ってね

ENV LANG C.UTF-8
ENV TZ Asia/Tokyo
=> 環境変数(ENV)で、文字コードの設定とタイムゾーンをアジアに設定してるよ

RUN curl -sL https://deb.nodesource.com/setup_18.x | bash - \
  && wget --quiet -O - /tmp/pubkey.gpg https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - \
  && echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list \
  && apt-get update -qq \
  && apt-get install -y build-essential libpq-dev nodejs yarn mariadb-client
=> nodeのインストールをしてるよ

RUN mkdir /sql_active_record_practice
=> sql_active_record_practiceというディレクトリの作成を実行してるよ

WORKDIR /sql_active_record_practice
=> 作業ディレクトリを sql_active_record_practiceにしてるよ

RUN gem install bundler:2.3.17
=> gem　bundlerのインストールを実行してるよ

COPY Gemfile /sql_active_record_practice/Gemfile
COPY Gemfile.lock /sql_active_record_practice/Gemfile.lock
=> Gemfileと Gemfile.lockをコピーしてるよ

RUN bundle install
=> bundle installで gemたちのインストールを実行してるよ

COPY . /sql_active_record_practice
COPY config/database-docker.yml /sql_active_record_practice/config/database.yml
=> ファイルをコピーしてるよ
~~~
と言う感じにこういう設計してねと書いてある。      
つまり、コンテナの設計図がイメージでそれを元にコンテナができる。  
***

# ボリューム
ボリュームとは Dockerが用意したデーターを保存する場所。      
      
例えば、コンテナを使って開発をして、データーは MySQLコンテナに保存されるが、    
MySQLコンテナが消えた場合、中のデーターも消える。    
ボリュームに保存しておけば、コンテナが消えてもデーターは消えない。
***
