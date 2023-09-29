# SQLインジェクションとは
アプリケーションのセキュリティ上の脆弱性の一つで、    
悪意のある攻撃者がデータベースへの不正なアクセスを試みることができる攻撃手法。    
SQLインジェクションは、不適切な入力検証やエスケープの不足などのセキュリティ上の問題がある場合に悪用される。
***

## たとえば...
あるウェブサイトにログインページがあり、ユーザー名とパスワードを入力してログインできるようになっている。  
このウェブサイトは、ユーザーが提供したユーザー名とパスワードを使ってデータベース内のユーザー情報を検証している。  
このように入力した場合、ユーザー名とパスワードは以下のようにデータベースクエリに組み込まれる。  

~~~
[ログインコントローラー]

def create
  users = User.where( email =  "#{ params[:user_name] }", password = "#{ params[:password] }",)
...省略
~~~
[![Image from Gyazo](https://i.gyazo.com/ca7cc981ddb822b9078c2221eecb9582.png)](https://gyazo.com/ca7cc981ddb822b9078c2221eecb9582)

~~~
SELECT * FROM users WHERE email = 's@exsample.com'(入力された email) AND password = 'ssssss'(入力されたパスワード);
~~~
***

## しかし...
たとえば悪意あるユーザーが、email・passwordに 「OR '1'='1'」と入れたとする。
  
[![Image from Gyazo](https://i.gyazo.com/e6e02c2fc4d1bfb2a7db743026d4a8ba.png)](https://gyazo.com/e6e02c2fc4d1bfb2a7db743026d4a8ba)

そうすると、発行クエリは下記のようになる。
~~~
SELECT * FROM users WHERE email = '' OR '1'='1' AND password = '' OR '1'='1';
~~~
ORに続く 「'1'='1'」は trueを返し、正しいと評価されるため、    
攻撃者は正しいメールアドレスとパスワードを知らずに、データベース内のすべてのユーザー情報を取得できてしまう。
  
#### それを防ぐにはエスケープさせる！
***

# エスケープ と プレースホルダー
## エスケープ
意味のある特殊文字を文字列として扱うようにすること。
~~~
& → &amp;
" → &quot;
< → &li;
> → &gt
~~~
***

## プレースホルダー
データベースクエリや SQL文などで、値が後から埋められる場所を指す特殊な記号や構文。  
🩷 `?`を使う。  
プレースホルダーを使うことで、🧡部分の後から入れる値をエスケープして無害にしてから入れる。
~~~
[ログインコントローラー]

def create
  users = User.where("email = 🩷? AND password = 🩷?", 🧡params[:email], params[:password])
...省略
~~~
***

