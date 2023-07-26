# システムスペック
システムテストのことをRspecではシステムスペックと言う。    
システムテストとは、実際に使用される状況と同じ設定でテストを行い、想定通りに動作するか検証すること。
***

# 準備
- gem 'capybara'    
- gem 'webdrivers'    
- Capybaraの設定
     
※ gem 'capybara'はrails newの時入ってるらしいが一応。
~~~
$ rails s rspec:system テスト名(複数形)
~~~
テスト名はモデル名でもコントローラ名でもなく、〇〇に関するシステムテストという感じなので、    
ユーザーに関するなら「Users」、投稿に関するなら「Tasks」という感じにつける。
***

# テストを書く
~~~
[spec/system/user_sessions.rb]

require 'rails_helper'

RSpec.describe "UserSessions", type: :system do
  let(:user) {create(:user)}

  describe 'ログイン前' do
    context 'フォームの入力値が正常' do
      it 'ログイン処理が成功する' do
        visit login_path
        fill_in 'Email', with: user.email
        fill_in 'Password', with: 'password'
        click_button 'Login'
        expect(current_path).to eq root_path
        expect(page).to have_content "Login successful"
      end
    end

    context 'フォームが未入力' do
      it 'ログイン処理が失敗する' do
        visit login_path
        fill_in 'Email', with: ''
        fill_in 'Password', with: 'password'
        click_button 'Login'
        expect(page).to have_content 'Login failed'
        expect(current_path).to eq login_path
      end
    end
  end

  describe 'ログイン後' do
    context 'ログアウトボタンをクリック' do
      it 'ログアウト処理が成功する' do
        login(user)
        click_link 'Logout'
        expect(current_path).to eq root_path
        expect(page).to have_content "Logged out"
      end
    end
  end
end
~~~
- expect(page)...ブラウザに表示されているものを探す。          
- expect(current_path)...現在いるURL,またはパスを探す。
***
