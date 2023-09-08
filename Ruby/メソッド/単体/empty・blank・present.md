# empty? と blank? と present?
## empty?
empty? は空かどうかを確認しますが、レシーバーが nilだと、UndefinedMethodになってしまう。
~~~
# 空配列に emptyを使ってみる
[].empty?
# => true

# 空文字に emptyを使ってみる
''.empty?
# => true

# nilに emptyを使ってみる
nil.empty?
# NoMethodError: undefined method `empty?' for nil:NilClass

# nilに emptyメソッドがあるのか確認
nil.respond_to? :empty?
# => false
~~~
***

## blank?
nilに対しても使えて nilなら trueを返す。
~~~
# 空配列に blankを使ってみる
[].blank?
# => true

# 空文字に blankを使ってみる
''.blank?
# => true

# nilに blankを使ってみる
nil.blank?
# => true

# nilに blankメソッドがあるのか確認
nil.respond_to? :blank?
# => true
~~~
***

## present?
blank?メソッドと逆の働きを持つ。
~~~
# 空配列に presentを使ってみる
[].present?
# => false

# 空文字に presentを使ってみる
''.present?
# => false

# nilに presentを使ってみる
nil.present?
# => false

# nilに presentメソッドがあるのか確認
nil.respond_to? :present?
# => false
~~~
***
