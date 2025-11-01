---
title: "Variational Autoencoder徹底解説"
source: "https://qiita.com/kenmatsu4/items/b029d697e9995d93aa24"
author:
  - "[[kenmatsu4]]"
published: 2017-07-19
created: 2025-11-01
description: "こんにちは、DeNAでデータサイエンティストをやっているまつけんです。 今回はディープラーニングのモデルの一つ、Variational Autoencoder(VAE)をご紹介する記事です。ディープラーニングフレームワークとしてはChainerを使って試しています。 VAE..."
tags:
  - "clippings"
  - "machine-learning"
  - "vae"
---
![](https://relay-dsp.ad-m.asia/dmp/sync/bizmatrix?pid=c3ed207b574cf11376&d=x18o8hduaj&uid=3999280)


> [!NOTE] VAE解説
> 画像と動画ががめっちゃ分かりやすい。


この記事は最終更新日から1年以上が経過しています。

こんにちは、DeNAでデータサイエンティストをやっているまつけんです。

今回はディープラーニングのモデルの一つ、Variational Autoencoder(VAE)をご紹介する記事です。ディープラーニングフレームワークとしてはChainerを使って試しています。

VAEを使うとこんな感じの画像が作れるようになります。VAEはディープラーニングによる生成モデルの１つで、訓練データを元にその特徴を捉えて訓練データセットに似たデータを生成することができます。下記はVAEによって生成されたデータをアニメーションにしたものです。詳しくは本文をご覧ください。  
[![vae_all_9to9.gif](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.amazonaws.com%2F0%2F50670%2Fdee17d7b-5166-8a6e-fd4a-83fc8564549c.gif?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=cdc9b371236ea20237e2732a6deebaf4)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.amazonaws.com%2F0%2F50670%2Fdee17d7b-5166-8a6e-fd4a-83fc8564549c.gif?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=cdc9b371236ea20237e2732a6deebaf4)

本記事で紹介している内容のPythonコードは [コチラ](https://github.com/matsuken92/Qiita_Contents/tree/master/chainer-vae) にあります。

## 1\. Variational Autoencoderとは？

まず、VAEとはどんなものであるかを説明したいと思いますが、その前に通常のオートエンコーダーについて触れたいと思います。

## 1-1. 通常のオートエンコーダー

オートエンコーダーとは、

- 教師なし学習の一つ。そのため学習時の入力データは訓練データのみで教師データは利用しない。
- データを表現する特徴を獲得するためのニューラルネットワーク。

といった特徴を持ちます。  
MNISTを例にとると、28x28の数字の画像を入れて、同じ画像を出力するニューラルネットワークということになります。  
下記の図のイメージですね。　入力データ $X$ から潜在変数 $z$ に変換するニューラルネットワークをEncoderといいます。（入力データを符号化とみなせるため、Encoderという名称が付いています）この時、 $z$ の次元が入力 $X$ より小さい場合、次元削減とみなすこともできます。  
逆に潜在変数 $z$ をインプットとして元画像を復元するニューラルネットワークをDecoderと言います。

[![Normal Autoendocer](https://qiita-image-store.s3.amazonaws.com/0/50670/e3388ee5-2170-39fd-1b4f-541b5b81bd31.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.amazonaws.com%2F0%2F50670%2Fe3388ee5-2170-39fd-1b4f-541b5b81bd31.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=aedadf3c294c9bfed6f092dbc0ee62f1)

ニューラルネットが1層の場合を数式で表すと、

$$
x^(x)=f^(W^f(Wx+b)+b^)
$$

です。このときロスを

$$
Loss=∑n=1N‖xn−x^(xn)‖2
$$

とします。これはReconstruction Errorと呼ばれます。入力したデータになるべく近くなるように誤差逆伝播法で重みの更新を行うことで学習することができます。

## 1-2. Variational Autoencoder(VAE)

VAEはこの潜在変数 $z$ に確率分布、通常 $z∼N(0,1)$ を仮定したところが大きな違いです。通常のオートエンコーダーだと、何かしら潜在変数 $z$ にデータを押し込めているものの、その構造がどうなっているかはよくわかりません。VAEは、潜在変数 $z$ を確率分布という構造に押し込めることを可能にします。  
イメージは下記です。  
[![Variational Autoencoder](https://qiita-image-store.s3.amazonaws.com/0/50670/9ea8d9b7-dc55-2e58-31c3-7d303f4dae8d.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.amazonaws.com%2F0%2F50670%2F9ea8d9b7-dc55-2e58-31c3-7d303f4dae8d.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=23b45ad027a5e500c56aeeb9657917d5)

まだよくわかりませんね。実際にプログラムを動かしたものを見ると少しイメージが湧くかと思います。  
まずは入力と出力を対比させてみます。（これは $z$ の次元を20に設定して学習したものです。）ちょっとぼやっとしていますが、元の形をほとんど復元できています。もともとMNISTは784次元のデータなので、次元削減した20次元にほとんどの本質的な特徴を入れ込むことができたと言えそうです。  
[![compare_reconstruction.png](https://qiita-image-store.s3.amazonaws.com/0/50670/9ae003fd-300f-42b1-032d-f363c4784e1b.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.amazonaws.com%2F0%2F50670%2F9ae003fd-300f-42b1-032d-f363c4784e1b.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=8d34683d1bc9a6b2112c290c5ec49465)

次に、Encoder部分に限ってその挙動を見てみましょう。すでに学習済みのEncoderがあったとしてそこに訓練データのデータセットを入れて潜在変数に落としてみます。可視化できるように $z$ の次元は２次元にします。どうでしょう、訓練データのMNISTデータセットが2次元の正規分布に従う円の上に散らばっている様子が見て取れると思います。また、ここがミソなのですが、教師データのない教師なし学習にもかかわらず同じクラスラベルのデータが近いところに集まっていることも見て取れます。VAEは $z$ が正規分布に従うように設計されており、正規分布に従う乱数を学習時に取り入れているので、この乱数によるブレによって似た形状のものを近くに寄せる効果があるためです。つまり、同じ画像を入力しても毎回ちょっとずれたところに $z$ がプロットされ、その $z$ からDecoderによって生成する画像を入力画像と同じようにするためです。下記の散布図で数字がテキストでプロットされている位置は、各ラベルの中央です。  
[![Encoder](https://qiita-image-store.s3.amazonaws.com/0/50670/821398b4-ff27-7f1d-ffef-eaca2b0bdd54.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.amazonaws.com%2F0%2F50670%2F821398b4-ff27-7f1d-ffef-eaca2b0bdd54.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=108c368ebdcef13343295aa06f2dcd63)

さて、それではこの"0"から"7"に向かって潜在変数の空間上に沿ってデータを少しずつずらしながらデータを生成してみたらどうなるでしょうか。

[![scatter_with_arrow](https://qiita-image-store.s3.amazonaws.com/0/50670/5f678093-531a-64aa-7bb5-54248b0e61ad.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.amazonaws.com%2F0%2F50670%2F5f678093-531a-64aa-7bb5-54248b0e61ad.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=f0f18a82d79a9c54b99e4aa794352cb8)

下記のアニメーションがその結果です。

$$
t⋅z0+(1−t)⋅z7,  0≤t≤1
$$

のように、 $t$ を0から1までちょっとずつ動かしたものをインプットとしてDecoderによってMNIST画像を生成したものです。0からスタートして、0-6-2-8-9-4-7とちゃんとその間にある数字を経由して7になる様子がわかりますねこれらは訓練データそのものではなくN(0,1)上にマッピングされているVAEが生成した画像であることがミソです。

[![vae_0to7_dim_2.gif](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.amazonaws.com%2F0%2F50670%2F920589ba-792f-39e1-dd6f-390f18f17a55.gif?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=9ce9ac2e7f430639db14b5ab016e2bc7)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.amazonaws.com%2F0%2F50670%2F920589ba-792f-39e1-dd6f-390f18f17a55.gif?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=9ce9ac2e7f430639db14b5ab016e2bc7)

一コマずつ表示したものが下記です。  
[![download-2.png](https://qiita-image-store.s3.amazonaws.com/0/50670/e3515101-4ef7-ccba-b4a0-bbed89a38d6b.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.amazonaws.com%2F0%2F50670%2Fe3515101-4ef7-ccba-b4a0-bbed89a38d6b.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=b6ca2cefbc765fe492397eb80cdb1c51)

$z$ の2次元の空間から $−2≤z1≤2, −2≤z2≤2$ を切り出してDecoderに入力し、対応する出力画像を表示したものが下記の図になります。 赤い線が先ほどの移動の軌跡です。  
[![2dim_map.png](https://qiita-image-store.s3.amazonaws.com/0/50670/a52f8f0f-7524-86cb-bedc-0e5176ba8d75.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.amazonaws.com%2F0%2F50670%2Fa52f8f0f-7524-86cb-bedc-0e5176ba8d75.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=6e9dd2415ddbdb5f5b71ceb34a1bebb2)

訓練データよりEncoderが生成した $z$ のマップと位置関係がほぼ同じことも見て取れます。逆に、N(0, 1)の観点から非常に確率(密度)が低いところは元のデータセットからはかけ離れているということなので、対応する生成された画像が数字の形としては崩れていることはデータ生成の確率分布の構造を正しく表していると言えます。

[![scatter.png](https://qiita-image-store.s3.amazonaws.com/0/50670/47899bd7-6c61-30aa-c0a6-45a5500ec1c9.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.amazonaws.com%2F0%2F50670%2F47899bd7-6c61-30aa-c0a6-45a5500ec1c9.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=6fd7b4636d73f52fb1689d131e6f82bc)

各0〜9からスタートして、各0〜9までたどり着く様子を全て表示したものがしたの図です。この例は潜在変数の次元を20まで増やしています。なので、先ほどと違って途中経由する数字が少なくなります。次元が高いので表現力が上がっていますね。

[![vae_all_9to9.gif](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.amazonaws.com%2F0%2F50670%2Fdee17d7b-5166-8a6e-fd4a-83fc8564549c.gif?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=cdc9b371236ea20237e2732a6deebaf4)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.amazonaws.com%2F0%2F50670%2Fdee17d7b-5166-8a6e-fd4a-83fc8564549c.gif?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=cdc9b371236ea20237e2732a6deebaf4)

## 1−3. 多様体仮説

Deep Learningはタスクの学習と表現学習を合わせて行えるところが特徴の１つです。画像などの高次元のデータは、実はその高次元の中でごく一部にしかデータが分布しておらず、意味のあるデータ（訓練データの本質を捉えたデータ）はその高次元の中で局所的に固まっていると考える多様体仮説(Manifold Hypothesis)というものがあります。下記のスイスロールデータの例がわかりやすいかと思います。3次元上にデータが分布しているのですが、かなりデータが局所に偏っており、実はデータのほとんどは２次元で表現できるということが見てわかるかと思います。ロールの間にデータのない隙間があります。なので、３次元での距離が近いデータが似ているとは限らず、局面に沿った方向の移動で距離を考えたほうが類似しているものが見つかる可能性が高いということになります。なので２次元に開いて距離を測ったほうが良いということですね。

[![swiss_roll.gif](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.amazonaws.com%2F0%2F50670%2F42470953-3d66-7a5f-85f5-a22f92fb1f03.gif?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=d8b621c2fd7c9be4699bd510adbfacdc)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.amazonaws.com%2F0%2F50670%2F42470953-3d66-7a5f-85f5-a22f92fb1f03.gif?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=d8b621c2fd7c9be4699bd510adbfacdc)  
\[3次元中の２次元多様体としてのスイスロール\]  
  
[![2D_swiss_roll.png](https://qiita-image-store.s3.amazonaws.com/0/50670/96f880dc-8f8d-5837-1f51-2df7291376ea.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.amazonaws.com%2F0%2F50670%2F96f880dc-8f8d-5837-1f51-2df7291376ea.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=feb3ec78c469dbc3ab7af3a99612cf58)  
\[スイスロールを2次元に展開してみるとこんな感じ\]

話をスイスロールからMNISTのVAEに戻すと、この多様体を捉えて７８４次元のMNIST画像データの次元から潜在変数 $z∼N(0,1)$ の次元にデータ押し込めるのがVAEです。表現学習と多様体仮説については\[7\]も参考にしてください。

## 2\. Chainerでの実装

[Chainer公式のExample](https://github.com/chainer/chainer/tree/master/examples/vae) をベースにカスタマイズしたものを用いて解説します。3層のMLP(Multi Layer Perceptron)を用いてEncoderとDecoderをモデリングしています。

コードに先立って、Chainerが出力する計算グラフ(Computation Graph)を下記に示します。EncoderとDecoderの3層のMLP部分と、潜在変数 $z$ の生成に `Gaussian` つまり正規分布に従う乱数を利用しているところが見て取れます。Encoderはこの正規分布のパラメーター $μ$ と $σ2$ を出力することで $z$ を表現しています。

[![network.png](https://qiita-image-store.s3.amazonaws.com/0/50670/df0ab0bd-8c91-d5d6-cfd1-c0dc046215a7.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.amazonaws.com%2F0%2F50670%2Fdf0ab0bd-8c91-d5d6-cfd1-c0dc046215a7.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=0ae9634c114e8fab80ec94122e2c2896)

```py3
# Reference: https://jmetzen.github.io/2015-11-27/vae.html
class Xavier(initializer.Initializer):
    """
    Xavier initializaer 
    Reference: 
    * https://jmetzen.github.io/2015-11-27/vae.html
    * https://stackoverflow.com/questions/33640581/how-to-do-xavier-initialization-on-tensorflow
    """
    def __init__(self, fan_in, fan_out, constant=1, dtype=None):
        self.fan_in = fan_in
        self.fan_out = fan_out
        self.high = constant*np.sqrt(6.0/(fan_in + fan_out))
        self.low = -self.high
        super(Xavier, self).__init__(dtype)

    def __call__(self, array):
        xp = cuda.get_array_module(array)
        args = {'low': self.low, 'high': self.high, 'size': array.shape}
        if xp is not np:
            # Only CuPy supports dtype option
            if self.dtype == np.float32 or self.dtype == np.float16:
                # float16 is not supported in cuRAND
                args['dtype'] = np.float32
        array[...] = xp.random.uniform(**args)

# Original implementation: https://github.com/chainer/chainer/tree/master/examples/vae
class VAE(chainer.Chain):
    """Variational AutoEncoder"""

    def __init__(self, n_in, n_latent, n_h, act_func=F.tanh):
        super(VAE, self).__init__()
        self.act_func = act_func
        with self.init_scope():
            # encoder
            self.le1        = L.Linear(n_in, n_h,      initialW=Xavier(n_in, n_h))
            self.le2        = L.Linear(n_h,  n_h,      initialW=Xavier(n_h, n_h))
            self.le3_mu     = L.Linear(n_h,  n_latent, initialW=Xavier(n_h,  n_latent))
            self.le3_ln_var = L.Linear(n_h,  n_latent, initialW=Xavier(n_h,  n_latent))
            
            # decoder
            self.ld1 = L.Linear(n_latent, n_h, initialW=Xavier(n_latent, n_h))
            self.ld2 = L.Linear(n_h,      n_h, initialW=Xavier(n_h, n_h))
            self.ld3 = L.Linear(n_h,      n_in,initialW=Xavier(n_h, n_in))

    def __call__(self, x, sigmoid=True):
        """ AutoEncoder """
        return self.decode(self.encode(x)[0], sigmoid)

    def encode(self, x):
        if type(x) != chainer.variable.Variable:
            x = chainer.Variable(x)
        x.name = "x"
        h1 = self.act_func(self.le1(x))
        h1.name = "enc_h1"
        h2 = self.act_func(self.le2(h1))
        h2.name = "enc_h2"
        mu = self.le3_mu(h2)
        mu.name = "z_mu"
        ln_var = self.le3_ln_var(h2)  # ln_var = log(sigma**2)
        ln_var.name = "z_ln_var"
        return mu, ln_var

    def decode(self, z, sigmoid=True):
        h1 = self.act_func(self.ld1(z))
        h1.name = "dec_h1"
        h2 = self.act_func(self.ld2(h1))
        h2.name = "dec_h2"
        h3 = self.ld3(h2)
        h3.name = "dec_h3"
        if sigmoid:
            return F.sigmoid(h3)
        else:
            return h3

    def get_loss_func(self, C=1.0, k=1):
        """Get loss function of VAE.

        The loss value is equal to ELBO (Evidence Lower Bound)
        multiplied by -1.

        Args:
            C (int): Usually this is 1.0. Can be changed to control the
                second term of ELBO bound, which works as regularization.
            k (int): Number of Monte Carlo samples used in encoded vector.
        """
        def lf(x):
            mu, ln_var = self.encode(x)
            batchsize = len(mu.data)
            # reconstruction error
            rec_loss = 0
            for l in six.moves.range(k):
                z = F.gaussian(mu, ln_var)
                z.name = "z"
                rec_loss += F.bernoulli_nll(x, self.decode(z, sigmoid=False)) / (k * batchsize)
            self.rec_loss = rec_loss
            self.rec_loss.name = "reconstruction error"
            self.latent_loss = C * gaussian_kl_divergence(mu, ln_var) / batchsize
            self.latent_loss.name = "latent loss"
            self.loss = self.rec_loss + self.latent_loss
            self.loss.name = "loss"
            return self.loss
        return lf
```

## 3\. VAEの理論的な概要

生成モデルの目的は、データの分布である $p(X)$ を推定することです。PRMLの言葉を借りると下記のようになります。

> 陰または陽に出力の分布だけでなく入力の分布もモデル化するアプローチは、モデルからのサンプリングによって入力空間で人工データを生成できることから、\*\*生成モデル(Generative model)\*\*と呼ばれる」(PRML 上巻p42)

画像を対象と考えると $X$ は通常非常に高次元のデータとなります。本記事ではMNISTを対象としていますので784次元のデータです。前節でご紹介した通り、この高次元のうち、実際にデータが存在する箇所は非常に限られているため、それをうまくキャプチャして低次元の因子(例えば10次元など)の潜在変数 $z$ で表現させることを考えます。つまり高次元データ $X$ と低次元データ $z$ の対応関係を構築してうまく利用しようというものです。  
VAEはこの潜在変数 $z$ が正規分布として分布するように学習させて、 $p(X)$ を推定します。 $p(X)$ はエビデンス(Evidence)と呼ばれることもあります。

ここで、

- $p(⋅)$ ：確率モデル
- $z$ ：潜在変数
- $X$ ：データ

とします。この確率分布にパラメーターがあるとすると、 $p(X)$ に関する最尤法を用いて最もよく $X$ を表現する $p(X)$ のパラメーターを求めることができます。VAEではこの確率分布を構成する要素の一部にニューラルネットワークを用い、誤差逆伝播法と確率的勾配降下法でそのパラメーターを求めていきます。

潜在変数 $z$ とデータ $X$ の関係をグラフィカルモデルで表すとこのようになります。

[![grahical model](https://qiita-image-store.s3.amazonaws.com/0/50670/3a0a6515-1a1a-77b8-8f37-f6df0b153632.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.amazonaws.com%2F0%2F50670%2F3a0a6515-1a1a-77b8-8f37-f6df0b153632.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=b98972d8c4925ad36b70a70e6feabaa8)

データ $X$ から潜在変数 $z$ を対応づけるニューラルネットワークをEncoderと呼びます。いわゆるjpegのように画像ファイルの圧縮のアナロジーで考えるとEncoderと呼ばれる所以がわかるかと思います。  
[![encoder](https://qiita-image-store.s3.amazonaws.com/0/50670/4a9a6957-e0c3-3dad-1b33-926b08b21519.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.amazonaws.com%2F0%2F50670%2F4a9a6957-e0c3-3dad-1b33-926b08b21519.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=0251fbb1c396bd031fcf7a4f1fc85c12)

逆に潜在変数 $z$ からデータ $X$ を復元するニューラルネットワークがDecoderです。  
[![decoder](https://qiita-image-store.s3.amazonaws.com/0/50670/ae08eb76-aaca-7b26-1b4b-d31bc4a4fceb.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.amazonaws.com%2F0%2F50670%2Fae08eb76-aaca-7b26-1b4b-d31bc4a4fceb.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=8fc873dde9f6f00bb3a3f303d3c42f4e)

以上から、Encoder、Decoder、2つのニューラルネットで構成されるVAEが下記のようにモデル化できます。 $ϕ$ はEncoderのパラメータ、 $θ$ はDecoderのパラーメータです。  
ポイントは、Encoderは直接 $z$ を生成するのではなく、下記のように $z$ が従う正規分布のパラメーター $μ,σ$ を生成していることです。

[![VAE](https://qiita-image-store.s3.amazonaws.com/0/50670/3bba04a9-da64-4d7e-3c09-2007e9a29f43.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.amazonaws.com%2F0%2F50670%2F3bba04a9-da64-4d7e-3c09-2007e9a29f43.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=5e539030f33d9e94a33368438af851fe)

推定したいモデル $p(X)$ は上記のように2つのニューラルネットワークから構成されるモデルであると決定できました。あとは、データにフィットしたパラメータを求める手順です。下記のステップで考えると変分下限(Variational lower–bound) $L(X,z)$ (後述)を最大にするようなパラメータを求めれば良いとわかります。変分下限はELBO(Evidence Lower BOund)と呼ばれることもあります。

**パラメータを求める考え方のステップ**

1. $p(X)$ の尤度を最大にするニューラルネットワークのパラメータ $θ,ϕ$ を最尤法にて求める。
2. 扱いやすいように対数尤度 $log⁡p(X)$ を最大にするターゲットとする。
3. そのまま $log⁡p(X)$ を最大にすることは積分の扱いが困難であるため、変分下限 $L(X,z)$ を最大にして下から抑えに行くことで対数尤度を最大にするパラメーターを求める。

さてこの変分下限とはなんでしょうか。これは下記のように求められます。

[![eq001](https://qiita-image-store.s3.amazonaws.com/0/50670/89eaa28e-4811-b5fb-aca7-d2299b5bbecc.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.amazonaws.com%2F0%2F50670%2F89eaa28e-4811-b5fb-aca7-d2299b5bbecc.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=3f24e1cd9a8df49a9039d02078eabee8) イェンセンの不等式については\[コチラ\](http://qiita.com/kenmatsu4/items/26d098a4048f84bf85fb)も参考にしてください。

不等号があるので、 $L(X,z)$ は $log⁡p(X)$ の下限を表し、下記の図の「？」で表されるようなギャップが存在しています。

[![eq001](https://qiita-image-store.s3.amazonaws.com/0/50670/5add60bb-7b2c-3c40-b85b-0e854ed0655f.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.amazonaws.com%2F0%2F50670%2F5add60bb-7b2c-3c40-b85b-0e854ed0655f.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=90f2a892761dfa063a1173b4ba03e91a)

この「？」を求めるため対数尤度 $log⁡p(X)$ と変分下限 $L(X,z)$ の差を計算してみると下記の式より $DKL[q(z|X)|p(z|X)]$ であることがわかります。

[![fig_002](https://qiita-image-store.s3.amazonaws.com/0/50670/402fdfc8-d388-45a2-0da6-8a2340a54c83.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.amazonaws.com%2F0%2F50670%2F402fdfc8-d388-45a2-0da6-8a2340a54c83.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=737d3e220b14d2f0b18a8730954f94d1)

最後の式のところ、0以上となるのはカルバックライブラーダイバージェンスはが常に0以上のためです。

[![fig_003](https://qiita-image-store.s3.amazonaws.com/0/50670/2fe60ff8-2015-2b3a-0b78-ce5d7ce8f79a.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.amazonaws.com%2F0%2F50670%2F2fe60ff8-2015-2b3a-0b78-ce5d7ce8f79a.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=a09b8a4219f290c40df8e248c86cca26)

よって変分下限は

[![fig_004](https://qiita-image-store.s3.amazonaws.com/0/50670/39abbf6a-a0c2-7f7c-15e4-e9cf79d25663.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.amazonaws.com%2F0%2F50670%2F39abbf6a-a0c2-7f7c-15e4-e9cf79d25663.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=0eb41356d64f72964c693f08cebc744c)

となりました。第1項は固定値、第2項はパラメータに依存する項目なので、ターゲットとしていた「変分下限の最大化」はカルバックライブラーダイバージェンス $DKL[qϕ(z|X)||pθ(z|X)]$ の最小化と同値になります。

このカルバックライブラーダイバージェンス $DKL[qϕ(z|X)||pθ(z|X)]$ は、下記のように3つの項に分解できます。

[![equ_002](https://qiita-image-store.s3.amazonaws.com/0/50670/7bfa1b2f-e033-55f5-633d-c925e4e9f697.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.amazonaws.com%2F0%2F50670%2F7bfa1b2f-e033-55f5-633d-c925e4e9f697.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=16a7c7a6a0a97732ce90526fad21db15)

これを変分下限の式に代入します。

[![equ_003](https://qiita-image-store.s3.amazonaws.com/0/50670/1a5b1a52-54b1-2e96-b643-d915567078f3.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.amazonaws.com%2F0%2F50670%2F1a5b1a52-54b1-2e96-b643-d915567078f3.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=e3c567df4685cd971268737d88ab9687)

これで変分下限が２つの項で表せることがわかりました。

[![fig_005](https://qiita-image-store.s3.amazonaws.com/0/50670/7e86c0b9-2500-b499-e5c6-d72d49017ef5.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.amazonaws.com%2F0%2F50670%2F7e86c0b9-2500-b499-e5c6-d72d49017ef5.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=428c46f848509e578e6e67f230857dc6)

2つめの項は $Eq(z|X)[log⁡p(X|z)]$ で、これはEncoder、Decoderを表します。つまり、 $q(z|X)$ に関する、データ $X$ の対数尤度 $log⁡p(X|z)$ の期待値を最大化したいということです。確率分布にEncoderで推論した $z$ の分布を用い、対数尤度 $log⁡p(X|z)$ の期待値を取っています。  
Chainerで該当する部分はこちらです。

```py3
mu, ln_var = self.encode(x)
            batchsize = len(mu.data)
            # reconstruction error
            rec_loss = 0
            for l in six.moves.range(k):
                z = F.gaussian(mu, ln_var)
                z.name = "z"
                rec_loss += F.bernoulli_nll(x, self.decode(z, sigmoid=False)) / (k * batchsize)
```

$pθ(X|z)$ は多変量ベルヌーイ分布に従うと仮定し(\[3\] C.1 Bernoulli MLP as decoderより) [F.bernoulli\_nll](https://docs.chainer.org/en/stable/reference/generated/chainer.functions.bernoulli_nll.html#chainer.functions.bernoulli_nll) で損失を計算しています。つまり、各ピクセルが0から1の間の値をとるとして、VAEの出力と、入力画像でクロスエントロピーを取っていると考えられます。この損失はReconstruction Errorと呼ばれます。

[![bern_loss](https://qiita-image-store.s3.amazonaws.com/0/50670/5a40c007-cdc9-006f-f916-a60f9c4c56a6.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.amazonaws.com%2F0%2F50670%2F5a40c007-cdc9-006f-f916-a60f9c4c56a6.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=dc34a150be3777cbe08e8b0295e8e62f)

1つめの項 $DKL[q(z|X)|p(z)]$ は正則化項で、 $z$ の分布が $p(z)$ つまり $N(0,I)$ となるように制約をかけている項になります。 $q(z|X)$ と $p(z)$ の分布が近づけば近づくほどカルバックライブラーダイバージェンスの値は0に近づくので、変分下限が大きくなることになります。  
Chainerで該当する部分はこちらです。

```py3
self.latent_loss = C * gaussian_kl_divergence(mu, ln_var) / batchsize
```

'gaussian\_kl\_divergence'のコア部分は下記の通り。

```py3
var = exponential.exp(ln_var)
    mean_square = mean * mean
    loss = (mean_square + var - ln_var - 1) * 0.5
```

これは正規分布に従う時のカルバックライブラーダイバージエンスを計算しています。(\[3\] APPENDIX B参照)  
[![latent_loss](https://qiita-image-store.s3.amazonaws.com/0/50670/52a75474-bd12-940c-cf2d-b6e677d11244.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.amazonaws.com%2F0%2F50670%2F52a75474-bd12-940c-cf2d-b6e677d11244.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=499f64c840ba55c64b1e8b0e101cf2be)

（正規分布の場合のKullback Leibler Divergenceの導出については [コチラ](http://qiita.com/kenmatsu4/items/c107bd51503462fb677f) に解説記事を書きましたのでご参考としてください。）

これで最大化したいものが計算可能になりました。ニューラルネットワークの学習としては損失を最小化する価値で行うため、この変分下限にマイナスをかけたものを損失関数として、パラメーター $θ,ϕ$ の最適化を行います。

## Reparameterization Trick

最後に問題になるのが、先ほどの構造だと、 $z∼N(μ(X),σ(X))$ という確率分布が間に入っているため、誤差逆伝播法をそれより下に適用することができなくなってしまうことです。これを解決するためにReparameterization Trickという手法が用いられます。  
$z∼N(μ(X),σ(X))$ を直接扱うのではなく、 $ε∼N(0,I)$ にてノイズを発生させ、 $z=μ(X)+ε∗σ(X)$ という形でつなげることで、下記の図のようにVAEを構成し、確率変数を避けて青い矢印を逆にたどって、誤差逆伝播法を適用するものです。

[![reparameterization trick](https://qiita-image-store.s3.amazonaws.com/0/50670/3b5d2d6b-090e-8417-386c-1324c77fe2bc.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.amazonaws.com%2F0%2F50670%2F3b5d2d6b-090e-8417-386c-1324c77fe2bc.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=1b19577e2987db2aac30ec2923b9f8dd)

Chainerコードでいうと下記のあたりが該当します。

```py3
mu, ln_var = self.encode(x)
            batchsize = len(mu.data)
            # reconstruction error
            rec_loss = 0
            for l in six.moves.range(k):
                z = F.gaussian(mu, ln_var)
```

## おまけ

TensorFlowのTensorboardの機能の１つ、Embeddingを使って、20次元の潜在変数 $z$ をT-SNEで3次元二次元削減してビジュアライズしたもの。

[![Tensorboard Embedding for MNIST with VAE](http://img.youtube.com/vi/uASrkb32XDY/0.jpg)](https://youtu.be/uASrkb32XDY)

## 参考

\[1\] 本記事のPythonコード  
[https://github.com/matsuken92/Qiita\_Contents/tree/master/chainer-vae](https://github.com/matsuken92/Qiita_Contents/tree/master/chainer-vae)

\[2\] Tutorial on Variational Autoencoders  
[https://arxiv.org/abs/1606.05908](https://arxiv.org/abs/1606.05908)

\[3\] Auto-Encoding Variational Bayes(Diederik P Kingma, Max Welling)  
[https://arxiv.org/abs/1312.6114](https://arxiv.org/abs/1312.6114)

\[4\] 自然言語処理のための変分ベイズ法(Daichi Mochihashi)  
[http://chasen.org/~daiti-m/paper/vb-nlp-tutorial.pdf](http://chasen.org/~daiti-m/paper/vb-nlp-tutorial.pdf)

\[5\] 猫でも分かるVariational AutoEncoder(Sho Tatsuno)  
[https://www.slideshare.net/ssusere55c63/variational-autoencoder-64515581](https://www.slideshare.net/ssusere55c63/variational-autoencoder-64515581)

\[6\] 生成モデルの Deep Learning(Seiya Tokui)  
[https://www.slideshare.net/beam2d/learning-generator](https://www.slideshare.net/beam2d/learning-generator)

\[7\] IIBMP2016 深層生成モデルによる表現学習  
[https://www.slideshare.net/pfi/iibmp2016-okanohara-deep-generative-models-for-representation-learning](https://www.slideshare.net/pfi/iibmp2016-okanohara-deep-generative-models-for-representation-learning)

\[8\] Variational AutoEncoder  
[https://www.slideshare.net/KazukiNitta/variational-autoencoder-68705109](https://www.slideshare.net/KazukiNitta/variational-autoencoder-68705109)

\[9\] Deep Learning Chapter 17 The Manifold Perspective on Representation Learning  
[http://www.deeplearningbook.org/version-2015-10-03/contents/manifolds.html](http://www.deeplearningbook.org/version-2015-10-03/contents/manifolds.html)

[6](https://qiita.com/kenmatsu4/items/#comments)

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

[1556](https://qiita.com/kenmatsu4/items/b029d697e9995d93aa24/likers)

いいねしたユーザー一覧へ移動

1103