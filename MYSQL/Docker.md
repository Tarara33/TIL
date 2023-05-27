# Dockerを使ってMysqlに入る
作業ディレクトリに「docker-compose.yml」のファイルを作り、内容記載する。   

ターミナルでファイルのある作業ディレクトリまで「cd」で移動したら     
`$ docker-compose up`でDockerコンテナ動くかチェック。 

動かしたら、ターミナルを別タブ開いてまた作業ディレクトリまで移動。（コンテナ起動のまま）

## 入り方①
~~~
$ docker-compose up -d && docker-compose exec db bash 
を打つと、    

bash-4.2#
のモードになるので
mysql -u root -p
と打つと
Enter password:
と出る。そこに「example」と入力（入力値見えないタイプ）すると
mysql>
モードに入る
~~~
***

## 入り方②
~~~
$ docker ps
でコンテナ情報見る
=>
CONTAINER ID   IMAGE       COMMAND                   CREATED          STATUS          PORTS                               NAMES
8353af6f078e   mysql:5.7   "docker-entrypoint.s…"   15 seconds ago   Up 14 seconds   33060/tcp, 0.0.0.0:3308->3306/tcp   sql_basic-db-1
などと出る

＄　docker exec -it MySQLのコンテナ名（NAMESのとこ） bash
を打つと
bash-4.2#
のモードになるので
mysql -u root -p
と打つと
Enter password:
と出る。そこに「example」と入力（入力値見えないタイプ）すると
mysql>
モードに入る
~~~
***

## 入り方③
~~~
$ docker-compose ps --service
でdocker-composeで起動しているサービス名を確認する。
（サービス名は、docker-compose.ymlにも書いてある（Services）ので、そちらを見てもよい。）

$ docker-compose exec (サービス名) /bin/bash
を打つと
bash-4.2#
のモードになるので
mysql -u root -p
と打つと
Enter password:
と出る。そこに「example」と入力（入力値見えないタイプ）すると
mysql>
モードに入る
~~~
***

mysql> や　bash-4.2# から出るには`exit`   
Docker止めるには`$ docker-compose down`とする
***


# Docker run と　start
runにすると新しくコンテナができる。   
startだと既存コンテナ起動
***

