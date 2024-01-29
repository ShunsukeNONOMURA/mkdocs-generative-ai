# 途中【SDXL】AUTOMATIC1111/stable-diffusion-webui 導入 (v1.7.0)
stable diffusion を実行する上で便利なツールである、[AUTOMATIC1111/stable-diffusion-webui](https://github.com/AUTOMATIC1111/stable-diffusion-webui)の導入について記載する。

なお、筆記時点の構築確認環境はUbuntu22.04である。

## 初期構築
Linuxの場合は[シェル](https://github.com/AUTOMATIC1111/stable-diffusion-webui?tab=readme-ov-file#automatic-installation-on-linux)が用意されているので利用すると簡単。  
wgetしたwebui.shは以降起動シェルとしても利用できる。
```
# Debian-based
sudo apt install wget git python3 python3-venv libgl1 libglib2.0-0
wget -q https://raw.githubusercontent.com/AUTOMATIC1111/stable-diffusion-webui/master/webui.sh
chmod 755 webui.sh
./webui.sh
```

webui.sh実行後にブラウザから利用できるようになるため、
Stable Diffusion checkpointを選び、Generateを押すと何かしらの絵が得られ、outputs以下に保存される。  
デフォルトだとSD1.5のモデルが利用できる。
![](./init.png)

### No module 'xformers'. Proceeding without it.
下記のようにしてxformersを利用するパラメータを加えれば良い([参考](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/5303#discussioncomment-6423824))。  
差分画像を取ってわかる程度に生成結果が変わるが、基本的には有効化しておいたほうが省メモリ化(5~10%)＋高速化(20~30%)されるので有効化しておくと良い。  
```
./webui.sh --xformers
```

webui-user.sh を下記のように作成して webui.shと同じディレクトリに入れておくとパラメータを読み取ってくれる
```
export COMMANDLINE_ARGS="--xformers"
```

## Extensions
拡張機能を追加する。

1. Extensionsに移動
1. Install from URLに移動
1. installしたい拡張機能のgit repository URLをURL for extension's git repositoryに入力してInstallをクリック
    - ControlNet for Stable Diffusion WebUI
        - https://github.com/Mikubill/sd-webui-controlnet
        - openposeとか組み合わせるためのプログラム
    - LyCORIS
        - https://github.com/KohakuBlueleaf/LyCORIS
        - 強化版Lora
1. Apply and restart UI を押して有効化する

## Settings
自分が利用しやすいように必要に応じて設定変更する。

### VAEの上書き切り替えを表示
1. Settingsに移動
1. User interfaceに移動
1. Quicksettings list に sd_vae を追加
    - デフォルトはsd_model_checkpointのみ
1. Apply settingsをクリック
1. ページを再読込すると上部に表示されるようになる
    - デフォルトはAutomatic
    - Noneを指定すると、モデルに内蔵されているVAEが利用される
    - VAEをモデルに内包したものも多いので、設定しなくても十分機能することも多い

### 保存名(日付-seed)に変更
1. Settingsに移動
1. Saving images/gridsに移動
1. Images filename patternを次のように書き換える
    - [datetime<%Y%m%d-%H%M%S-%f><Asia/Tokyo>]-[seed]
1. Add number to filename when saving のチェックを外す
1. Apply settingsをクリック

## モデル導入
SDXL用のリソースを中心に配布されているモデルを組み込んでいく。  
下記のサイトに色々と公開されているので、必要に応じてDLする。  

- https://civitai.com/
- https://huggingface.co/

<!-- < -----------------途中----------------- -->

### Baseモデル
/models/Stable-diffusion に配置  

sd_xl_base_1.0.safetensors  
https://huggingface.co/stabilityai/stable-diffusion-xl-base-1.0/tree/main?_fsi=3kT7R9AB

### VAE（Variational Auto-Encoder）
/models/VAE に配置  

sdxl_vae.safetensors  
https://huggingface.co/stabilityai/sdxl-vae/tree/main?_fsi=3kT7R9AB

### [まだ] Refinerモデル
sd_xl_refiner_1.0.safetensors  
https://huggingface.co/stabilityai/stable-diffusion-xl-refiner-1.0/tree/main?_fsi=3kT7R9AB&_fsi=3kT7R9AB

### Emmbedings
/embeddings に配置

negativeXL  
https://civitai.com/models/118418/negativexl

### Lora
/models/Lora に配置  
FlatXL  
https://huggingface.co/2vXpSwA7/iroiro-lora/blob/main/sdxl/sdxl-flat.safetensors

### Upscaler
/models/ESRGAN に配置

4x-Ultrasharp
https://civitai.com/models/116225/4x-ultrasharp
.pt -> .pthに拡張子変更する

### ControlNet
/models/ControlNet に配置

https://huggingface.co/lllyasviel/sd_control_collection/tree/main
https://huggingface.co/bdsqlsz/qinglong_controlnet-lllite/tree/main


### LyCORIS
/models/LyCORIS に配置（ない場合作成）  

BadHands SDXL (negative Lycoris/LoKr)

https://civitai.com/models/128969/badhands-sdxl-negative-lycorislokr

## 動作確認
初音ミクをパラメータを変えながら生成してみる。

sdxl系は1024×1024で学習されているので、出力画像の解像度はこのサイズに近い方が出力が安定する。

hires fixは1.5倍以内程度が安定する。

1. 960 × 1440 で出力
1. 1440 × 2160 に1.5倍高解像度化