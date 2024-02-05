# ADetailerによるキャラクター表情操作
[ADetailer](https://github.com/Bing-su/adetailer)を用いてキャラクター立ち絵の画像の表情差分を生成してみる。

## ADetailer
再描画領域を自動して、画像を再計算した後に結果をなじませる技術。  
人物画像の顔修正に利用するパターンが多い。  
下記の動画が詳しく解説している。  

<iframe width="560" height="315" src="https://www.youtube.com/embed/sF3POwPUWCE?si=fF2KRFtsaKbAWGMs" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/urNISRdbIEg?si=WMPVjyvbLmgDaxzi" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

## ベースにする画像

![](./05-benchmark-adetailer-emotion/20240205-011601-824662-1820494072-base.png)


## 設定
face_yolov8n.pt モデルを利用し顔を検出しつつ、下記のようにパラメータ調節する。

| パラメータ                 | デフォルト値 | 設定例    | 備考                                                                                                       |
| -------------------------- | ------------ | --------- | ---------------------------------------------------------------------------------------------------------- |
| Inpaint denoising strength | 0.4          | 0.4 〜 0.7 | 低すぎると変化が見えない。<br>高くなりすぎるとバランスが崩れる。<br>なるべく固定したほうが差分絵らしくなる |
| Inpaint mask blur          | 4            | 4 〜 18    | 数値を上げるとなじませるための領域が広がる。                                                               |

## 生成例

<img src="./05-benchmark-adetailer-emotion/20240205-015234-980315-1820494072-expressionlessness.png" width="25%" style="vertical-align:middle;"/><img src="./05-benchmark-adetailer-emotion/20240205-023307-144144-1820494072-smile2.png" width="25%" style="vertical-align:middle;"/><img src="./05-benchmark-adetailer-emotion/20240205-030024-909832-1820494072-repulsed.png" width="25%" style="vertical-align:middle;"/><img src="./05-benchmark-adetailer-emotion/20240205-021228-872335-1820494072-angry2.png" width="25%" style="vertical-align:middle;"/>
<img src="./05-benchmark-adetailer-emotion/20240205-030800-523189-1820494072-sad.png" width="25%" style="vertical-align:middle;"/><img src="./05-benchmark-adetailer-emotion/20240205-031316-597722-1820494072-fear2.png" width="25%" style="vertical-align:middle;"/><img src="./05-benchmark-adetailer-emotion/20240205-024252-002128-1820494072-surprised.png" width="25%" style="vertical-align:middle;"/><img src="./05-benchmark-adetailer-emotion/20240205-021616-949082-1820494072-embarrassed2.png" width="25%" style="vertical-align:middle;"/>

### 失敗例
| 髪の色が変わりすぎる                                                           | そもそも顔にならない                                                           |
| ------------------------------------------------------------------------------ | ------------------------------------------------------------------------------ |
| ![](./05-benchmark-adetailer-emotion/20240205-011934-583353-1820494072-f0.png) | ![](./05-benchmark-adetailer-emotion/20240205-013612-314176-1820494072-f1.png) |

### 所感
- 顔部分が置き換えになるので目の色や髪の色など顔に関係するプロンプトはセットで渡してしまったほうが良さそう。
    - ベース画像の時点で指示できている方が生成結果がぶれないと思われる。
- angryとか\^∀\^ みたいなやつがなかなか作りにくい。
    - 笑顔は[SD1.5だと補正Lora](https://civitai.com/models/156650/smiling-face-helper)があるのでLora欲しい。
- Inpaint denoising strength は全体的に影響が出るので、なるべく早い段階で固定したほうが差分絵らしくなる。
- 違和感の解消などの微調整でそこそこ試行錯誤した。
    - ComfyUIのようにバッチ的に生成してから選択したほうが筋が良いかも
- リアル調はパラメータを工夫しないと別人になりそうな気もする。
