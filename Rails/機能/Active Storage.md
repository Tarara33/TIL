[参考](https://qiita.com/hmmrjn/items/7cc5e5348755c517458a#active-storage-%E3%81%A8%E3%81%AF)
  
# Active Storage
gem Carrierwaveなどを使わなくても、あるレコードに 1つまたは複数のファイルを添付できる。
***

# Active Storageを使えるようにする
~~~
$ rails active_storage:install
$ rails db:migrate
~~~
このマイグレーションによって  
  
- active_storage_blobs    
実際にアップロードしたファイルが保存されるテーブル  
(実際はファイル名、ファイルの種類、バイト数、誤り検出符号などのファイルデータを保持するテーブル)  
    
- active_storage_attachments  
実際に操作を行うモデルと active_storage_blobsを紐づける中間テーブル

  
という２つのテーブル生成され、それぞれ Blobと Attachmentというの2つのモデルが使う。  
なお、Active Storageを使う際、直接 Blobと Attachmentモデルに触れる必要はない。  
***

# Active Record モデルを用意する
例えば、記事（Article）に一枚画像を添付できるようにしたい。  
  
- 画像を一枚添付したい場合...`has_one_attached :ファイルの呼び名(単数系)`      
- 画像を複数添付したい場合...`has_many_attached :ファイルの呼び名(複数形)`
  
⚠️ 単数系か複数形か間違えないように！
~~~
[app/models/article.rb]

class Article < ApplicationRecord
  has_one_attached :eye_catch
end
~~~
:eye_catchはファイルの呼び名で、:photo、:avatar、:hogeなど、ファイルの用途に合わせて好きなものを指定してOK。  
ここで eye_catchカラムや、Imageモデルなどを作る必要はない。  
Active Storageは裏側で Blobと Attachmentモデルを使って、こそこそと article.imageを使えるようにしてくれる。
***

# コントローラー編集
ストロングパラメーターに「:eye_catch」を入れる。
~~~
[app/controllers/article_controller.rb]

def create
  authorize(Article)

  @article = Article.new(article_params)
  @article.state = :draft

  if @article.save
    redirect_to edit_admin_article_path(@article.uuid)
  else
    render :new
  end
end

private

def article_params
  params.require(:article).permit(:title, :description, :slug, :state, :published_at, :eye_catch, :category_id, :author_id, :eyecatch_width, :eyecatch_align, tag_ids: [])
end
~~~
***

# ビューの編集(フォーム)
~~~
[app/views/articles/new.html.erb]

<%= simple_form_for @article, url: admin_article_path(@article.uuid) do |f| %>
  <%= f.input :eye_catch, as: :file %>
<% end %>
~~~
***

## 複数ファイルを添付するとき
- `multiple: true`をつける。  
- ファイル名を複数形にする。    
~~~
<%= simple_form_for @article, url: admin_article_path(@article.uuid) do |f| %>
  <%= f.input :eye_catchs, as: :file, multiple: true %>
<% end %>
~~~
***

# ビューの編集(表示)
image_tagに渡すだけ！
~~~
[app/views/articles/show.html.erb]

<% if @article.eye_catch.attached? %>
  <%= image_tag @article.eye_catch %>
<% end %>
~~~
***

## 複数ファイルを表示するとき
- ファイル名を複数形にする。(eye_catch => eye_catchs)
~~~
<% if @article.eye_catchs.attached? %>
  <% @article.eye_catchs.each do |eye_catch| %>
    <%= image_tag eye_catch %> <br>
  <% end %>
<% end %>
~~~
***

# ファイルの保存先の変更
ファイルの保存先は、各環境の設定ファイルに記載する。  
まずは、 config/environments/development.rb と production.rb の中身を見てみる。
~~~
[config/environments/development.rb]

config.active_storage.service = :local


[config/environments/production.rb]

config.active_storage.service = :local
~~~
初期状態では、開発環境(development)、本番環境(production)ともに保存先は :local に設定されている。  
この localとは、「config/storage.yml」で定義された保存先の名前。  

これを変更するには、:local のところを :amazon, :google, :microsoft のいずれかと置き換え、  
「config/storage.yml」の方に、必要な認証情報などの値を入力する。  
***

## config/storage.yml
~~~
[config/storage.yml]

test:
  service: Disk
  root: <%= Rails.root.join("tmp/storage") %>

local:
  service: Disk
  root: <%= Rails.root.join("storage") %>

# Use rails credentials:edit to set the AWS secrets (as aws:access_key_id|secret_access_key)
# amazon:
#   service: S3
#   access_key_id: <%= Rails.application.credentials.dig(:aws, :access_key_id) %>
#   secret_access_key: <%= Rails.application.credentials.dig(:aws, :secret_access_key) %>
#   region: us-east-1
#   bucket: your_own_bucket

# Remember not to checkin your GCS keyfile to a repository
# google:
#   service: GCS
#   project: your_project
#   credentials: <%= Rails.root.join("path/to/gcs.keyfile") %>
#   bucket: your_own_bucket

# Use rails credentials:edit to set the Azure Storage secret (as azure_storage:storage_access_key)
# microsoft:
#   service: AzureStorage
#   storage_account_name: your_account_name
#   storage_access_key: <%= Rails.application.credentials.dig(:azure_storage, :storage_access_key) %>
#   container: your_container_name
~~~
先ほど見た保存先の localは、使用するサービスが Disk(ローカルディスク)に設定されていて、  
railsアプリ直下の「/storage」ディレクトリがファイルの保存先に指定されている。  
      
他のものにしたいときはコメントアウトされてる中から  
適切なところのコメントを解除することで、好きなストレージサービスを使える。  
また、使うサービスの gemを Gemfileに追記する必要がある。  
これは、aws-sdk-s3, google-cloud-storage, azure-storageのいずれか。  
  
まだ実装したことないのでしたらまた詳しくまとめる。
***

# バリデーション
[こちら](https://github.com/Tarara33/TIL/blob/main/Rails/Model/%E3%83%90%E3%83%AA%E3%83%87%E3%83%BC%E3%82%B7%E3%83%A7%E3%83%B3/Active%20Storage.md)
***
