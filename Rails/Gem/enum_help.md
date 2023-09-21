# 機能
[enum](https://github.com/Tarara33/TIL/blob/main/Rails/Model/%E3%83%A1%E3%83%A2/enum.md) の国際化。
***

# 使い方
## Gem導入
~~~
[gemfile]
gem 'enum_help'

$ bundle
~~~
***

## 国際化設定
モデルやカラムの国際化を設定しているファイルで設定する。
~~~
[config/locals/activerecord/ja.yml]

ja:
  enums:
    user:(enum設定してるモデル名)
      role:(enum設定してるカラム名)
        general: '一般'
        admin: '管理者'
~~~
***

## コンソールで確認
enum_helpを導入すると、便利なヘルパーも追加される。
基本`モデル名.カラム名(複数形)`で使う。  
~~~
[rails c]

$ User.roles
=> {"general"=>0, "admin"=>1}

$ User.roles_i18n
=> {"general"=>"一般", "admin"=>"管理者"}

$ User.roles_i18n.invert
=> {"一般"=>"general", "管理者"=>"admin"}  ※キーとバリューが入れ替わる！
~~~
***

