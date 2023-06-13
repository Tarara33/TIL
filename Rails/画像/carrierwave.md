# gemの導入
- carrierwave   
Uploaderクラスを持つようになるgem。   
なので画像に関しての設定（保存先は？、　デフォルト画像は？、　画像のサイズは？など）は   
Uploaderクラス内に記述する。
***

- Paperclip   
使ったことないが、シンプルな機能を手早く実装できるgem。   
しかしcarrierwaveと違ってクラスを持たないので画像に関しての設定を   
画像カラムが存在するモデルで書かなければいけない。
***

# carrierwaveを使った導入
## ①Gemfileに記載してbundle installする。    
## ②Uploaderクラスをアプリ内に作る。
~~~
$ rails g uploader アップローダーモデル名（既存モデル名ではなく、「画像カラム持たせるモデル＋Image」とか。　例：　BoardImage）
=> app/uploaders/image_uploader.rb　ファイルができる
~~~
***

## ③画像情報を入れるカラムを追加する
~~~
[例：　Boardモデルに追加する場合]
$ rails g migration AddBoardImageToBoard　　board_image(追加カラム名)：string

[db/migrate/日付_add_image_to_board.rb]
class AddImageToBoard < ActiveRecord::Migration[5.2]
  def change
    add_column :boards, :board_image, :string
  end
end

=>マイグレーションファイル確認したら $ rails db:migrate　する
~~~
⚠️データベースに保存されるのは、「画像データ」ではなく「画像のファイル名」である。なのでstring型。       
（画像データを保存するとDBサーバの容量を圧迫するため）    
カラムには、どの画像ファイルか確認できるようにファイル名だけを保存し、   
画像を表示するときに「画像が格納されているパス」と「データベースに保存されているファイル名」を使う
***

## ④Uploaderクラスと画像カラムの紐付け
`mount_uploader`メソッドを使う(carrierwave導入で使えるようになるメソッド)   
指定された属性（カラム）に対してアップロード機能を追加する。    
このメソッドにより、アップロードされたファイルが自動的に保存され、   
ファイルのバージョン管理や画像のリサイズなどの処理が容易になる。
~~~
mount_uploader :画像カラム名, Uploaderクラス名

[画像カラムがあるモデルファイル　例：app/models/board.rb]
class Board < ApplicationRecord
  mount_uploader :board_image, ImageUploader
end
~~~
***

## ⑤Uploaderクラスの設定
紐付けはできたので次はアップロードされた画像がどのように処理されるかを設定する。    
### 1.画像の保存先
- storage :file   
アップロードされたファイルはRailsアプリケーションのファイルシステム内の指定された場所に保存される。    
デフォルトでは、Railsのpublic/uploadsディレクトリ内に保存されるが、カスタマイズすることも可能。   
***

💡カスタマイズ方法
`store_dir`メソッドを使う
~~~
def store_dir
   "uploads/#{model.class.to_s.underscore}/#{mounted_as}/#{model.id}"
end

=> uploads/モデルのクラス名_/アップローダーのマウント名/モデルのID
で保存されるように設定している。

④の例でboardクラスと結びついてるとすると
public/board/board_image/1(boardのID)
と言う感じ。
~~~
***

- storage :fog    
fog-aws　というファイルの保存先を外部のストレージする際にサポートしてくれるgemを導入する。    
このように書くと保存先をそこにする。
***

### 2.デフォルト画像の設定
`default_url`メソッド使う。    
アップロードされたファイルが存在しない場合や表示できない場合に代替のデフォルト画像ファイルを返すために使用される。
通常、アップロードされたファイルが存在しない場合やエラーが発生した場合、    
CarrierWaveは指定されたデフォルトの画像ファイルを表示するためにdefault_urlメソッドを呼び出す。
~~~
def default_url
  "board_default.png"
end
~~~
⚠️default_urlメソッドは、デフォルトの画像ファイルを相対パスまたは絶対パスで返す必要があり、        
アプリケーション内のどこかに配置されている必要がある。（だいたいapp/assets/image以下におく）
***

### 3.拡張子の制限
`extension_whitelist`メソッドを使う。   
アップロードされるファイルの許可される拡張子（ファイルの種類）を制限するために使用される。   
このメソッドによって指定された拡張子以外のファイルはアップロードを拒否し、バリデーションエラーを発生させる。
~~~
def extension_whitelist
  %w[jpg jpeg png gif]
end

=> .jpg .jpeg .png .gif　のファイル以外は拒否する。
~~~
なぜ制限が必要か    
=> 悪意のあるコードを含む実行可能ファイルなどがアップロードされるのを防ぐことができる。
***

### 4.画像の処理
- サムネイルの生成    
~~~
varsion :thumbnail do
  process resize_to_fit: [100,100]
end
~~~
***

- 画像の切り抜き
~~~
process :crop_image

def crop_image
  if model.crop_x.present? && model.crop_y.present? && model.crop_width.present? && model.crop_height.present?
  => crop_x,yはそれぞれX,Y座標のこと。
  
    manipulate! do |img|
    => manipulate!メソッドは、画像処理ライブラリ（例：MiniMagick）を使用して画像を変更するためのブロックを受け取る
    
      img.crop!(model.crop_x.to_i, model.crop_y.to_i, model.crop_width.to_i, model.crop_height.to_i)
    end
  end
end
~~~
***


