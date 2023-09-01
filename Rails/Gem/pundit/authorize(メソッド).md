# authorize(メソッド)
コントローラーでアクション内に authorizeを書くときに()の中に    
モデルやインスタンスではなく、メソッドを書くことがあった。    
  
メソッドは applicationコントローラーに定義されていた。  
~~~
[app/controllers/site/attachmments_controller.rb]

def destroy
  authorize(current_site)
end


[app/controllers/application_controller.rb]

def current_site
    @current_site ||= Site.first
end
~~~
この  `authorize(current_site)`の動きとしては、  
current_siteメソッドで @current_siteに入った Siteモデルのインスタンスが
特定のアクションを実行できるか確認してる。

Siteモデルなので認可を確認するのは「app/policies/site_policy.rb」
~~~
[app/policies/site_policy.rb]

def destroy?
  user.admin?
end
~~~
adminなら、destroyアクションができる！
***
