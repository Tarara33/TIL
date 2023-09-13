# Mailerの request spec
Mailerを併用したリクエストスペックのテストでは、下記コードを書いて、
~~~
before do
  ActionMailer::Base.deliveries.clear
end
~~~
メール送信キューをクリアする。    
これにより、前のテストで送信されたメールが削除される。  
***

# 例
~~~
require 'rails_helper'
 
RSpec.describe "PasswordResets", type: :request do
  let(:user) { FactoryBot.create(:user) }
 
  before do
    ActionMailer::Base.deliveries.clear
  end

  describe '#create' do
    context '有効なメールアドレスの場合' do
      it '送信メールが1件増えること' do
        expect {
          post password_resets_path, params: { password_reset: { email: user.email } }
        }.to change(ActionMailer::Base.deliveries, :count).by 1
      end
    end
  end
~~~
このように、メーラーの件数などのスペックで使う。
***
