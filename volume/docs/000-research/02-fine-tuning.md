# 【簡易記述中】ファインチューニング
- Dreambooth
    - モデル全体を更新する
    - https://dreambooth.github.io/
- Textual Inversion
    - embeddingされるtokenの重みを更新する
    - stable-diffusion-webuiではembeddingsという名前で利用される
- LoRA (Low-Rank Adaptation)
    - モデルの一部の重みを更新する
    - モデルの更新部分がpromptによって発火しない場合は生成結果に影響しない
    - 一部の更新なので学習が早い
    - https://arxiv.org/abs/2106.09685
    - https://blog.shikoan.com/lora_diffusers/
- Hypernetworks
    - 別のモデルを用意する

## 参考：Lora, Inversion, Dreambooth, Hypernetworks
![](https://preview.redd.it/well-researched-comparison-of-training-techniques-lora-v0-vl01e5grs6ca1.png?width=1080&crop=smart&auto=webp&v=enabled&s=f19779b56fc1f90484f92048dcda0812f9e0d8f0)

https://www.reddit.com/r/StableDiffusion/comments/10cgxrx/wellresearched_comparison_of_training_techniques/
