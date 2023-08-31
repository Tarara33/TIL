# パスワードをもつ（ハッシュで暗号化する）model
~~~
class User < ApplicationRecord
  has_secure_password
~~~
と`has_secure_password`と入れる    
するとデータベースカラムには対応しない、「password」「password_confirmation」と言う２つの属性が追加される   
- 「password」...ユーザーが入力した生のパスワードを一時的に格納する場所    
- 「password_confirmation」...確認としてもう一度パスワードを入れてもらい先ほど入れたものと合っているか    
一時的に確認用パスワードを格納する   
***

## has_secure_passwordを使う際の注意
⚠️データベースにテーブルを作る時にパスワードを格納するカラム名は`password_digest`とする     
passwordなど他の名前にするとhas_secure_passwordが発動されなくて暗号化されずに生のままカラムに入ってしまう       
    
⚠️ gem bcryptをダウンロードする必要ある    
***

# has_secure_passwordのバリデーション
オブジェクト**生成時**にパスワードの存在性を検証する。  
💡 アップデートなどでは検証しない。  
    
しかし、このバリデーションは空パスワードを持つレコードにしか適用されないので、  
空白('    ')でも OKになってしまう。  
なので validatesもかけることが必要。
~~~
has_secure_password
validates :password, presence: true
~~~
***
