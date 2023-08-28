# Active Storageのバリデーション
Active Storageや Paperclipなどにおいて、ファイル添付属性に関連するオプションを設定する場合
~~~
validates :ファイルのカラム名, attachment: { ... }
~~~
` attachment: { ... }`という構文を使う。
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
ファイルのコンテンツタイプを検証する。

### ⭐️ maximum
ここでの maximumはファイルの最大サイズをバイト単位で指定している。  
ここでは約 10MB（10,485,760バイト）を設定し、超えればバリデーションに引っかかる。
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
先ほどのバリデーションで制限かかるが、エラーメッセージを設定したりしたい場合
