---
title: "Reparameterization Trickについて"
source: "https://qiita.com/Ogin0pan/items/ce58b6da95a66c04fefe"
author:
  - "[[Ogin0pan]]"
published: 2024-12-02
created: 2025-11-03
description: "Reparameterization Trickについて IPFactory Advent Calender 2024 2日目の記事です。 Reparameterization Trick は、変分オートエンコーダー (Variational Autoencoder..."
tags:
  - "clippings"
  - "vae"
  - "reparameterization-trick"
---
![](https://relay-dsp.ad-m.asia/dmp/sync/bizmatrix?pid=c3ed207b574cf11376&d=x18o8hduaj&uid=3999280)


> [!NOTE] Reparameterization Trick
> VAE などで損失関数を最小化するために、勾配降下法を使ってパラメータを更新する必要がある。
> だが、
> $$
> 	z \approx N(\mu, \sigma^2)
> $$
> というサンプリング操作は確率的で、**乱数に依存する**非決定的な操作。だから $\frac{\partial z}{\partial \mu}$, $\frac{\partial z}{\partial \sigma}$ が計算できない。
> 具体的には、 $\mu$ を少し増やして再度実行すると、乱数に依存するので $z$ が確率的に変わってしまい、 更新によって $z$ の値がどうなったのか一貫しない。
> 一方、 $z = \mu + \sigma \cdot \epsilon, \; \epsilon \approx \mathcal{N}(0,1)$ とすれば、 $\epsilon$ は確率的だが、 $\mu$ と $\sigma$ に依存しなくなり、 $\frac{\partial z}{\partial \mu} = 1, \frac{\partial z}{\partial \sigma} = \epsilon$ となる。

IPFactory Advent Calender 2024 2日目の記事です。

Reparameterization Trick は、変分オートエンコーダー (Variational Autoencoder, VAE)  
のような深層生成モデルで使用される重要なテクニックです。  
このトリックは、サンプリング操作を微分可能にするために考案され、勾配降下法を用いた最適化を可能にします。  
本記事では、理論的な背景から実装例までを詳しく解説します。

## サンプリングの課題

VAE では、入力データ $x$ から潜在変数 $z$ を推定する必要があります。  
  
この際、潜在変数の分布 $qϕ(z|x)$ を以下のようにパラメータ化します。

ここで、 $μ$ は平均、 $σ2$ は分散を表します。この分布から直接サンプリングを行うと、  
サンプリング操作が確率的であるため、その過程をニューラルネットワークの勾配計算に利用することが困難になります。これにより、ニューラルネットワーク全体の最適化が妨げられます。

## 解決策としての Reparameterization Trick

Reparameterization Trick は、確率的なサンプリングを「再パラメータ化」することで微分可能な操作に変換します。このアプローチでは、以下のように操作を分解します

- 標準正規分布 $N(0,1)$ からサンプルを生成する
- このサンプルにスケール ( $σ$ ) とシフト ( $μ$ ) を適用して目的の分布に変換する

具体的には、以下の式で表現されます。

ここで、 $ϵ∼N(0,1)$ は標準正規分布からのサンプルです。この変換は線形操作のみを含むため、微分可能です。これにより、勾配降下法を用いた最適化が可能になります。

## Reparameterization Trick の仕組み

Reparameterization Trick を簡単に言うと、  
「サンプリング操作を数学的に分解して、微分可能な部分と確率的な部分に分ける」  
という考え方です。

- 標準正規分布からサンプリング: 標準正規分布 $N(0,1)$ は簡単にサンプリング可能であり、微分には影響しない
- スケールとシフトの適用: 平均 $μ$ と分散 $σ2$ を用いて、標準正規分布のサンプルを変換する

これにより、サンプリング自体がネットワークのパラメータ（ $μ$ と $σ$ ）に依存する形になります。これが最適化可能な形に変換する鍵です。

## 実装

### ライブラリとデータimport

```python
import torch
import torch.nn as nn
import torch.nn.functional as F
from torchvision import datasets, transforms
from torch.utils.data import DataLoader
import numpy as np
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import accuracy_score

transform = transforms.Compose([
    transforms.ToTensor()  # [0, 1] にスケール
])
train_dataset = datasets.MNIST(root='./data', train=True, download=True, transform=transform)
test_dataset = datasets.MNIST(root='./data', train=False, download=True, transform=transform)
train_loader = DataLoader(train_dataset, batch_size=64, shuffle=True)
test_loader = DataLoader(test_dataset, batch_size=64, shuffle=False)
```

- MNISTデータセット（28×28ピクセルの手書き数字画像）をダウンロード
- データを \[0, 1\] の範囲に正規化
- トレーニングデータとテストデータをそれぞれ準備

### VAEモデルの定義

```python
class VAE(nn.Module):
    def __init__(self, input_dim, latent_dim):
        super(VAE, self).__init__()
        
        self.fc1 = nn.Linear(input_dim, 128)
        self.fc_mu = nn.Linear(128, latent_dim)      # 平均
        self.fc_logvar = nn.Linear(128, latent_dim)  # 対数分散
        #入力画像を圧縮し、潜在空間の平均（mu）と対数分散（logvar）を出力

        
        self.fc2 = nn.Linear(latent_dim, 128)
        self.fc3 = nn.Linear(128, input_dim)
        # 潜在変数（z）を元に画像を再構成

    def encode(self, x):
        h1 = F.relu(self.fc1(x))
        mu = self.fc_mu(h1)
        logvar = self.fc_logvar(h1)
        return mu, logvar

    def reparameterize(self, mu, logvar):
        std = torch.exp(0.5 * logvar)  # 標準偏差
        eps = torch.randn_like(std)    # 標準正規分布からサンプリング
        return mu + eps * std
        #潜在空間からサンプリングを行うためのトリック。
        #サンプリング操作を微分可能にする。

    def decode(self, z):
        h2 = F.relu(self.fc2(z))
        return torch.sigmoid(self.fc3(h2))  # [0, 1] にスケール

    def forward(self, x):
        mu, logvar = self.encode(x)
        z = self.reparameterize(mu, logvar)
        return self.decode(z), mu, logvar
```

### 損失関数

```python
def loss_function(recon_x, x, mu, logvar):
    BCE = F.binary_cross_entropy(recon_x, x, reduction='sum')
    KLD = -0.5 * torch.sum(1 + logvar - mu.pow(2) - logvar.exp())
    return BCE + KLD
```

- 再構成誤差（BCE）
	- 再構成画像（デコーダ出力）と元の画像のピクセルごとの差を計算
- KLダイバージェンス（KLD）
	- 潜在変数の分布を標準正規分布（平均0、分散1）に近づけるための正則化

### モデルと最適化の準備

```python
input_dim = 28 * 28  # MNISTの画像データ（28x28ピクセル）
latent_dim = 20
model = VAE(input_dim, latent_dim)
optimizer = torch.optim.Adam(model.parameters(), lr=1e-3)
```

- input\_dim
	- 入力データ（MNIST画像）は28×28ピクセル
	- ネットワークに渡す際、2次元配列を1次元ベクトルにフラット化するため、  
		次元は 28×28=784
	- input\_dim = 784 がエンコーダの入力層のサイズ
- latent\_dim
	- 潜在空間の次元数を指定（ここでは20)
	- 潜在空間とは、エンコーダが高次元データを圧縮する低次元の隠れ表現のこと
	- 潜在空間の次元数はモデルの表現力と計算負荷のトレードオフに影響する
		- 小さすぎると表現力が不足
		- 大きすぎると過学習や計算コスト増加のリスク
