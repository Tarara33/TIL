# ❓ FormObjectとは？？
１つのフォーム送信で複数のモデルを操作したい場合や、    
テーブルに情報を保存しない情報に対するバリデーションを行いたい場合使うデザインパターンのこと。      
        
もし、FormObjectを使用しなかったら、バリデーションも自分で作らなければいけない。  
こんな感じで、大変。
~~~
class FeedbacksController < ApplicationController

  def create
    if params[:title].present? && params[:body].present?
      AdminMailer.feedback(params[:title], params[:body]).deliver_later
      redirect_to home_path, notice: 'フィードバックを送信しました'
    else
      @error_messages = []
      @error_messages << 'タイトルを入力してください' if params[:title].blank?
      @error_messages << '本文を入力してください' if params[:body].blank?
      render :new
    end
  end
end
~~~
でも、FormObjectなら、このようなコードを書かなくても、**たった１行でバリデーションが使えるようになる！**
***

# ⭐️ include ActiveModel::Model
ActiveModelは、Railsのモデルクラスが持つべき一般的な機能を提供するためのフレームワークで、    
このモジュールを　includeすると、ActiveModelの主要な機能を　FormObjectに追加できる。      
    
- バリデーション    
- エラーメッセージ    
- マスアサインメントのセキュリティ  
- フォームデータのマッピングと属性    
~~~
[クラス]
class Tarara
  include ActiveModel::Model
end
~~~
これで Tararaクラスは立派な FormObject!
***

# ⭐️ include ActiveModel::Attributes
こちらも includeするといいことあるよ！   

これは FormObjectに属性（属性値）を追加できるようになる。  
また、ゲッターセッターの役割も持つ。  

属性はフォームデータの一部を表し、    
FormObjectがその属性を持つことで、フォームデータの受け渡しとマッピングが簡単になる。    
    
設定の仕方は    
~~~
[クラス]
class Tarara
  include ActiveModel::Attributes
  
  attribute :name, :string
end
~~~
こうすることで、フォームの
~~~
<%= f.text_field :name %>
=> この:nameに対応するようになる！
~~~
***

## ❓ attr_accessor と attributeの違い
[参考](https://zenn.dev/kumasaka/articles/96c0f352b2a97f)

ネットでFormObjectについて調べると、attr_accessor使ってる人、attribute使ってる人両方いた。  
どちらがいいのか...??

### [attr_accessor](https://github.com/Tarara33/TIL/blob/main/Ruby/%E3%82%AF%E3%83%A9%E3%82%B9/%E3%82%B2%E3%83%83%E3%82%BF%E3%83%BC%E3%81%A8%E3%82%BB%E3%83%83%E3%82%BF%E3%83%BC.md#attr_accessor)
- ゲッター・セッターの役割を持つ。
- データベースや ActiveRecordとは関連がない。
- 一時的なデータ保持や、データベースに保存しない属性のために使用されることが多い。
***

### [attribute](https://github.com/Tarara33/TIL/blob/main/Ruby/%E3%82%AF%E3%83%A9%E3%82%B9/attributes.md)
- データベーステーブルのカラムに対応する属性を定義しゲッター・セッターのメソッドを生成。
- ActiveRecord の一部として動作する。
***

## 💡 結論
通常の Rubyクラスには attr_accessor を使用し、  
ActiveModelを含むクラス（例: Railsモデルやフォームオブジェクト）には attribute を使用すると良い!
***

