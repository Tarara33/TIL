# Chapter5

## RSpecってどういうものですか？
RubyにおけるBDD（何の略か忘れた）のテスティングフレーム。    
動く仕様書（Spec）として自動テストを書くと言う発想からできている。   

書き方としては
~~~
describe [仕様を記述する対象（テスト対象）], type: :[Specの種類] do
  
  context [ある状況、状態] do
    before do
      [事前準備]
    end
    
    it [仕様の内容(期待の概要)] do
      [期待する動作]
    end
  end
end
~~~

***

## RSpecとCapybaraの関係はどういったものですか？
Capybaraは、webアプリケーションのE2E（何の略か忘れた）テスト用フレームワーク。  
ブラウザの動きをシミュレートするRubyライブラリで、人が手作業でするような（ボタンクリックとか）操作をでき、動きをテストできる。   
どちらもRubyGemsパッケージとして配布されている。  
RSpecとCapybaraを組み合わせて、任意のWebページのテストを行える。
***

## FactoryBotってどういうものですか？
テスト用データの作成をサポートするgem。

~~~
[FactoryBotファイル] 例
FactoryBot.define do
  factory :user do
    name {"テストユーザー"}
    email {"test@com"}
    password {"password"}
  end
end
~~~
***

## 例
~~~
describe "タスク機能管理", type: :system do
  describe "一覧機能表示" do
    before do
      user_a = FactoryBot.crate (:user, name: "ユーザーA", email: "a@com")　
      => FactoryBotファイルで定義したのを作成する。nameなど定義と変えたい場合はこのようにこちらで記述する。
      FactoryBot.crate (:task, name: "最初のタスク", user: user_a)
    end
    
    context "ユーザーAがログインしてるとき" do
      before do
        visit login_path #ログイン画面にいく
        fill_in "メールアドレス", with: "a@com" #メアド入力
        fill_in "パスワード", with: "password"　#パス入力
        click_button "ログインする" #ボタン押す
      end
      => これらがCapybaraの動き
    
      it "ユーザーAの作成したタスクが表示される" do
        　expect(page).to have_content "最初のタスク"
      end
    end
  end
end
~~~
***