- VAE
	- 先ほど定義されたクラスをインスタンス化して、モデルを初期化
	- このインスタンス model には以下の機能が含まれる
		- エンコーダ
			- 入力次元 input\_dim を隠れ表現 latent\_dim に圧縮
		- デコーダ
			- 潜在空間 latent\_dim から元のデータ次元 input\_dim を再構成
- input\_dim と latent\_dim の用途
	- input\_dim: エンコーダの入力層とデコーダの出力層のサイズ
	- latent\_dim: 潜在空間の次元数でエンコーダの出力層およびデコーダの入力層のサイズ

### トレーニングループ

```python
model.train()
epochs = 10
for epoch in range(epochs):
    train_loss = 0
    for data, _ in train_loader:
        data = data.view(-1, 28 * 28)
        optimizer.zero_grad()
        recon_batch, mu, logvar = model(data)
        loss = loss_function(recon_batch, data, mu, logvar)
        loss.backward()
        train_loss += loss.item()
        optimizer.step()
    print(f"Epoch {epoch + 1}, Loss: {train_loss / len(train_loader.dataset):.4f}")
```

```text
Epoch 1, Loss: 169.6315
Epoch 2, Loss: 127.2651
Epoch 3, Loss: 119.4524
Epoch 4, Loss: 115.9890
Epoch 5, Loss: 114.0870
Epoch 6, Loss: 112.8046
Epoch 7, Loss: 111.8957
Epoch 8, Loss: 111.2991
Epoch 9, Loss: 110.7355
Epoch 10, Loss: 110.3423
Test Accuracy: 0.8951
```

