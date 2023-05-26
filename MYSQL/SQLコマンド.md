# データベース一覧を見る
~~~
mysql> show databases;
=>

+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| runteq_study       |
| sys                |
+--------------------+
5 rows in set (0.00 sec)
~~~
***

# データベースを作成する
~~~
mysql> create database runteq_study;
~~~
***

# データベースを削除する
~~~
mysql> drop database データベース名
~~~
***

# どのデータベースを使うかを選択する
~~~
mysql> use runteq_study
~~~
***

# テーブル一覧を見る
~~~
mysql> show tables;
~~~
***

# テーブルを作成する
~~~
mysql> CREATE TABLE user(
    id INT(11) AUTO_INCREMENT NOT NULL, 
    first_name VARCHAR(30) NOT NULL,
    last_name VARCHAR(30) NOT NULL,
    PRIMARY KEY (id));

=>　CREATE TABLE　の後にテーブル名
  （）内にカラム名、詳しい設定入れていく
~~~
- INT（）...ZELOFILLといってカッコ内で指定した数値の桁数になるようにゼロ埋め処理が実施される。
今回なら１１桁の数字で1番なら「00000000001」と表示される。

- AUTO_INCREMENT...テーブルを作成するときにカラムに AUTO_INCREMENT をつけると、
データを追加した時にカラムに対して現在格納されている最大の数値に 1 を追加した数値を自動で格納する。

- VARCHAR()...()の数字の文字数まで入る。
- PRIMARY KEY ()...データを一意に識別するための項目で（）内のカラムにもたせる
***

# テーブルにレコードを追加する
~~~
INSERT INTO テーブル名　　（カラム名１, カラム名２) VALUE("カラム名１の内容", "カラム名2の内容")
~~~


