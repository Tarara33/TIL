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

# RspecでのCapybaraのメソッド
itブロック内で使う。            
他のブロック内だとエラー出る。        
[参考](https://qiita.com/sogu/items/4370e8e54751899d5cad)        
[困った時](https://qiita.com/kon_yu/items/52a0f5f0016564486061)

## visit
テストで訪れる URLやパスを入れる。
~~~
visit '/login'
visit login_path
~~~
***

## fill_in '', with: ''
フォームなどの入力を行う。
~~~
fill_in 'Email', with: 'user@example.com'
fill_in 'Email', with: user.email
fill_in 'カレンダー', with: '002023-06-18-16-30'
fill_in 'カレンダー', with: DateTime.new(2023, 6, 18, 16, 30)
~~~
💡 カレンダーの日付登録は「年」の頭に「00」つける。    
もう一つの方でもできた。
***

### ⚠️ fill_in　''　どこの文字列を入れればいいか？？
~~~
[ビューファイル]
<%= form.label :email %><br />
<%= form.text_field :email %>


[検証でみたHTML]
<label for="email">Email</label><br>
<input type="text" name="email" id="email">
~~~
この場合 labelタグのテキスト「Email」でも、        
inputタグの name属性の「email」でもOK。    
        
もし labelタグがない時は nameかid属性の値を入れる。        
⚠️ nameもid属性も無ければ fill_inは使えない。
その場合は「困った時」を見てね。
***

### click_button/click_link/click_on
- click_button...ボタンを押す操作。        
- click_link...リンクを押す操作。        
- click_on...ボタンかリンクを押す。        
~~~
click_button 'Login'
click_link 'Login'
click_on 'login'
~~~
⚠️ 基本的にbuttonタグのテキストを指定するが、
~~~
<button><i>Button</i></button>
~~~
このようになってると上手くいかないので「困った時」を見てね。
***

## check/uncheck
チェックボックスをチェックする/チェックボックスのチェックを外す
~~~
check 'woman'
uncheck 'woman'
~~~
***

## select '', from: ''
セレクトボックスを選ぶ。        
select ''には選ぶ要素、from: ''はセレクトボックス名を入れる。
~~~
select 'todo', from: 'Status'
~~~
***

## within do~end
指定した要素の範囲内でのみ特定のアクションを実行することができる。       
        
例えば、複数の掲示板が表示されていて、それぞれのタイトルをクリックするテストを行う場合、        
click_on board.titleだと最初に見つかったタイトルがクリックされてしまう。        
それが意図した掲示板のタイトルでない場合、        
テストは失敗することになるので、もしクリックして欲しいものが決まってるなら within使う。
~~~
within "#board-id-1" do
  click_on board.title
end
~~~
***

## wait
非同期処理など画面の更新を待ちたい時に使う。

### 設定方法① グローバル設定値を変更する
デフォルトでは 2秒だが、変更することができる。    
「spec/spec_helper.rb」に追記する。    
~~~
[spec/spec_helper.rb]

Capybara.default_max_wait_time = 5(好きな秒数)
~~~
***

### 設定方法② オプション引数で渡す
`wait: 〇()秒数`を渡す。
~~~
find('.alert-success', text: "Success!", wait: 5)
=> 5秒待ってから探す。
~~~
***

# 画像を添付した投稿のテスト
① spec/配下に fixturesというフォルダを作成。        
② その配下にテスト用の「sample.jpg」(成功用)と「sample.txt」(失敗用)を入れる。        
        
ここからコード内        
③ テスト用ファイルまでのルート指定。        
④ attach_fileメソッドを使って、'サムネイル'というフィールドにファイルを添付する
~~~
it '掲示板が作成できること' do
    fill_in 'タイトル', with: 'テストタイトル'
    fill_in '本文', with: 'テスト本文'
    ③file_path = Rails.root.join('spec', 'fixtures', 'example.jpg')
    ④attach_file "サムネイル", file_path　　#上で定義した変数
    click_button '登録する'
    expect(current_path).to eq(boards_path)
    expect(page).to have_content('掲示板を作成しました')
    expect(page).to have_content('テストタイトル')
    expect(page).to have_content('テスト本文')
    end
    
    it '掲示板の作成に失敗すること' do
    fill_in 'タイトル', with: 'テストタイトル'
    fill_in '本文', with: nil
    ❌file_path = Rails.root.join('spec', 'fixtures', 'example.txt')
    attach_file "サムネイル", file_path
    click_button '登録する'
    expect(page).to have_content('掲示板を作成できませんでした')
    expect(page).to have_content('本文を入力してください')
~~~
❌ 失敗させる方は、画像拡張子ではないものをわざと選んで入れてる。
***

