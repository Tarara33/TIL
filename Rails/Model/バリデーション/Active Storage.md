# Active Storageのバリデーション
Active Storageや Paperclipなどにおいて、ファイル添付属性に関連するオプションを設定する場合
~~~
validates :ファイルのカラム名, attachment: { オプション内容 }
~~~
` attachment: { オプション内容 }`という構文を使う。
***

## 実装例
~~~
[app/models/article.rb]

has_one_attached :eye_catch
validates :eye_catch, attachment: { purge: true, content_type: %r{\Aimage/(png|jpeg)\Z}, maximum: 10_485_760 }
~~~

### ⭐️ purge
バリデーションを通過しなかった場合にファイルを自動的に削除するためのオプション。  
バリデーションに合格しないファイルは自動的に削除される。  

### ⭐️ content_type
ファイルのコンテンツタイプを検証するオプション。

### ⭐️ maximum
ここでの maximumはファイルの最大サイズをバイト単位で指定している。  
ここでは約 10MB（10,485,760バイト）を設定し、超えればバリデーションに引っかかる。
  
⚠️ `attachment:{}`の中身はオプションなのでコレらは全てメソッドではなくオプション！
***

### ちなみに...
~~~
validates :eye_catch, attachment: { purge: true, content_type: %r{\Aimage/(png|jpeg)\Z}, maximum: 10_485_760 }
~~~
~~~
validates :eye_catch, attached: true, content_type: %r{\Aimage/(png|jpeg)\Z}, size: { less_than: 10.megabytes }
~~~
この二つのコードは、ほぼ同じ意味(?)  
※ purgeは Active Storageのメソッドなので無効になる。らしい。

### ⭐️ attached
ファイルが添付されていることを確認する。

# カスタムバリデーションも追加する場合    
先ほどモデルで定義したバリデーションで制限かかるが、    
エラーメッセージのカスタマイズや、バリデーション失敗時の処理など、より細かい制御を行いたい場合は、カスタムバリデータが必要。  
  
⚠️ カスタムバリデーションファイルは手動で作成(`検証名_validator.rb`つけるの忘れずに)
~~~
[app/validators/attachment_validator.rb]

①class AttachmentValidator < ActiveModel::EachValidator
  ②include ActiveSupport::NumberHelper

  ③def validate_each(record, attribute, value)
    return if value.blank? || !value.attached?

    ④has_error = false

    ⑤if options[:maximum]
      has_error = true unless validate_maximum(record, attribute, value)
    end

    ⑥if options[:content_type]
      has_error = true unless validate_content_type(record, attribute, value)
    end

    ⑦record.send(attribute).purge if options[:purge] && has_error
  end

  private

  ⑦def validate_maximum(record, attribute, value)
    if value.byte_size > options[:maximum]
      record.errors[attribute] << (options[:message] || "は#{number_to_human_size(options[:maximum])}以下にしてください")
      🩵false
    else
      💚true
    end
  end

  ⑧def validate_content_type(record, attribute, value)
    if value.content_type.match?(options[:content_type])
      true
    else
      record.errors[attribute] << (options[:message] || 'は対応できないファイル形式です')
      false
    end
  end
end
~~~
***

## ① ActiveModel::EachValidator
1つの属性を検証するための新しいバリデータを追加したい場合は ActiveModel::EachValidatorを継承したクラスを定義する。    
今回に関しては「:eye_catch」属性を検証してる。  

- クラス名は<検証名>Validatorの形式で命名。
- validate_eachメソッドを実装する必要がある。
***

## ② ActiveSupport::NumberHelper
サポートモジュールの一つ。  
数値を通貨形式にしてくれたり、数値の桁を区切ってくれたりする。
***

## ③ validate_each(record, attribute, value)
この「ActiveModel::EachValidator」を継承したクラス内で実装する必要があるバリデーションを行うためのメソッド。
    
- record...検証対象のモデルインスタンス  
- attribute...検証対象の属性名  
- value...検証対象の属性値  

しかし、もし valueが空かファイルの添付がない場合は returnでバリデーションスキップしてる。
***

### 確認してみる
この記事で record, attribute, valueそれぞれ何が入ってるのか確認してみる。[(参考)](https://blog.cloud-acct.com/posts/u-rails-custom-eachvalidator/#%E4%BB%AE%E3%83%A6%E3%83%BC%E3%82%B6%E3%83%BC%E3%81%AE%E4%BF%9D%E5%AD%98)    
[![Image from Gyazo](https://i.gyazo.com/7379be1a4b6ac442036cb38de9317545.png)](https://gyazo.com/7379be1a4b6ac442036cb38de9317545)  
    
~~~
def validate_each(record, attribute, value)
    binding.pry
~~~
~~~
[rails c]

[1] pry(#<AttachmentValidator>)> record
=> #<Article:0x000000010b7c26b0
 id: 5,
 category_id: 9,
 author_id: 4,
 uuid: "bf823b2e-76a4-45ef-a87e-a6774df3147e",
 slug: "w",
 title: "ruby on rails",
 description: "デスクトップ",
 body: "<p>ruby on rails</p>",
 state: "published",
 published_at: Mon, 28 Aug 2023 22:00:00 JST +09:00,
 created_at: Fri, 25 Aug 2023 16:16:28 JST +09:00,
 updated_at: Tue, 29 Aug 2023 03:22:23 JST +09:00,
 deleted_at: nil,
 eyecatch_width: 100,
 eyecatch_align: "center">

[2] pry(#<AttachmentValidator>)> attributes
=> [:eye_catch]

[3] pry(#<AttachmentValidator>)> value
=> #<ActiveStorage::Attached::One:0x000000010c0ae058
 @dependent=:purge_later,
 @name="eye_catch",
 @record=
  #<Article:0x000000010b7c26b0
   id: 5,
   category_id: 9,
   author_id: 4,
   uuid: "bf823b2e-76a4-45ef-a87e-a6774df3147e",
   slug: "w",
   title: "ruby on rails",
   description: "デスクトップ",
   body: "<p>ruby on rails</p>",
   state: "published",
   published_at: Mon, 28 Aug 2023 22:00:00 JST +09:00,
   created_at: Fri, 25 Aug 2023 16:16:28 JST +09:00,
   updated_at: Tue, 29 Aug 2023 03:22:23 JST +09:00,
   deleted_at: nil,
   eyecatch_width: 100,
   eyecatch_align: "center">>

~~~
***

## ④ has_error = false
このバリデーションメソッド内でエラーの有無をトラッキングするための変数。
この変数は初期値として falseで初期化しておく。  
  
⑤・⑥の処理を行い、最終的に trueに変わったら、⑥の処理を行う。
***

## ⑤・⑥ options[:maximum] / options[:content_type]
この optionsには以下のコードで定義された同名のキーの値が格納されてる。    
💡 `attachment:{}`の中身はオプションが書かれているので、`options[]`で読み取っている！ 
~~~
[app/models/article.rb]

validates :eye_catch, attachment: { purge: true, content_type: %r{\Aimage/(png|jpeg)\Z}, maximum: 10_485_760 }
~~~
- [:maximum]は maximumの最大値「10_485_760」が格納されてる。    
- [:content_type]は「%r{\Aimage/(png|jpeg)\Z}」が格納されてる。  



