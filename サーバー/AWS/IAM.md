# IAM
AWSにおけるユーザー。  
全ての権限を持つ rootユーザーがいるが、それで開発すると乗っ取られた時厄介。  
そのため、ユーザーの子ユーザーのような感じで、ある程度の権限を与えたユーザーが IAMユーザ  
***

# 作り方
[参考](https://blog.proglus.jp/5943/)  
  
- IAMページにいき、左の「ユーザー」を押すと、ユーザー一覧画面にいく。 
- そこでユーザー作成を押す。
- 名前とコンソールアクセスの許可を ONにする
[![Image from Gyazo](https://i.gyazo.com/6b1fadcf0b712dc87c3c334a3a59c43c.png)](https://gyazo.com/6b1fadcf0b712dc87c3c334a3a59c43c)
ここのパスワードはサインイン時のパスワーになる。(多分)  
ユーザーは次回のサインイン時...にチェック入れると、  
初回サインイン時ではここで設定したパスワード入れてその後に新しいパスワードの設定を求められる。  
  
- グループは設定してもしなくてもどちらでもOK  
よく使うポリシーをいちいち IAMユーザー作るたびにつけるのめんどいときに、    
グループでポリシー定義してれば、そのグループに入れば付与されるので楽になるって感じ。
    
- ポリシーを直接付与する場合は「AdministratorAccess”(タイプ：AWS 管理 – ジョブ機能)」を選ぶと良さげ。  
- ユーザーの作成を押す。
- ⚠️ ログインで使うパスワードと IAMユーザーの番号のファイルがダウンロードできるのでする。
- サインインする。
***

# アクセスキーの付与
本番環境で IAMユーザーのアクセスキーとシークレットアクセスキーが必要になったので忘備録。
***

## 付与の仕方
- IAMページに行き、ユーザー一覧から、付与したいユーザーを選択して詳細ページへ。
- セキュリティ認証情報を押して、アクセスキー欄で「アクセスキーを作成する」を押す。
- その他を選択
- タグ値(何に使うアクセスキーなのか)を入力しとく。
- アクセスキー作成。
- ⚠️ アクセスキーとシークレットアクセスキーのファイルがダウンロードできるのでする。  
アクセスキーは後々確認できるが、シークレットアクセスキーはできない。
[![Image from Gyazo](https://i.gyazo.com/137a4d82944b450d0e58a6f98fd66b51.png)](https://gyazo.com/137a4d82944b450d0e58a6f98fd66b51)
***
