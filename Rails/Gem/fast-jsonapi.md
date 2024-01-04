[公式](https://github.com/Netflix/fast_jsonapi)  
[参考](https://qiita.com/mmaumtjgj/items/c855f43bef750d86d9cf)

# fast-jsonapi
[JSON](https://github.com/Tarara33/TIL/blob/main/API/API%E3%81%A8JSON%E3%81%A8%E3%81%AF.md#json)の serializer。

#### ❓ serializer(シリアライザー)
JSON形式のファイルの中身をコンピューターがわかりやすい文字列に書き換えること。
~~~
{
  "名前": "ピカチュウ",
  "タイプ": "電気",
  "HP": 50
}

↓

{"名前": "ピカチュウ", "タイプ": "電気", "HP": 50}
~~~
改行が無くなっただけのように思えるが、改行がないことによりデータのサイズが小さくなり、取り扱いが容易になる。
***

# 使い方
## Gem導入
~~~
[gemfile]
gem 'fast_jsonapi'

$ bundle
~~~
***

# JSON形式にしたいモデル情報を作る
~~~
rails g serializer Article（モデル名） title contents status（カラム名を列挙）
~~~
すると、`app/serializers/article_serializer.rb`が作成される。
***

## app/serializers/article_serializer.rbの編集
今回、欲しい JSON形式は以下のような感じ。  
articleと userの情報が欲しい。
~~~
{
  "data"=>
    [
      {
        "id"=>"1",
        "type"=>"article",
        "attributes"=>{
          "title"=>"MyString1",
          "contents"=>"MyText1",
          "status"=>"draft"
        },
        "relationships"=>{
          "user"=>{
            "data"=>{
              "id"=>"1",
              "type"=>"user"
            }
          }
        }
      },
~~~

なので、articleモデルとのアソシエーション情報も記載する。
~~~
[app/serializers/article_serializer.rb]

class ArticleSerializer
  include FastJsonapi::ObjectSerializer
  attributes :title, :contents, :status

  ⭐️belongs_to :user
end
~~~
***

## コントローラーの編集
~~~
class ArticlesController < BaseController
  def index
    ① articles = Article.all
    ② json_string = 🩵ArticleSerializer.new(articles).⭐️serialized_json

    ③ render json: json_string
  end
end
~~~
##### ⭐️ serialized_json
このメソッドは、シリアライザーが生成した JSONデータを取得するためのもので、  
これにより Railsの renderメソッドなどを使って、JSONデータをクライアントに返すことができる。

🩵 ArticleSerializer.new(article)は articleオブジェクトを JSONにシリアライズするためのシリアライザーを作成し、  
その後の`.serialized_json`はそのシリアライザーを使って JSONデータを生成するメソッド。

***

