# Scaffold 
アプリの足場を作ってくれる   
CRUD(Create, Read, Update, Delete)を備えてくれる
***

# ストロングパラメーター
ストロングパラメータはWeb上から受けつけたパラメータが本当に安全なデータかどうかを検証した上で、取得するための仕組み     
例えばデータを管理するUserテーブルで、(:text)、(:contents)、(:admin)の３つのカラムがあるとする。   
ユーザーフォームから受け取りたいのは(:text)と(:contents)だが、    
(:admin)は管理者かどうかの判断をするカラムなのでユーザーからは受け取りたくない。    
しかしセキュリティとしてストロングパラメーターを設定しないと悪意あるユーザーが(:admin)カラムの情報も送ってくることがあり、   
データテーブルがグチャる可能性がある    
***

## 設定の仕方
~~~
private
def user_params
  params.require(:キー(モデル名)).permit(:カラム名１,：カラム名２,・・・).merge(カラム名: 入力データ)
end
~~~
CRUD機能で実装するとしたらユーザーから情報を受け取る(params)    
NEW(新規作成),EDITE(編集),UPDATE(更新)のそれぞれのメソッドで使うことになるので    
ストロングパラメーター自体をメソッドにした方がいい（privateで）  
***

### なぜrequire？
requireメソッドを使用する事で、params内の特定のキーに紐付く値だけを抽出する事ができるため。   
そのため、引数には取り出したい値のキーを指定する必要がある。    
設定元はView上でform_withメソッドを使用した場合のキー設定箇所は、form_withに続く”@task"がrequireメソッドで指定すべきキー。
~~~
= form_with model: @task, local: true do |f|
...省略
~~~
***

### permitメソッド
permitメソッドを使用する事で、許可された値のみを取得することができる。    
⚠️そのため、permitメソッドの引数には登録を許可する全てのカラム名を指定しておく必要がある。   
もし、許可されていないカラムがparams内に存在した場合、そのデータは取得されず無視される。   
***

### margeメソッド
mergeメソッドを使用することでハッシュ同士を結合することができる。   
例えば、paramsに含まれない値をストロングメソッドに加えたい場合などにストロングパラメータの後に記述することができる。
***

#### 例
~~~
private
def user_params
  params.require(:task).permit(:text, :contents).merge(user_id: current_user.id)
end
~~~
***
