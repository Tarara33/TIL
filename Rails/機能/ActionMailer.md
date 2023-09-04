# メーラーの作成・設定
## メーラー作成
~~~
$ rails g mailer UserMailer(メーラークラス名) reset_password_email(クラス内のメソッド名)
~~~
メール文の作成ファイルが「app/views/layouts/」配下に作られる。    
設定ファイルが「app/mailers/」配下に作られる。
***

## メーラーの設定
コントローラー側でメールを送信する処理が実行されると、このメーラーが働くのだと思われる。    
今回の場合は、reset_password_emailメソッドが引数としてuserオブジェクトを受け取り、    
メールテンプレートにユーザー情報やパスワードリセット用のURL、メールの宛先や件名の情報を渡している。
~~~
[app/mailers/user_mailer.rb]
class UserMailer < ApplicationMailer

  default from: 'from@example.com'
=> メールの送信元のアドレスを指定している。

-----------------------------------------------------------------

  def reset_password_email(user)
    @user = User.find(user.id)
=> ユーザーをidで見つける。

    @url = edit_password_reset_url(@user.reset_password_token)
=> reset_password_tokenカラムに保存されてる tokenを使用して URL生成する。

    mail(to: user.email, subject: 'パスワードリセット')
=> ユーザーのemailカラムのメールアドレスに送る。
   件名を subject:で決めてる。
  end
end
~~~
***

## メーラーのテキスト作成
基本的に「app/views/layouts//」配下の「mailer.text.erb」が送信に使われるが、    
「mailer.html.erb」もあるのは htmlファイルで装飾されたメールの方が見やすかったりする訳である。    
逆に顧客によってhtmlメールを嫌がる（容量が大きい、ガラケーで表示できない等）方もいるので、    
２つのファイルに記述した方が多方面性的に優れるため、両方書く。
~~~
[mailer.text.erb]
<%= @user.decorate.full_name %>様
パスワード再発行のご依頼を受け付けました。
こちらのリンクからパスワードの再発行を行ってください。
<パスワードリセット用リンク>
<%= @url %>


[mailer.html.erb]
<p><%= @user.decorate.full_name %>様</p>
<p>パスワード再発行のご依頼を受け付けました。</p>
<p>こちらのリンクからパスワードの再発行を行ってください。</p>
<p><パスワードリセット用リンク></p>
<a href = "<%= @url %>"><%= @url %></a>
~~~
***
