# col(カラム)
bootstrapでは12のカラムに分かれる。    
ブラウザの幅によって並べ方を変えたい場合は[ブレイクポイント](https://github.com/Tarara33/TIL/blob/main/Bootstrap/%E3%83%AC%E3%82%A4%E3%82%A2%E3%82%A6%E3%83%88/%E3%83%96%E3%83%AC%E3%82%A4%E3%82%AF%E3%83%9D%E3%82%A4%E3%83%B3%E3%83%88.md)つける。

[![Image from Gyazo](https://i.gyazo.com/7ba315d949da7d8b18e883855db255fe.jpg)](https://gyazo.com/7ba315d949da7d8b18e883855db255fe)

12が上限なので、13個目のカラムは下にいく。

[![Image from Gyazo](https://i.gyazo.com/1715ec530ffe69d677d01a717d3acf90.png)](https://gyazo.com/1715ec530ffe69d677d01a717d3acf90)
***

# colの使い方
#### ⚠️ containerクラスと rowクラスを作ってから col使える
~~~
[⭕️]

<body>
  <div class='container'>
    <div class='row'>
      <div class='col'>
      </div>
    </div>
  </div>
</body>

--------------------------
[❌]

<body>
  <div class='col'>
  </div>
</body>
~~~
***

# colの大きさ調節
大きさを指定する場合は`col-1~12`で指定する。     
指定しない場合は行 ÷ 列数の数のサイズになる。

⚠️ [offset](https://github.com/Tarara33/TIL/blob/main/Bootstrap/%E3%83%AC%E3%82%A4%E3%82%A2%E3%82%A6%E3%83%88/col/offset.md) 使う場合はその colも含めた数字で並ぶ。

## 例
均等に col-4
[![Image from Gyazo](https://i.gyazo.com/bde1ac85cde5818c68cf945049e1c567.png)](https://gyazo.com/bde1ac85cde5818c68cf945049e1c567)

最後の colを数値指定なし。12以内に収まるように、col-4が適応される。
[![Image from Gyazo](https://i.gyazo.com/d961e658e74383f253364a03ad750cc3.png)](https://gyazo.com/d961e658e74383f253364a03ad750cc3)

真ん中のみ col-1指定。左右の colは自動的に col-6,col-5が割り振られて 12以内に収まる。
[![Image from Gyazo](https://i.gyazo.com/a8a270010aff2ca01fe26cfef59a162c.png)](https://gyazo.com/a8a270010aff2ca01fe26cfef59a162c)

最初２つで 12になると残りは下にいき、１つで col-12となる。
[![Image from Gyazo](https://i.gyazo.com/6cc3d1683dfffda9e2078c37140f6931.png)](https://gyazo.com/6cc3d1683dfffda9e2078c37140f6931)
***
