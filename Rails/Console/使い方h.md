# 入り方　と　出方
入る：`$ rails c`    
出る：`$ exit`
***

## sandboxモード
RSpecなどでコンソールを利用してテストデーターが保存されてしまうと厄介だが、    
このモードを使えば、exitすると　rollbackされて元通りになる。
~~~
$ rails c -s
~~~
***

# reload!
いちいち「exit」で出なくても`$ reload!`で再読み込みされる。   
しかし @user = User.last など変数定義してた情報もリセットされる。
***

# rails c の環境の確認
Railsにはテスト環境（test）、開発環境（development）、    
そして本番環境（production）の3つの環境がデフォルトで装備されている。    
Rails consoleのデフォルトの環境は development。
~~~
$ rails console
Loading development environment 　　　　　#developmentを読みこんでいる。
>> Rails.env
=> "development"
>> Rails.env.development?
=> true
>> Rails.env.test?
=> false
~~~
***

## 環境をオプションで渡す
テスト環境のデバッグなど、他の環境で consoleを実行する必要が生じた場合は、      
環境をオプションとして consoleスクリプトに渡すことができる。    
    
`rails c --environment 環境` とする
~~~
$ rails console --environment test
Loading test environment
>> Rails.env
=> "test"
>> Rails.env.test?
=> true
~~~
***
