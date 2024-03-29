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
***

# max-height　と　min-height
- max-height...ブラウザのが高さが上がっても、指定数値以上、上がらない
- min-height...最低限の高さ。大きくなる分には勝手に高くなる。
***

# overflow
- hidden...定めた枠以上の要素はカットされる
- scroll...定めた枠以上の要素はスクロールするとみれる
***

# line-height
行の高さの設定   
pxで設定するとfont-size変更した時にややこしいので数値のみで設定する（1.5とか）  
ただし、一行の文を領域内で中央揃えにしたい時は指定してるheightのpxにする
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
[html]
<p><img href= "">こんにちは</p>

[CSS]
img {
float: right;
}
p {
display: flow-root
}
~~~
imgをp段落内で覆う
***

# background
- color...背景色
- image...`background-image: url();`で画像入れる
- size
   - px...widthとheightで指定できる
   - contain...画像がはみ出さない最大の大きさで敷き詰めてくれる
   - cover...縦横比を保ちながらはみ出してもいいのでなるべく大きく表示
- position...　画像の起点を決める(left,right,center)
coverと合わせて使うと真ん中ドアップという感じで表示できたりする
~~~
header {
backgrond-color: pink;
backgrond-image: url();
backgrond-size: cover;
backgrond-position: center;
}
[一括指定]
background: url() center/cover pink
~~~
サイズはポジションの後に「/」を入れて表示するがそれ以外は順不同
***

# text-decoration
テキストの装飾を指定する
- none...テキストの装飾を行わない（初期値）
- underline...テキストに下線を引く
- overline...テキストに上線を引く
- line-through...取り消し線を引く
***

# cursor
上をホバーした時にカーソルの形を変える   
値の種類多いので調べたほうがいい
***

# border-collapse
隣り合ったテーブルセルの間隔を指定する
- collapse...隣接するボーダーラインを重ねあわせて表示するよう指定
- separate...隣接するボーダーラインを離して表示する、初期値
***


