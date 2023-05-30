# rails g 実行時に無駄なファイル作りたくない
Applicationクラス内に記述
~~~
[config.application.rb]

config.generators do |g|
   g.ファイル名 false
例) g.helper false
  ...と書いていく
end
~~~
***

# 言語設定日本語にする
Applicationクラス内に記述
~~~
[config.application.rb]

config.i18n.default_local = :ja
~~~
