# [アーカイブ途中] stable-diffusion-webuiの導入

### 起動手順
前提となるライブラリをインストールし、[AUTOMATIC1111 / stable-diffusion-webui](https://github.com/AUTOMATIC1111/stable-diffusion-webui)からクローンして起動。
起動後は[http://127.0.0.1:7860/](http://127.0.0.1:7860/) にアクセスするとブラウザ上でツールを操作できる。

**ubuntu上での起動**
```
sudo apt install wget git python3 python3-venv
git clone https://github.com/AUTOMATIC1111/stable-diffusion-webui
cd stable-diffusion-webui
./webui.sh
```

### 引数設定
起動時のパラメータを指定して、アプリケーションを制御できる。
linuxの場合、webui-user.shで設定可能。

**設定例**
```
export COMMANDLINE_ARGS="--xformers"
```

| 引数                                 | 説明                                                                                                                               |
| ------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------- |
| --disable-nan-check                  | 画像生成に失敗して黒画像が生成される場合に停止するかどうかを設定。長期間バッチを回す場合入れておいたほうが途中では止まらなくなる。 |
| --xformers                           | RAM節約と計算高速化に有用なライブラリを有効化。pythonのバージョンを合わせる必要がある。イマイチ恩恵がわからない。                  |
| --listen                             | 起動ホスト以外のネットワーク経由でのアクセスを許可。                                                                               |
| --api                                | apiとしての利用を許可 [API guide by @Kilvoctu](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/API)                   |
| --precision full --no-half           | 画像生成しても真っ黒になる場合に指定。                                                                                             |
| --medvram                            | 省VRAMで起動する(8GB)。VRAMを使い潰して停止する場合に指定。                                                                        |
| --lowvram                            | medvramよりも更に省VRAMで起動（4GB）                                                                                               |
| --skip-torch-cuda-test --use-cpu all | gpu検証をスキップしてcpuしか使わない                                                                                               |

### 更新方法
必要になったら実施して追記予定。gitリポジトリのpullでよいはず。

### よく触るディレクトリ
| パス                    | 説明                              |
| ----------------------- | --------------------------------- |
| models/Stable-diffusion | 画像生成モデルを配置する先        |
| models/VAE              | VAEを配置する先                   |
| embeddings              | embeddingの学習モデルを配置する先 |
| outputs                 | 生成した画像の出力先              |

### 実行ベンチマーク
- 計算条件
    - マシンA(19年度そこそこスペックマシン)
        - AMD Ryzen 5 3600 6-Core Processor : 3.6GHz 6コア / 12スレッド
        - MSI Radeon RX 5700 XT EVOKE OC : VRAM 8GB, 1690 MHz
        - memory : 80GB, 2666Mhz
    - マシンB(マシンA + 22年度ハイエンドGPU)
        - AMD Ryzen 5 3600 6-Core Processor : 3.6GHz 6コア / 12スレッド
        - TUF-RTX4090-O24G-GAMING : VRAM 24GB, 2520 MHz
        - memory : 80GB, 2666Mhz
- 実行内容
    - 30sampling
- 備考
    - モデル読み込み時間は考慮しない
    - マシンBはcudnnあたり最適化すると倍ぐらい早くなりそう

| 計算環境                                    | 計算速度 | 画像生成時間 |
| ------------------------------------------- | -------- | ------------ |
| cpu(AMD Ryzen 5 3600 6-Core Processor)      | 0.1it/s  | 300s         |
| gpu(MSI Radeon RX 5700 XT EVOKE OC + lvram) | 0.43it/s | 60s          |
| gpu(MSI Radeon RX 5700 XT EVOKE OC + mvram) | 3it/s    | 10s          |
| gpu(UF-RTX4090-O24G-GAMING)                 | 10it/s   | 3s           |
