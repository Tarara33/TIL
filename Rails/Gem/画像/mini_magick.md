[参考](https://qiita.com/wonder_boooy/items/1510f8750d1282693148)  

# 準備
~~~
[Gemfile]

gem 'mini_magick'
$ bundle
~~~
***

# carrierwave + mini_magickでできること（一部）
mini_magick使う時は
~~~
[app/uploaders/〇〇_uploader.rb]

include CarrierWave::MiniMagick
~~~
を記述してから画像のリサイズなど書く。。
***

## resize_to_fit(width, height)
画像を指定したサイズにリサイズする。    
アスペクト比は保持されるが、指定したサイズに収まるように変更される。
~~~
process resize_to_fit: [300, 200]
~~~
***

## resize_to_limit(width, height)
画像を指定したサイズにリサイズする。  
アスペクト比は保持されますが、指定したサイズを超えないように変更される。 
~~~
process resize_to_limit: [300, 200]
~~~
resize_to_fitとの違いは、   
対象の画像のサイズが引数に指定された縦横サイズ以内の場合はリサイズしない。   
例：　元の画像サイズが [200, 150] だった場合は、リサイズせずにそのまま表示される。   
***

### 余白の設定できる
リサイズ後のファイルの余白部分を指定色でぬりつぶすこともできる。    
第三引数で塗りつぶしの色を指定し、第四引数で余白が発生した際の画像の配置を指定。
~~~
process resize_to_limit: [300, 200, "#ffffff", "Center"]
~~~
***

## resize_to_fill(width, height, gravity)
画像を指定したサイズにリサイズする。    
アスペクト比は無視され、指定したサイズに合うように画像が切り取られる。
~~~
process resize_to_fill: [100, 100, "Center"]
~~~
画像の端っこが一部切れてしまうこともあるが、    
投稿した画像サイズがバラバラでも全て同じサイズにリサイズされ、縦横比の調整によって画像が横に表示されることも無いので、   
画像投稿一覧を表示させるにはこのresize_to_fillが相性が良いかも。
***

[![Image from Gyazo](https://i.gyazo.com/440b3209e8d9553f7eab24ef68e88c43.png)](https://gyazo.com/440b3209e8d9553f7eab24ef68e88c43)
***
