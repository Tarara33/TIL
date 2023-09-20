# enumとは
enumは、特定のフィールドの値を予め定義した固定のセットから選択するためのデータ型。      
  
例えば Userモデルに性別を表す genderカラムがあったとして、  
その値を'男性'、'女性'、'その他'の3つから選ぶようにするときに役立つ。  
***

# 設定の仕方
## enum使うカラムは integer型にする
enum使うカラムを追加する。　(すでにあればそれ使う)
~~~
$ rails g migration AddGenderToUser gender:integer
~~~
***

## モデルファイルで設定
カラムを持つモデルファイルにて設定。
~~~
[app/models/user.rb]

enum gender: {man: 0, woman: 1}
~~~
***
