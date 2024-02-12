# i2iでの顔アニメーションの作成
i2iで構図を変えずに画像を修正できる。  
応用して顔アニメーションする立ち絵を作成してみる。

## ベース画像
animagine + zundalora で適当に全身絵を出力

![](./06-benchmark-i2i-simple-face-animation/20240209-222450-675952-2274485379.png)

## 差分作成
口はInpaintで口周辺を塗りつぶし(白っぽくなっている領域)。  

<img src="./06-benchmark-i2i-simple-face-animation/sc0.png" width="300">

閉じた目は単純にclosed eyesで指定すると眉毛に引っ張られるのかまぶたの位置が高くなりすぎるので、Inpaint sketchでラフに手書き。

<img src="./06-benchmark-i2i-simple-face-animation/sc1.png" width="300">

i2iを掛けて画像生成。  
Seed値はベース画像を作成したものと同じものを設定し、Denoising strengthは破綻しない程度に数値調整していく。  
目を閉じて口を開けている画像は、目を閉じて画像から口を閉じた画像を生成して作成。
<img src="./06-benchmark-i2i-simple-face-animation/20240209-223404-822349-2274485379.png" width="33%" style="vertical-align:middle;"/><img src="./06-benchmark-i2i-simple-face-animation/20240209-233137-583321-2274485379.png" width="33%" style="vertical-align:middle;"/><img src="./06-benchmark-i2i-simple-face-animation/20240209-233403-744222-2274485379.png" width="33%" style="vertical-align:middle;"/>

## アニメーション作成
transparent-backgroundで透過してImageMagick（convertコマンド）で適当につないでみる。

```
# srcにある画像を透過
transparent-background --source src --dest dst
# dstにある画像まとめて動画にする
convert -delay 20 -loop 0 ./dst/*.png out.gif
```

![](./06-benchmark-i2i-simple-face-animation/dtp.gif)

## 所感
- 単純に4枚作成して適当につないでみたが、なかなかいい感じ。細かいとこ見るとちょっと荒い。
- 手動作業多めなのでちょっとめんどくさい
- 目、口、前髪あたりをレイヤ別生成していい感じにパターンつくるプラグインかスクリプトを作れると便利かも