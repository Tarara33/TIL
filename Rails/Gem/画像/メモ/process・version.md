# process・versionの違い
どちらも、carrierwaveのメソッドである。

## process(プロセス)
processの中に mini magickなどの画像加工の処理を書く。  
そして、processは上から順に実行される。
***

## version
vesion :〇〇の〇〇部分は名前は自由につけられる。    
そして version :〇〇は、オリジナルのアップロードされた画像から生成される小さなサイズのバージョンのことで、    
do ~ endには、その小さなサイズのバージョンに対して行う画像加工の処理を書く。  
***

# ❓疑問
まずアップローダーファイルにこのようなコードがあったとする。
~~~
[アップローダーファイル]

...省略
process resize_to_fill: [30, 30, "Center"]
version :rounded do
　process round
end

def round manipulate! do |img|
  img.format 'png' do |c|
    画像を円形にする処理
    end
  img
end
~~~
これは、まずフォームからアップロードされた「画像A」に対して processでリサイズをする。  
そのリサイズされた画像を小さなサイズのバージョンということで「画像A.mini」とする。  
今度は「画像A.mini」を vertionで roundedして円形にする。  


しかし、結果としてフォームからアップロードされた画像は全部リサイズされて「画像〇.mini」になるなら、  
versionで rounded処理を書かないで、下記のように書いてもいいのでは??
~~~
process resize_to_fill: [30, 30, "Center"]
process round

def round
   manipulate! do |img|
    img.format 'png' do |c|
      画像を円形にする処理
    end
  img
end
~~~
**結果:　これでもいい！！**
***

## ❓ processのみと version使う違いは？？
⭐️ なぜ、versionを使わなくてもいいのかというと、条件がないから。  
例えば、「画像の縦横比が一定以上のものだけを円形にする」、「一定の解像度以上の画像だけを円形にする」など  
条件があれば、その条件を満たすものに対して行う処理を versionを使ってかく。
~~~
[アップローダーファイル]

...省略
process resize_to_fill: [30, 30, "Center"]
version :rounded do
　process round
end

def round manipulate! do |img|
  # ここに縦横比をチェックする条件を設定
  if img.width.to_f / img.height.to_f >= 1.5

    # 画像の縦横比が1.5以上の場合のみクロップを実行
    img.format 'png' do |c|
      画像を円形にする処理
    end
  end
  img
end
~~~
このようなコードなら、まずフォームからアップロードされた「画像A」に対して processでリサイズをする。    
そのリサイズされた画像「画像A.mini」が縦横比が1.5以上なら、 version roundedを発動して「画像A.mini」を円形にする。  
もし縦横比が1.5以下なら発動せず、リサイズのみされた状態で終わる。
***

# よくある条件
- [サムネイルの設定](https://github.com/Tarara33/TIL/blob/main/Rails/Gem/%E7%94%BB%E5%83%8F/mini_magick.md#%E3%82%B5%E3%83%A0%E3%83%8D%E3%82%B5%E3%82%A4%E3%82%BA%E3%81%AE%E8%A8%AD%E5%AE%9A)
