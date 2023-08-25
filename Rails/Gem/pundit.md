# 機能
認可の仕組みを提供してくれる gem。  
簡単に言うと「ユーザーによってページ表示の許可・拒否をしたり、表示情報の範囲を変えたりすることができる」
***

### ❓ 認可とは？？　認証とは??
[参考](https://dev.classmethod.jp/articles/authentication-and-authorization/#return-note-184783-1)  

#### 認証  
通信の相手が誰（何）であるかを確認すること。  
今話題のマイナンバーカードを使った本人確認をイメージするとわかりやすい。  
  
純粋な認証は、それが完了したからといって何かが許される話とは関係がない。  
例えば、私にあなたのマイナンバーカードを提示して、そこに「山田太郎」と書いてあった場合、  
私は目の前にいる人が山田太郎さんであることを認める。でも、それだけ。こんにちは、山田太郎さん。（ニッコリ）
***

#### 認可
とある特定の条件に対して、リソースアクセスの権限を与えること。  
「鍵の発行」や「チケット（切符）の発行」をイメージするとわかりやすい。   
  
純粋な認可は、それがあるからといって身元が明らかになる話とは関係がない。  
駅で切符を買ったあなたは電車に乗ることを許される。それだけ。あなたが誰かは関係ない。  
また、誰かから購入済みの切符をもらった。これでもあなたは電車に乗ることができる。  
認可は、それを持っていることによって何か（リソースアクセス）が許される。  
ドアは鍵を持っていれば開くし、持っていなければ開かない。
繰り返すが、あなたが誰かは関係ない。  
***

# 使い方
## Gem導入
~~~
[gemfile]
gem 'pundit'

$ bundle
~~~
***

## 認可を利用したいコントローラーに継承する
~~~
[app/controllers/application_controller.rb]

class ApplicationController < ActionController::Base
  include Pundit
end
~~~
***

## 大元のポリシーファイル(認可の設定ファイル)を作る
~~~
$ rails g pundit:install
~~~
コレで「app/policies/」配下に「application_policy.rb」というファイルができる。
***

## application_policy.rbの編集
ここで定義した内容がデフォルト値になり、  
他のポリシーファイルにオーバーライドして使うため、細かく設定しておくと楽。
~~~
[application_policy.rb]

class ApplicationPolicy
  attr_reader :user, :record

  def initialize(user, record)
    @user = user
    @record = record
  end

  def index?
    false
  end

  def show?
    false
  end

  def create?
    false
  end

  def new?
    create?
  end

  def update?
    false
  end

  def edit?
    update?
  end

  def destroy?
    false
  end

  def scope
    Pundit.policy_scope!(user, record.class)
  end

  class Scope
    attr_reader :user, :scope

    def initialize(user, scope)
      @user = user
      @scope = scope
    end

    def resolve
      scope
    end
  end
end
~~~
【ポイント】
- メソッドに「?」ついてる
