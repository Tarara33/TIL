# BOXモデルの数値
16をベースに
- 16の倍数（32,48...）
- 半分の８
- そのまた半分の４  
が見栄えが良くなりやすい
***

#テーブルのCSS
tableタグではなくthとtdにborderをつける
その後tableタグでborder-collapseでセル間を設定する
~~~
table{
border-collapse: collapse;
}
th {
border: 1px solid black;
}
td {
border: 1px solid black;
}
~~~
***

# CSS変数
色んな要素に使う色などを変数として使えるので変更があった時も楽に変えられる
~~~
[変数の設定]
:root {
  --変数名: 設定カラー；
  }
  
[変数の使用]
h1 {
color: var(--変数名)
border: 1px solid var(--変数名);
}
~~~
などと使える    
⚠️変数名つける時は「--」つけてから
***

