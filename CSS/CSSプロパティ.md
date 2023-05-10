# position
- static...初期値
- relative...初期値(static)から動かす
- absolute...
  - 何の指定もしないとブラウザで見える領域を基準に配置される
  - relativeを併用すると基準がそこからになる
  (absoluteを指定した要素囲っている要素にrelativeつける)
- fix...スクロールしてもついてくる
***

# z-index
positionプロパティがついている要素に使える(static以外)
***

# object-fit
- cover...領域内で縦横比を保ちながら、なるべく大きく表示してくれる
***

# opacity
不透明度を決める
値が０(透明)〜１(不透明)なので.3(0.3)などと書く

# max-width　と　min-width
- max-width...ブラウザの幅が広がっても、指定数値以上広がらない
- min-width...ブラウザの幅が狭まっても、指定数値以上狭まらない
- ***

# overflow
- hidden...定めた枠以上の要素はカットされる
- scroll...定めた枠以上の要素はスクロールするとみれる
***

# line-height
行の高さの設定   
pxで設定するとfont-size変更した時にややこしいので数値のみで設定する（1.5とか）
***

# vertical-align
インライン要素にのみ使える   
- +...baselineから上に要素が動く
- -...baselineから下に要素が動く   
- top...baseline からの距離ではなくて行ボックスの上上端に配置する
- bottom...baseline からの距離ではなくて行ボックスの下端に配置する
***

# float と　clear
- folat...要素を左右どちらかに寄せることができる 
- clear...floatの解除
~~~
h1 {
float: right;
}
h2 {
clear: right;
}
~~~
h1で要素が右にいくがh2では解除される
***

# display
- block...要素をブロック属性にする（縦に並ぶ）
- inline-block....要素をインラインブロック属性にする（横に並ぶ）
- inline....要素をインライン属性にする（要素の中に並ぶ）

ブロック形式変更以外の使い方
-　flow-root...floatさせた要素もブロックボックスに含める
~~~
img {
float: right;
}
p {
display: flow-root
}
~~~
imgをp段落内で覆う
***
