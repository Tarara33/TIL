# rails g 実行時に無駄なファイル作りたくない
~~~
[config.application.rb]

config generators do |g|
   g.ファイル名 false
例) g.helper false
  ...と書いていく
end
~~~
