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

# sequence
複数のテストデータを生成する際に活用できると便利
~~~
[factories/users.ub]

FactoryBot.define do
  factory :user do
    sequence(:name) { |n| "ごりら#{n}" }
  end
end
~~~
rails cなどで`FactoryBot.create(:user)`されるたびに    
~~~
irb(main):001:0> FactoryBot.create(:user)
=> #<User id: 11, name: "ごりら1", created_at: "2021-08-22 23:32:54", ....
irb(main):002:0> FactoryBot.create(:user)
=> #<User id: 12, name: "ごりら2", created_at: "2021-08-22 23:33:04", ....
~~~
とつくられていく
***

