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
