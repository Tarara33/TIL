#  リクエストスペック
リクエストスペックはコントローラのテストのこと。
リクエストに対するレスポンスを確認することをテストする。
***

# 準備
~~~
$ rails g rspec:request テスト名
~~~
***

# テスト書く
今回は、指定したURLにリクエストしたら、望んだレスポンスが帰ってくるかのテスト。
~~~
[spec/requests/users_spec.rb]

require 'rails_helper'

RSpec.describe "Users", type: :request do
  describe "GET /signup" do
    it "正常にレスポンスを返すこと" do
      get signup_path
      expect(response).to have_http_status(:success)
    end
    
    it 'Sign up | Ruby on Rails Tutorial Sample Appが含まれること' do
     get signup_path
     expect(response.body).to include full_title('Sign up')
   end
  end
end
~~~
***
