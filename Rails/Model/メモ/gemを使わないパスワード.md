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
空白何個か('    ')でも OKになってしまう。  
なので validatesもかけることが必要。
~~~
has_secure_password
validates :password, presence: true
~~~
⚠️ しかし、新規作成の時に、パスワードが空だったとき、  
has_secure_password分のバリデーションと  
validates :password, presence: true分のバリデーションの両方分かかるので  
「パスワードが空です」が２行でる。  
  
解決は下の +aで。
***

# ＋a ユーザー編集画面でパスワードを入れなくても　OKにする
ユーザー編集画面でこのようにパスワードも入れる画面があった場合に、
  
[![Image from Gyazo](https://i.gyazo.com/cfb15a44a2d4e68edc7bd6b2eda9f340.png)](https://gyazo.com/cfb15a44a2d4e68edc7bd6b2eda9f340)
~~~
has_secure_password
validates :password, presence: true
~~~
このコードのままだと、パスワードを入れずに更新しようとした時に、  
編集の時に`validates :password, presence: true`が発動してバリデーションかかる。  
(has_secure_passwordは新規作成の時のみ発動なので更新時は発動されない。)
  
[![Image from Gyazo](https://i.gyazo.com/09a7a575375c01313eb7fb427f36b7d2.png)](https://gyazo.com/09a7a575375c01313eb7fb427f36b7d2)
***

## 解決方法
`allow_nil: true`を追加する。
~~~
has_secure_password
validates :password, presence: true, allow_nil: true
~~~
allow_nilは trueの場合、入力hが nil(空白)だったら、バリデーションをスキップする。    
なので編集時にパスワードが入力されてなければ、スキップしてそのまま更新できる。  
  
新規作成時は、has_secure_passwordが発動する + 空白何個かだと validates :passwordが発動するので  
空白でパスワードが通ることもない。
  
なので編集時だけスキップできる。
***

### 💡 ついでに...
パスワードが空の時に「パスワードが空です」が２行でる問題があったが、    
`allow_nil: true`をつけることで、これも解決される。  
(空の時は validates :passwordの方のバリデーションがスキップされるため)
***
