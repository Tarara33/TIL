# Capybaraをサポートファイルにまとめる
spec_helperはコードがたくさん書いてあって、いちいちブラウザドライバーを切り替えるの大変なので切り分ける方法。    
    
その場合は    
① spec/supportディレクトリを作成(手動)        
② その配下にcapybara.rbファイルを作成        
③ supportファイルを読み込むように rails_helperで設定    
***

## ②
~~~
[spec/support/capybara.rb]

RSpec.configure do |config|
  config.before(:each, type: :system) do
    #driven_by :rack_test
    driven_by :selenium_chrome_headless
    #driven_by :selenium_chrome
  end
end
~~~
⚠️ `require 'capybara/rspe'`は なくても動いた。        
RSpecの最新バージョンでは、Capybaraのライブラリを自動的に読み込むようになっているので書かなくていい場合があるとのこと。
***

## ③
rails_helper.rbの23行目付近にある
~~~
#Dir[Rails.root.join('spec', 'support', '**', '*.rb')].sort.each { |f| require f }
~~~
これのコメントアウトを消す。
***

