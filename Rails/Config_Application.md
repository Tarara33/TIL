[ジェネレーター](https://railsguides.jp/configuring.html#%E3%82%B8%E3%82%A7%E3%83%8D%E3%83%AC%E3%83%BC%E3%82%BF%E3%82%92%E8%A8%AD%E5%AE%9A%E3%81%99%E3%82%8B)
[ワークフローをカスタマイズする](https://railsguides.jp/generators.html#%E3%83%AF%E3%83%BC%E3%82%AF%E3%83%95%E3%83%AD%E3%83%BC%E3%82%92%E3%82%AB%E3%82%B9%E3%82%BF%E3%83%9E%E3%82%A4%E3%82%BA%E3%81%99%E3%82%8B)
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
    g.skip_routes true
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
