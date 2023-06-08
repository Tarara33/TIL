[ジェネレーターを設定する](https://railsguides.jp/configuring.html#%E3%82%B8%E3%82%A7%E3%83%8D%E3%83%AC%E3%83%BC%E3%82%BF%E3%82%92%E8%A8%AD%E5%AE%9A%E3%81%99%E3%82%8B)      
[ワークフローをカスタマイズする](https://railsguides.jp/generators.html#%E3%83%AF%E3%83%BC%E3%82%AF%E3%83%95%E3%83%AD%E3%83%BC%E3%82%92%E3%82%AB%E3%82%B9%E3%82%BF%E3%83%9E%E3%82%A4%E3%82%BA%E3%81%99%E3%82%8B)
# rails g 実行時に無駄なファイル作りたくない
Applicationクラス内に記述
~~~
[config.application.rb]

module RunteqNormal
  class Application < Rails::Application
   config.generators do |g|
      g.ファイル名 false
 例)   g.helper false　
      # helperファイルを作成しない
      g.assets false
      # css,javascriptファイルを作成しない
      g.test_framework false
      # テストファイルを作成しない
      g.skip_routes true
      # routes.rbを変更しない
   end
 end
end
~~~
***

# 言語設定日本語にする
Applicationクラス内に記述
~~~
[config.application.rb]
module RunteqNormal
  class Application < Rails::Application
   config.i18n.default_local = :ja
  end
end
~~~
***

# 時間を日本時間にする
Applicationクラス内に記述
~~~
[config.application.rb]
module RunteqNormal
  class Application < Rails::Application
    config.active_record.default_timezone = :local 
    config.time_zone = 'Asia/Tokyo'
  end
end
~~~
***

## config.active_record.default_timezone
DBを読み書きする際に、DBに記録されている時間をどのタイムゾーンで読み込むかの設定。   
:UTC か　　:local 　の二種類から選ぶ。   
ActiveRecordを読み書きする際に、:UTCと設定されていればUTC時刻として読み書きすることになり、   
:localと設定すると、OSのタイムゾーン設定と同じ形になる。
***

## config.time_zone
Rails自体のアプリケーションの時刻の設定。
***

## rails c　で`$ 設定した時刻の確認
- `$ Time.zone.now`...OSのタイムゾーンの結果（UTC or local）を返す。
- `$ Time.current`...Railsアプリのタイムゾーンの結果を返す。
***
