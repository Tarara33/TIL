rails sなどでサーバー立ち上げようとしたら
~~~
.
.
.
> uninitialized constant Nokogiri::HTML4 (NameError)
~~~
こういうエラーが出た場合、
Gemfileに下記を追加して、bundleしてから再度 rails sするとうまくいくこと多い。
~~~
[Gemfile]
gem 'nokogiri', '1.12.5'

$ bundle 
$ rails s
~~~
***
