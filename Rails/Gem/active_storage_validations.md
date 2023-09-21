[Gemなしのバリデーション](https://github.com/Tarara33/TIL/tree/main/Rails/Model/%E3%83%90%E3%83%AA%E3%83%87%E3%83%BC%E3%82%B7%E3%83%A7%E3%83%B3)  
  
# 機能
active_storageに簡単にバリデーションを追加できる。  
既存の validateと同じような記述でバリデーションを追加できる！  
***

# 使い方
## Gem導入
~~~
[gemfile]
gem "active_storage_validations"

$ bundle
~~~
***

## モデルファイルにバリデーションかく
たとえば、画像添付を Postモデルに attachしているなら、バリデーションも Postモデル内にかく。
~~~
[app/models/post.rb]

lass Posts < ApplicationRecord
  belongs_to :user
  has_one_attached :image

  validates :image,   content_type: { in: %w[image/jpeg image/gif image/png],
                                      message: "must be a valid image format" },
                      size:         { less_than: 5.megabytes,
                                      message:   "should be less than 5MB" }
end
~~~
***
