# authenticateメソッド
has_secure_passwordメソッドをユーザーモデルで呼び出すだけで使えるようになるメソッド。    
パスワードを引数としてユーザーの認証を行うことができる。      
- 誤ったパスワードの場合　→ falseを返す
- 正しいパスワードの場合　→ そのユーザーを返す   
⚠️
~~~
ログイン分岐などで
if user.authenticate(params[:session][:password])
としたときにuserがnil(elseを辿ろうとすると)だとエラーが出るのでその場合は
user&.authenticate(params[:session][:password])
や
user && user.authenticate(params[:session][:password])　=> userが存在したら &&の後ろを実行、存在しなければそのままelseへ
とする
~~~
***

# redirect_to　と　redirect_back_or_to
- redirect_to...指定したページに行かせる
- redirect_back_or_to...次のように元々遷移しようとしていたURLにリダイレクトされる    
【redirect_back_or_toを使用したときの処理の流れの例】    
①掲示板一覧ページのURLを入力して遷移する    
②ログインしていなかったため、ログイン画面に強制遷移される   
③ログインする   
④(元々掲示板一覧ページに遷移しようとしてたため)掲示板一覧ページに遷移する    
***
