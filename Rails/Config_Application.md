# rails g 実行時に無駄なファイル作りたくない
Applicationクラス内に記述
~~~
[config.application.rb]

config.generators do |g|
   g.ファイル名 false
例) g.helper false　
    # helperファイルを作成しない
    g.assets false
    # css,javascriptファイルを作成しない
    g.test_framework false
    # テストファイルを作成しない
    g.skip_routes
    # routes.rbを変更しない
end
~~~
***

# 言語設定日本語にする
Applicationクラス内に記述
~~~
[config.application.rb]

config.i18n.default_local = :ja
~~~
