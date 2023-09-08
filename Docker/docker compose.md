# docker composeとは
それぞれのコンテナをマネジメントしてるやつ。
コンテナ管理を docker-composeによる管理で行うと複数のコンテナで構成されるアプリケーションについて、    
Docker Imageのビルドや各コンテナの起動・停止、ネットワーク接続をコマンド1行で実行できるようになる。
***

# ❓ docker composeを使った場合 と 使わない場合
例えば コンテナ: apple と　コンテナ:banana を起動したいとき
~~~
$ docker ps
=>

CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
67e46a979b6b        ubuntu              "/bin/bash"         3 days ago          Up 2 seconds                            apple
90jcds4ghca3        ubuntu              "/bin/bash"         1 days ago          Up 2 seconds                            banana
~~~
***

### 使った場合
~~~
$ docker compose up -d
~~~
💡 -dオプションつけるとバックグラウンドでコンテナ立ち上げるので、ターミナルで他の作業できて便利。
***

### 使わなかった場合
~~~
$ docker start 67e46a979b6b
または
$ docker start apple
=> コンテナ: apple起動

$ docker start 90jcds4ghca3
または
$ docker start banana
=> コンテナ: banana起動
~~~
実際はコンテナの依存関係なども考慮しなきゃなので、めんどい。
***

# docker compose.yml
docker-composeコマンドで立ち上げるコンテナの起動オプションなどを含め、複数のコンテナの定義を書くファイル。      
これを利用して docker compose up -dコマンドで 一括コンテナ起動をすることができる。
~~~
[docker compose.yml]

version: '3'
services:
  db:
    image: mysql:5.7
    platform: linux/amd64
    environment:
      TZ: Asia/Tokyo
      MYSQL_ROOT_PASSWORD: password
    volumes:
      - mysql_data:/var/lib/mysql
    ports:
      - 3307:3306
  web:
    build: .
    command: bash -c "rm -f tmp/pids/server.pid && bundle exec rails s -p 3000 -b '0.0.0.0'"
    tty: true
    stdin_open: true
    volumes:
      - .:/app
      - bundle_data:/usr/local/bundle:cached
      - node_modules:/app/node_modules
    environment:
      TZ: Asia/Tokyo
    ports:
      - "3000:3000"
    depends_on:
      - db
volumes:
  mysql_data:
  bundle_data:
  node_modules:
~~~~
***

## docker compose exec web bash / docker compose exec db bash
これらのコマンドは、「docker compose.yml」にある、    
db:...のコンテナ    
web:...のコンテナ    
に入るコマンド。    
~~~    
services:
  db:.....
  web:....
~~~
***

# コンテナ停止
~~~
$ docker compose down
~~~
一括停止。
***
