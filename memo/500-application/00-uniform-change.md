# （検討中）実写人物の着せ替え
画像生成を用いて人物画像の衣装を変更してみたいと思う。

## 基本方針
- 1枚の画像に映る人物の衣装を変えること
- 顔は本人の面影がなるべく残ること
    - 表情は特に変更しなくてよい
- 体格が維持できてこと

## 技術方針
- 人物画像から顔mask画像を作成する
- 顔mask画像と元画像を入力として着替えた画像を生成する

## ベース画像

![](./00-uniform-change/20240209-180149-224207-2156412637.png)