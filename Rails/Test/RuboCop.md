[参考](https://qiita.com/tomohiii/items/1a17018b5a48b8284a8b)


# RuboCop
Rubyの静的コード解析を実行するgem。    
要はRuboCopが.rbファイルに記述してあるコードを検査し、ここのコードは長すぎるね。とか、    
インデント入れたほうがいいよ。とかメソッド名変えようか。とかをコマンド１つでターミナルに吐き出しててくれる。    
(HTML、CSS、またはそれらの中の埋め込みrubyは解析してくれない。)
***

# 設定
- .rubocop.yml
RuboCopの設定ファイル。    
対象となるファイルの種類だったり、チェックする構文のデフォルトを変えたりと、        
自分たちのコーディングスタイルに沿った現実的なルールをこのファイルで設定する。
***

- .rubocop_todo.yml
あまりに警告が多い時に`$ rubocop --auto-gen-config`を    
実行することによって自動生成され、警告内容を全てこのファイルに一旦退避することができる。    
それ以降は退避された警告は無視される。    
***

# コマンド
## 解析
~~~
$ rubocop

[特定のファイルをチェックする場合]
$ rubocop ファイル名
~~~
解析して結果をターミナルに吐き出す。
***

## 自動修正
~~~
$ rubocop --auto-correct
または
$ rubocop -a

[特定のファイルをチェックする場合]
$ rubocop --auto-correct　ファイル名
または
$ rubocop -a　ファイル名
~~~
直せる箇所を自動で修正する。
***

## 警告避難
~~~
rubocop --auto-gen-config
~~~
.rubocop_todo.ymlに警告を一旦退避する。    
⚠️.rubocop.ymlに 「inherit_from: .rubocop_todo.yml」 と書くのを忘れないように。
***