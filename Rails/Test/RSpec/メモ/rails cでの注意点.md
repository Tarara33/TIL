# rails c はサンドボックスモードで
例えば、このようなFactoryBotファイルを設定して、    
そのデーターを確認したくて rails cで入力すると、
~~~
[spec/factries/user.rb]
FactoryBot.define do
  factory :user do
    sequence(:email) { |n| "user_#{n}@example.com" }
    password {'password'}
    password_confirmation {'password'}
  end
end


[rails c]
$ user = FactoryBot.craete(:user)
=> => #<User:0x0000000108e233b8
 id: 3,
 email: "user_1@example.com",
 crypted_password:...
~~~
実際にデータベースにコレが登録されてしまう。    
    
そして exitしてもう一度同じことをすると、    
FactoryBotはまた「user_1@example.com」から作ろうとするので、    
emailにユニーク制度かけていた場合、そのemailは存在しますとエラーが出る。    
    
無駄にテストデータも増えてしまうし、テストが動かなくなったり厄介なので    
基本的にRspecのデーターはデータベースに登録しない。    
そのためサンドボックスモードにする。
***
