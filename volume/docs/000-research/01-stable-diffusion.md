# Stable Diffusion について

## Stable Diffusion
ドイツのミュンヘン大学の研究チームが公開した**Diffusion Modelをベースとしたtext-to-imageの画像生成モデル**。v1は23億枚の画像で学習されている。
[Stable Diffusion Online](https://stablediffusionweb.com/)で試用可能。
2022年8月に Stability AI から Stable Diffusion v1 がオープンソースとして一般公開され、**だれでも無制限にAI画像生成できるようになった**ことで注目された。
ライセンスは独自のもの（[CreativeML Open RAIL-M](https://github.com/CompVis/stable-diffusion/blob/main/LICENSE)）で概ね以下の内容で商用利用は可能。

- 出力する画像は基本自由に使っていいよ、でも違法/有害なことしないでね
- 学習モデルの再配布や、商用利用も可能。その場合、同じライセンスを使う & CreativeML OpenRAIL-Mのコピーを共有する
- [参考：画像生成AI「Stable Diffusion」のLicense要約を和訳して読む](https://note.com/iwaken71/n/n1e78353f5bea)

## stable-diffusion-webui
AUTOMATIC1111氏によって開発されている、Stable Diffusionを用いたAIによる画像生成を行うことができるWebアプリ。
[AUTOMATIC1111 / stable-diffusion-webui](https://github.com/AUTOMATIC1111/stable-diffusion-webui)で公開されている。

- UIツールやAPI提供など基本的な機能は抑えられている
- 有志によるプラグイン拡張が盛ん
- Stable Diffusion以外のモデルでも利用可能（Diffusion Modelのものが利用できると思われる）
- RTX3060(VRAM12GB)以上が推奨されている
    - VRAM4GBやcpu演算でも低速動作可能
    - 利用するプラグインや生成設定によってはVRAM12GBでは不足
- ライセンスはGNU Affero General Public License v3.0なので商用利用は注意
    - [stable-diffusion-webui/LICENSE.txt](https://github.com/AUTOMATIC1111/stable-diffusion-webui/blob/master/LICENSE.txt)