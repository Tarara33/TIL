# capybara
システムテストとかをするときに人間がブラウザ操作するようなシュミレーションをしてくれる。(ログインボタン押すとか)    

    
rails newして bundle installしたときにもう入ってるので、gemの追加などはいらない。    
⭐️しかし、クロームブラウザ使うので、　gem 'webdrivers'はインストールする。
~~~
[gemfile]

group :test do
gem 'webdrivers'
end

$ bundle
~~~
⚠️ capybaraはテスト環境しか使わないのでグループ「テスト」に書く。
***

# capybaraの設定
基本的には「spec/spec_helper.rb」に書く。
~~~
[spec/spec_helper.rb]

⭕️require 'capybara/rspe'

RSpec.configure do |config|
-------------------------------------------
  config.before(:each, type: :system) do　　　　　　　　#type:　で使用するテストタイプを指定。（例えば、システムテストやコントローラーテストなど）
    #driven_by :rack_test　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　#いくつか書いておくことでコメントアウト切り替えるだけでブラウザドライバー変えられる。
      driven_by :selenium_chrome_headless
    #driven_by :selenium_chrome
　　　 end
-------------------------------------------
end
~~~
⭕️を RSpec.configureの外に書く。
点線内のコードは RSpec.configureの中に書く。
***

## driven_by
driven_byメソッドは、Railsのシステムテストにおいて使用されるブラウザドライバーを指定するためのもの。
    
- driven_by :rack_test    
Railsのデフォルトのブラウザドライバーであり、JavaScriptを無視してシステムテストを実行する。    
このドライバーは高速で軽量だが、JavaScriptを含む多くのテストケースをサポートしていない。    
***

- driven_by :selenium_chrome_headless    
Google Chromeをヘッドレスモードで起動してシステムテストを実行する。    
JavaScriptを含むテストケースをサポートする。    
ヘッドレスモードは画面上にはテスト実行画面が出ないため、高速でテストできる。
***

- driven_by :selenium_chrome    
通常のGoogle Chromeブラウザを起動してシステムテストを実行する。    
ヘッドレスモードを使用しないので、実際のブラウザの動作をテストすることができる。    
その分実行が少し遅くなる可能性がある。
***