- 以下のコード部分で Reparameterization Trick を使用しています。

```python
def reparameterize(self, mu, logvar):
    std = torch.exp(0.5 * logvar)  # 標準偏差を計算
    eps = torch.randn_like(std)    # 標準正規分布からサンプリング
    return mu + eps * std          # サンプリングした潜在変数
```

### どのようにReparameterization Trickを使っているか

- 変分オートエンコーダー（VAE）は潜在空間を確率分布（正規分布）として表現する
- エンコーダから出力される平均（mu）と分散（logvar）を用いて潜在変数をサンプリングする
- **問題点**
	- 通常のサンプリング操作（乱数生成）は非微分可能であるため、逆伝播（backpropagation）を通じた学習ができない
- **解決方法**
	- サンプリングを「微分可能な形」に分解するのが Reparameterization Trick

### 分散（標準偏差）を計算

```python
std = torch.exp(0.5 * logvar)
```

- logvar（対数分散）を用いて標準偏差（std）を計算

$σ=exp⁡(log⁡var2)$

ここで、  
𝜎は標準偏差

### 標準正規分布から乱数をサンプリング

```python
eps = torch.randn_like(std)
```

平均0、分散1の標準正規分布（ $N(0,1)$ ）から乱数 $ϵ$ を生成

### 線形変換で潜在変数を生成

```python
return mu + eps * std
```

標準正規分布からのサンプリング結果を、以下の式で変換します

$$
z=μ+ϵ⋅σ
$$

- $μ$ : エンコーダで出力された平均
- $σ$ : 標準偏差
- $ϵ∼N(0,1)$ : 標準正規分布からの乱数

この形式にすることで、 $μ$ と $σ$ の関数として潜在変数 $z$ を生成できます。

## なぜこれがReparameterization Trickなのか

通常のサンプリング（例: torch.randn）は非微分可能ですが、mu と logvar を使ったこの変換は微分可能です。これにより、勾配降下法でエンコーダとデコーダを統一的に学習できるようになります。  

具体的には、mu と logvar に対して勾配を計算できるようになるため、損失関数が潜在変数𝑧を通じてモデル全体に逆伝播します。

## まとめ

**Reparameterization Trick** を用いることで、確率的なサンプリングを微分可能な形式に変換し、VAE などのモデルを効率的に最適化することが可能になります。  
本記事では、理論的背景と共に具体的なコード例を示しました。  
この手法は深層生成モデルを学ぶ上で避けて通れない重要な技術ですので、ぜひ実装して理解を深めてみてください。

> 参考文献  
> [https://qiita.com/pocokhc/items/d438a13d4c6ef861364a](https://qiita.com/pocokhc/items/d438a13d4c6ef861364a)  
> [https://gregorygundersen.com/blog/2018/04/29/reparameterization/](https://gregorygundersen.com/blog/2018/04/29/reparameterization/)  
> [https://leimao.github.io/blog/Reparameterization-Trick/](https://leimao.github.io/blog/Reparameterization-Trick/)  
> [https://sassafras13.github.io/ReparamTrick/](https://sassafras13.github.io/ReparamTrick/)

[0](https://qiita.com/Ogin0pan/items/#comments)

コメント一覧へ移動

X（Twitter）でシェアする

Facebookでシェアする

はてなブックマークに追加する

## Qiita Conference 2025 Autumn 11月5日(水)~7日(金)開催！

![](https://cdn.qiita.com/assets/public/official_campaigns/qiita_conference_2025_autumn/image-conference_2025_autumn_ogp-d35d2500cd0420d93f2ed69a8d162d74.png)

Qiita Conferenceは、AI時代のエンジニアに贈るQiita最大規模のテックカンファレンスです！

基調講演ゲスト(敬称略)

piacere、牛尾 剛、Esteban Suarez、和田 卓人、seya、ミノ駆動、市谷 聡啓、からあげ、岩瀬 義昌、まつもとゆきひろ、みのるん、 Null-Sensei

[イベント詳細を見る](https://qiita.com/official-campaigns/conference/2025-autumn)

## Qiita Advent Calendar 開催！

[5](https://qiita.com/Ogin0pan/items/ce58b6da95a66c04fefe/likers)

いいねしたユーザー一覧へ移動

0