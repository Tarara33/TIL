# コマンド
~~~
$ rspec
または
$ bundle exec rspec
~~~
***

# テスト種類
- システムテスト...ブラウザを使ったテスト
***


# before
beforeの内容をおく階層によって共通化できる
~~~
require "rails_helper"

describe "タスク管理機能", type: :system do
  let(:user_a) {FactoryBot.create(:user, name: "ユーザーA", email: "a@example.com")}
  let(:user_b) {FactoryBot.create(:user, name: "ユーザーB", email: "b@example.com")}
  let!(:task_a) {FactoryBot.create(:task, name: "最初のタスク", user: user_a)} #！強制で呼ぶ

  before do
    visit login_path #ログイン画面にいく
    fill_in "メールアドレス", with: login_user.email #メアドうつ
    fill_in "パスワード", with: login_user.password#パスうつ
    click_button "ログインする" #ログインボタン押す
  end
  
  describe "一覧表示機能" do
    context "ユーザーAがログインしている時" do
      let(:login_user) {user_a}

      it_behaves_like "ユーザーAが作成したタスクが表示される"
    end
~~~
ここでのbeforeは同じ階層、下の階層で描かれるbeforeに使われる（共通）    
例では一覧機能表示のcontextで行うbefore（記入省略されてる）でvisit~からの処理される
***

# let(){}
`lit(定義名){定義の内容}`で定義できる
***

# expect(期待する結果)の中身
- have_content '文字列'    
ページ内に　'　'　内の文字列がある
~~~
例：　expect(page).to have_content 'Blogs'
=> ページ内に'Blogs'と言う文字列がある
~~~
***

- have_no_content '文字列'   
ページ内に　'　'　内の文字列がない
~~~
例：　expect(page).to have_no_content 'Blogs'
=> ページ内に'Blogs'と言う文字列がない
~~~
⚠️not_toでも同じ意味になる
~~~
例：　expect(page).to have_no_content 'Blogs'
と
例：　expect(page).not_to have_content 'Blogs'
は同じ意味
~~~
***

- eq    
左で定義してること　＝　右で定義してること
~~~
例：　expect(current_path).to eq blogs_path
=> （左）current_path(現在のページ)　＝　（右）blogs_path
~~~
***

