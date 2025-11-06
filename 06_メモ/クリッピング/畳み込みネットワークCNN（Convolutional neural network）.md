---
title: "畳み込みネットワークCNN（Convolutional neural network）"
source: "https://qiita.com/DeepTama/items/379cac9a73c2aed7a082"
author:
  - "[[DeepTama]]"
published: 2021-04-29
created: 2025-11-06
description: "本書は筆者たちが勉強した際のメモを、後に学習する方の一助となるようにまとめたものです。誤りや不足、加筆修正すべきところがありましたらぜひご指摘ください。継続してブラッシュアップしていきます。 © 2021 [NPO法人AI開発推進協会](https://sites.goo..."
tags:
  - "clippings"
  - "machine-learning"
  - "cnn"
---
![](https://relay-dsp.ad-m.asia/dmp/sync/bizmatrix?pid=c3ed207b574cf11376&d=x18o8hduaj&uid=3999280)

本書は筆者たちが勉強した際のメモを、後に学習する方の一助となるようにまとめたものです。誤りや不足、加筆修正すべきところがありましたらぜひご指摘ください。継続してブラッシュアップしていきます。 © 2021 \[NPO法人AI開発推進協会\](https://sites.google.com/deepaelurus.com/aboutus/home)

本書はディープラーニング手法の各種モデルのベースとなっているCNNについて、説明します。  
CNNの説明の前に初心者の方向けにDeep Learningの基本について記載します。（極力従来のアプリケーション開発を実施してきたエンジニアが理解しやすいように記載をしています)

## １．背景

現在のAI（人口知能）の発展を支えている基礎技術は機械学習です。  
機械学習は、データから正解を導き出すルール（≒Deep Learningにおける重み）を自動的に生成するもので、従来のアプリケーション開発のようにルールを設計しコーディングするプロセスとは全く異なったアプローチをします。その中でもDeep Learningは、特徴量の判断や調整を自動的に設定、学習するという特徴があります。そのため、Deep Learningは、人の認識・判断では限界があった分野（画像認識・翻訳・自動運転の分野等）での活用が特に期待されています。

[![1.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1413278/260a3dfb-d0c8-60e0-caa4-4985fea46586.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1413278%2F260a3dfb-d0c8-60e0-caa4-4985fea46586.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=8dd2e8bcda8d11151d6426e978834492)  
$$
図1　従来プログラムと機械学習の比較
$$
  
  
また、教師あり学習、教師なし学習、強化学習といった学習手法や、識別モデルや生成モデルといったモデルなど、現在でも発展が目覚ましい分野です。

## ２．Deep Learningの基礎

Deep Learningは、従来の開発でルールを階層化して実装するように、何層にも処理を重ねた形をしています。学習は各処理で持っているパラメータ（ルール＝重み）を最適化する処理です。（一般には層が多いほど精度が高くなります。これは、層が多いほど複雑な特徴を表現できるためですが、その一方で学習が複雑になるという問題があります。）  
Deep Learningに使われる演算の代表的なものとしては、データからモデルを形成する処理（Feedforward）と、パラメータを最適化（更新）するために誤差をフィードバックする処理（Backpropagation）があります。

[![2.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1413278/f584aa1c-8def-6715-55ae-a54db0c5df86.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1413278%2Ff584aa1c-8def-6715-55ae-a54db0c5df86.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=92874d3eef0c13f9e38596f30ce42c5a)  
$図2　DeepLearning概要$

  
CNNは、データを上下左右の関係をもった二次元で扱うため、画像認識に使われることが多いネットワークです。現在では画像認識だけでなくテキストや音声、感情分析など幅広い領域に応用されるようになってきています。各種モデルのベースとなっているCNNを通してDeep Learningの基本を学習してください。

## ３．CNNの概要

[![3.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1413278/357f1072-9244-146a-6329-8809f45f8a65.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1413278%2F357f1072-9244-146a-6329-8809f45f8a65.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=c69209a21600bbd45d978e14e664c1f4)  
$$
図3　CNNネットワーク概略図
$$

※Convolutionは4.1、ReLUは4.2、Max poolingは4.3、全結合は4.4章を参照してください

CNNは主に特徴を抽出する「Convolutionレイヤ（畳み込み層）」と畳み込んだデータ（特徴マップ）の解像度を下げる「Poolingレイヤ（プーリング層）」を階層化した構造をしています 。 畳み込み層とプーリング層では入力のニューロンの一部の領域を絞って、局所的に次の層へと対応付けをしていきます。また、各層はフィルタ（カーネルともいう）と呼ばれる検出器を複数持っています。 最初の層ではエッジなど低レベルな情報を検出し、層が深くなるにしたがってより抽象的な特徴を検出していきます。CNNはこういった特徴を抽出するための検出器であるフィルタのパラメータを自動で学習していきます。 最終的には出力に近い層では、全結合により分類などを行います （上記の例では、数字０～９の１０分類のスコア・確率を出力します）。

---

［参考］上記のような多クラス分類においてOne-Hot ベクトルがよく用いられます。One-Hotベクトルとは、「１」が１つ、他はすべて「０」が並ぶベクトルです。分類したいカテゴリの数と同じ次元のベクトルを用意し、対応するカテゴリに１を割り当てて表現するものです。画像分類のほかにも自然言語の単語表現など各カテゴリが独立になっている場合に用いられる表現手法です。

---

## ４．CNNの構成

## (1) 畳み込み層（Convolutionレイヤ）

脳の単純型細胞をモデル化したもので、元の画像からフィルタにより特徴点を凝縮する（画像から局所的な特徴を抽出）処理です。

### ⅰ) 畳み込み演算

畳み込み演算は、下図に示すように入力データに対してフィルタ（カーネルともいう）を適用します。入力データに対してフィルタのウィンドウを一定の間隔（ストライド）でスライドさせながら演算を行います。具体的には入力データの左上からフィルタを順次重ねてフィルタの要素と入力データの対応する要素を乗算しそれの和を求めて（積和演算という）、出力結果の対応する場所（左上から順）に格納します。

[![4.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1413278/51cff6ed-a0e3-8429-c767-d70cf45a653e.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1413278%2F51cff6ed-a0e3-8429-c767-d70cf45a653e.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=214b333e26ab1dcd2e78b55a3918a71d)  
$図4　畳み込み演算のイメージ$

### ⅱ) ゼロパディングとストライド

畳み込みを行うとそのままでは出力サイズは縮小されます（ex.上記の図では4×4 -> 3×3）。これでは畳み込みを繰り返していくとある時点で出力サイズが１になってしまいそれ以上は畳み込みが出来なくなってしまいます。これを回避するために入力データの周囲を固定のデータ（ゼロ）で埋め込むゼロパティングを行います。畳み込み演算を実施しても元のサイズを保つたり、縮小するサイズを軽減することが可能になります。

[![5.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1413278/f951921a-88ce-8446-e478-2f4da235afbe.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1413278%2Ff951921a-88ce-8446-e478-2f4da235afbe.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=5f895315b06b8459fa9a39431dbe7d2a)  
$図5　ゼロパディングの例$

  
なお、通常ディープラーニングのＡＩフレームワーク（後述）では、畳み込み演算の関数パラメータで「Same」というパラメータが用意されており、「Same」を設定すると畳み込み演算前後でのサイズは同一となるようにゼロパディングのサイズを自動調整してくれます。

一方、フィルタの間隔をストライド（stride）と言います。

[![6.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1413278/4e506bee-1dc2-79e8-1a1b-5aa5edd983a6.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1413278%2F4e506bee-1dc2-79e8-1a1b-5aa5edd983a6.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=2c947918227f22888cd452ec3a777a20)  
$図6　ストライドの例$  
  
パディングやストライドの値を考慮した出力サイズは以下の計算式で求めることが可能です。

`出力サイズ ＝ ｛（入力サイズ　＋　２×パディングサイズ － フィルタサイズ）/ ストライドサイズ　｝　＋　１　`

## (2) 活性化関数

入力信号（データ）の総和を出力信号（データ）に変換するために使用する関数です。y1を処理する関数を活性化関数h(y1)と呼びます。出力は y=h(y1)で計算されます。  
活性化関数は、全結合層や畳み込み層の計算結果を入力として、次の層へどのようにデータを伝播させるかを調整する働きを担っています。

[![7.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1413278/becbba8c-c594-8b1f-1fa5-025da33d6fba.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1413278%2Fbecbba8c-c594-8b1f-1fa5-025da33d6fba.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=17efc5a26072f4036050509268308d90)  
$図7　活性化関数$  
  
代表的な関数としては以下があります。

### ⅰ) Sigmoid（シグモイド）

出力値を0～１の間に変換します。  
[![8.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1413278/4af53a68-3bbe-c5ae-9a8b-e9a26c7ed3ae.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1413278%2F4af53a68-3bbe-c5ae-9a8b-e9a26c7ed3ae.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=4333522ab6c955109a0b8053f8983643)

### ⅱ) ReLU

出力値を入力が０以下ならば０、０を超えていればその値をそのまま出力します。Sigmoidは出力値が大きくなっても最大1までしか出力しないので学習が遅いという弱点を持っていますが、  
ReLUは入力の値が大きくなるに従って出力も大きくなるので、学習が速いというメリットがあります。

[![9.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1413278/c3fd2317-22b0-1c83-df1f-0e6b52a95bce.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1413278%2Fc3fd2317-22b0-1c83-df1f-0e6b52a95bce.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=1fdbb1a14af91fb31d97121640345aaa)

### ⅲ) tanh

出力値を-1～１の間にします。Sigmoid関数の微分が最大0.25と小さく勾配が小さくなるため学習に時間がかかる、tanhでは、  
微分の最大値が1.0となり学習時間の短縮になります。

[![10.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1413278/4918243d-5d32-0267-cd73-defc2f955f55.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1413278%2F4918243d-5d32-0267-cd73-defc2f955f55.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=6e6f65e9e306341c760ea4f59d0d1376)

これらの関数を比べると以下のようになります。

[![11.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1413278/eb1af419-871e-a3a5-227a-95d5103b6859.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1413278%2Feb1af419-871e-a3a5-227a-95d5103b6859.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=846943fb6731628e569705983f22f04e)  
$$
図　11
$$

### ⅳ) ソフトマックス関数（softmax）

ニューラルネットワークは、「分類問題」と「回帰問題」とに分類されますが、どちらに分類するかにより最終出力層の活性化関数が決まります。一般的に、「分類問題」ではソフトマックス関数を、「回帰問題」では恒等関数を用います。

① 恒等関数  
恒等関数は入力されたものに対してそのまま出力する関数です。

② ソフトマックス関数  
個々の出力が０~1.0に、出力の総和が1.0になる関数です。この性質によりソフトマックス関数の出力を「確率」として使うことができます。

$$
y=exp(ak)/∑exp(ai)
$$

  
[![12.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1413278/cda73f69-ba08-a52e-ace6-931bbe26ab2e.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1413278%2Fcda73f69-ba08-a52e-ace6-931bbe26ab2e.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=a1d48328063210f3a7496a434bc15169)  
$$
図12
$$

### ⅴ) その他活性化関数

最近のディープラーニングではsigmoidやtanhでは勾配消失問題（特に時系列を扱うRNN系のモデル）が解決できないため、「ReLU」がよく使われています。これは、ReLUであれば入力がゼロ以上であれば勾配（微分）が常に1.0となり勾配消失が起こらないためです。  
また、ReLU関数の「負の入力に対しては学習が進まない」という欠点を補うため負の入力の時はごく小さな傾きの1次関数を出力するようにした「Leaky ReLU」や、ReLU関数をX＝０のときにより滑らかにした「ELU」、さらにはReLUの後継として微分値がX=0でも連続となる「Swish」や「Mish」等様々な活性化関数があります。

最近では、画像認識に特化させた「FReLU」（Funnel Activation）も登場しています。

[![13.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1413278/e15a825f-97c1-03ae-7914-3c1a0dbe9577.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1413278%2Fe15a825f-97c1-03ae-7914-3c1a0dbe9577.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=ce483bc081a9e8496d74b3814f0c0219)  
$$
図13
$$

## (3) Poolingレイヤ

脳の複雑型細胞をモデル化したもので、空間的な位置ずれを吸収し、同一形状と見なせるように機能します。データの次元削減を行なって、計算に必要な処理コストを下げる目的があります。  
プーリングには学習という概念はなく粛々と決まった計算を行うのみです。

[![14.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1413278/1b5fb42a-d8c0-b0eb-cf81-db6bea2c9f1f.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1413278%2F1b5fb42a-d8c0-b0eb-cf81-db6bea2c9f1f.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=70bd6cd633cf47e94760ed0ad1c0c7d9)  
$図　14　MaxPoolingの例$  

上記はMax Poolingですが、これ以外にも平均を取るAverage Poolingがあります。

[![15.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1413278/821ec5b0-9d70-6c48-90e9-be47a5dbc5a2.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1413278%2F821ec5b0-9d70-6c48-90e9-be47a5dbc5a2.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=fb49397fa8c7cfc3d56e68fc8d08cb51)  
$図　15　AveragePoolingの例$

## (4) 全結合（Affine変換）

これまでの畳み込み演算とプーリングにより画像の特徴を得ましたが、特徴量を抽出するだけでは画像の識別はできません。識別には、「特徴量に基づいた分類」が必要です。この分類の役割を担っているのが、全結合層になります 。得られたそれぞれの特徴量を１つのノードに集約し（各全結合層のノード数分実施）、活性化関数を通して出力します。具体的には隣接する層のすべての出力信号（データ）を結合します。このときに隣接する層のどのチャネルからどの程度の信号を受け取るべきなのか、（分類された結果と正解データの差から学習された）重み（ｗ）およびバイアス(ｂ)を使って計算していくことになります。  
これを複数層重ね最終的に目的となる分類数のノード（数字であれば１０ノード、画像識別であれば目的とする分類クラスの数）に出力して入力された特徴量が何なのかを予測することになります。

[![16.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1413278/566abc8f-d089-df35-12c7-c8a27ba28b94.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1413278%2F566abc8f-d089-df35-12c7-c8a27ba28b94.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=61021241dccda8d3ff6817a407842723)  
$$
図16
$$

## ５．CNNの学習

学習の目的は畳み込み層や全結合層で用いる重み（やバイアス値）を学習データから自動で獲得することです。適切な重みやバイアスが学習で獲得できる＝入力データが正しく分類されることになります。学習は以下のステップで行います。

- ステップ1　入力データの処理（ミニバッチ）
- ステップ２　各重みパラメータに関する損失関数の勾配を求める
- ステップ３　重みパラメータを勾配方向に微少量だけ更新する
- ステップ４ 繰り返す

ここで「損失関数」とは、入力データに対しての正解ラベルと予測したデータとの差に  
ここで「損失関数」とは、入力データに対する正解ラベルとモデルが出力した予測とのズレ（誤差）を数値化する関数です。一般には「二乗和誤差（MSE）」や「クロスエントロピー誤差」などがよく使われます。学習のイメージとしては、この損失関数で算出した損失を元に、出力層から入力層に向かって順に各層に勾配（微分値）を伝播させ、重み（フィルタ）を更新していきます。これを誤差逆伝播（Backpropagation）といいます。

[![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1413278/ac4da151-40f0-44a8-bbc9-2261c65ed1f0.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1413278%2Fac4da151-40f0-44a8-bbc9-2261c65ed1f0.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=1b5e060bd2e6ecd0d29952c07089d059)

$$
図17
$$

## (1) 損失関数

### ⅰ) 2乗和誤差

モデルの出力 y と正解データ t をそれぞれ二乗し、その差分を誤差（損失）として返却する関数になります。

$$
E=−12∑(yk−tk)2
$$

### ⅱ) クロスエントロピー誤差

自然対数eを底とするモデル出力値のlog値と正解データ値を乗算したものの総和を、損失とします。  
  
$$
E=−∑tklogy⁡K
$$

自然対数log は、log に渡される x の値が 0 に近い時には絶対数の大きな出力になり、xの値が 1 に近いほど絶対数が 0 に近い出力になります。これは正解データ t が 1 の時にそれに対応するモデル出力予測yが 1 に近い数値を出力できていれば、tとxの乗算結果は小さくなり、xが 0 に近い誤った数値を出力していれば、tとxの乗算結果は大きくなるという論理です。

## (2) 誤差逆伝播（Backpropagation）

### ⅰ) 活性化関数

誤算逆伝播は順伝播の微分で表せられます。代表的な活性化関数の微分は以下になります。

　① Sigmoid  
$$
∂L∂y・y(1−y)
$$

　② ReLU

$$
∂y∂x=1(x>0)　　　=0(x≦0)
$$

　③ Softmax  
$$
∂y∂x=yn−tn　※tは正解ラベル
$$

### ⅱ) 畳み込み層

畳み込み層の誤差逆伝播は（畳み込み処理は関数ではないので）微分では算出できず、以下の流れになります。

　-1 フィルタの値を縦横反転する  
　-2 出力１チャネルごとに畳み込み計算を行う  
　-3 各チャネルを合計する

\[補足\]フィルタの値を縦横反転する

順伝播で関わった重みは以下のようになります。

[![18.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1413278/6467482b-edf8-9290-195c-b3b7cbf20f61.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1413278%2F6467482b-edf8-9290-195c-b3b7cbf20f61.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=ea2858e6c5cdc1f875b72aa700587254)  
$$
図18
$$
  

したがって、順伝播の重みを縦横反転したものを逆伝播に利用すれば良いことになります。

[![19.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1413278/2211ad47-6500-b310-7bda-0ffa587956f4.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1413278%2F2211ad47-6500-b310-7bda-0ffa587956f4.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=69bb6d3621042cea941f06191be8adfe)  
$$
図19
$$

\[補足\]出力１チャネルごとに畳み込み計算を行う、各チャネルを合計する

各層での計算は以下のようなイメージになります。

【順方向畳み込み】

[![20.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1413278/59655438-066f-e01d-3ddf-a694ee9f0cfd.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1413278%2F59655438-066f-e01d-3ddf-a694ee9f0cfd.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=f97d7ab29be78fe4a4f1293b1d1b605f)

【逆方向伝搬※】

逆変換したフィルタを用いて畳み込む計算を行い、それらの結果を合計してもとのＬ層に戻します。なお、順方向畳み込みでゼロパディングやスライドを用いた場合は、それらを考慮してＬ＋１層を拡大する必要があります。

※下記補足にある画像をもとの大きさに戻す「逆方向畳み込み （Deconvolution）」と区別するため、「逆方向伝搬」という言葉を使っています

[![21.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1413278/58755cac-a424-85b9-395a-8ab42bed9466.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1413278%2F58755cac-a424-85b9-395a-8ab42bed9466.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=b8196a639f6d4195b00d35288ef15723)

※Ｆ‘ <sub>n</sub> は準方向でのフィルタの位置を逆に反転したもの

[![22.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1413278/bf859265-1705-5a92-3ac5-7cc5f9da2c15.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1413278%2Fbf859265-1705-5a92-3ac5-7cc5f9da2c15.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=3e265bd3bf6cfc429c43e9da0f4e39a7)

### ⅲ) プーリング（最大プーリング）層

プーリング層はフィルタ情報がないので下層から伝播してきた誤差を更新して上層へ伝播します。　順伝播の際にウインドウサイズで選択した位置を覚えておき、逆伝播時には誤差を順伝播で選択した位置に分配しそれ以外のエリアにはゼロを設定します。

[![23.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1413278/2724fadd-1a0d-659b-d178-d2b5d953f7f8.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1413278%2F2724fadd-1a0d-659b-d178-d2b5d953f7f8.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=48dcb7260ba70cee3521a32d6446f5bc)  
$$
図20
$$

## (3) 重みパラメータの更新方法（Optimizer)

学習時の重みを差分で一気に更新してしまうと1回のミニバッチ単位での学習結果が大きく反映されてしまうことになります。そのため、ミニバッチ単位で重みを徐々に更新していく手法が取れています。以下に主な手法を説明します。

### ⅰ) SGD（確率的勾配降下法）

ミニバッチとして無作為に選ばれたデータを使用して勾配降下を行う方法です。

$$
W←W−η⋅∂L∂W
$$
 
$$
※ηは学習係数。実際には0.01や0.001といった値を前もって決めて使用します
$$

[![24.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1413278/af8407e4-49f0-df37-8a94-f72b1fc2c4fc.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1413278%2Faf8407e4-49f0-df37-8a94-f72b1fc2c4fc.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=9c04b351199444ce38d407c08ec7935d)  
$$
図21
$$

### ⅱ) Momentum（モーメンタム）

SGDに 移動平均 を適用して振動を抑制したもの。SGDのジグザグな動きを軽減することができる。  

$$
v←αv−η⋅∂L∂WW←W+v
$$

[![25.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1413278/55c682f1-4d4b-648f-7ef8-8c28c4cf88e3.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1413278%2F55c682f1-4d4b-648f-7ef8-8c28c4cf88e3.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=ffe51e62eddaa81643ec6ee8a8e28bb5)  
$$
図22
$$

### ⅲ) AdaGrad

学習係数ηの減衰（学習が進むにつれて学習係数を小さくすることで最初は大きく、次第に小さく学習する）する手法です。過去の勾配を2乗和としてすべて記録しています。  

$$
h←h+∂L∂W⊙∂L∂WW←W−η⋅1h⋅∂L∂W
$$

[![26.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1413278/f1e553ce-a9e1-227b-c626-542dbf3c1a86.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1413278%2Ff1e553ce-a9e1-227b-c626-542dbf3c1a86.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=012f38f15ad86689207e47f749bb0f2c)  
$$
図23
$$

### ⅳ) Adam

MomentumとAdaGradを融合したような手法です。

$$
mt+1=β1mt+(1−β1)∇E(wt)　vt+1=β2vt+(1−β2)∇E(wt)2　m^=mt+11−β1tv^=vt+11−β2twt+1=wt−αm^v^+ε
$$

[![28.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1413278/d44f8053-2205-ff52-0362-05ab1d4c133d.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1413278%2Fd44f8053-2205-ff52-0362-05ab1d4c133d.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=b09a00723b267625c952354fa18fc039)  
$$
図24
$$

### ⅴ) RMSProp

AdaGradを改良したアルゴリズムです。指数関数的に過去の勾配情報を忘れ、より直近の勾配情報を優先する手法です。

$$
ht=αht−1+(1−α)∇E(wt)2ηt=η0ht+ϵwt+1=wt−ηtE(wt)
$$

### ⅵ) その他

Momentumの改善版のNAG(Nesterov(ネステロフ)の加速勾配法）など改良された手法が次々に登場しています。

多くのモデルでは今でもSGDが使われてきましたが、最近ではAdamが多く使われています。ただし、すべてのモデルで優れた手法というものはなく、それぞれの手法（扱うデータにもよる）で得意・不得意があり、実際には学習して評価してみる必要があります。

## (4) パラメータの初期値

ニューラルネットワークの学習で重みの初期値は特に重要になります。以下に2つの初期値について説明します。

### ⅰ)Xavierの初期値

前層のノード（ニューロン）数をｎとした場合、1/√nの標準偏差を持つ分布を使います。活性化関数が線形であることを前提に導いたもので、sigmoid関数やtanh関数が適しています。

[![30.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1413278/38b0c1be-8f45-216b-7491-6c71248722fe.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1413278%2F38b0c1be-8f45-216b-7491-6c71248722fe.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=c491d2688176af31f2b4fc228223de70)  
$$
図25
$$

### ⅱ) Heの初期値

前層のノード（ニューロン）数をｎとした場合、√2/nの標準偏差を持つ分布を使います。ReLUに特化した初期値であり、ReLUの場合は負の領域がゼロになるため、より広がりを持たせるために２倍の係数を持たせたものです。

## ６．付録

## (1)過学習

### ⅰ) 正則化(Regularization）

機械学習では、過学習（overfitting）が発生することがあります(下図のように)。過学習とは訓練データに過度に適応しすぎてしまい、訓練データ以外のデータには対応できない状態（＝正しく識別できない）を指します。過学習を抑止するためのテクニックを正則化といい主に下記の対応方法があります。

[![31.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1413278/dd79f533-6b4e-bcd5-749c-4dcf3f0cbc3a.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1413278%2Fdd79f533-6b4e-bcd5-749c-4dcf3f0cbc3a.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=20409ae374c4670241f0f9909d3f90ab)  
$$
図26
$$

- Weight decay（荷重減衰）  
	すべての重みに対して、損失関数に1/2λＷ <sup>2</sup> （L2ノルム）を加算し、大きな重みを持つことに対してペナルティを与えることで過学習を抑止します。また、誤差逆伝播法による勾配の伝播はこの微分であるλWが伝わることになります。

### ⅱ) 正規化(Normalization）

正規化とは、特徴量の値の範囲を一定の範囲におさめる変換になります。主に\[0, 1\]　か、\[-1, 1\]の範囲内におさめることが多いです。  
例えば、\[0, 1\]におさめるとすると、特徴量CNNｘのi番目の値の変換の式は以下になります。

$$
xnorm,i=xi−xminxmax−xmin
$$

x <sub>norm</sub> が正規化された x になります。x <sub>min</sub> は x の最小値、x <sub>max</sub> は x の最大値です。  
これを計算することで、正規化する前の特徴量の最小値は正規化されて0に、最大値は1となり、新しい特徴量は \[0, 1\] におさまります。  
ノーマライズには以下の類似的な処理があります。　いずれもバッチノーマライズと処理方法は同じで、１度に正規化する範囲が異なるだけになります。

①バッチノーマライズ：は学習を速く進行させることができ、初期値にそれほど依存せず学習を抑制する(Dropout などの必要性を減らす)効果があります  
②Layerノーマライズ：１つのデータの全チャネルに対して正規化を行う  
③Instanceノーマライズ：１つのデータの１つのチャネルに対して正規化を行う  
④Groupノーマライズ：上記の中間で１つのデータの任意のチャネル数に対して正規化を行う  

[![33.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1413278/8176bf8e-9b46-92eb-7d9b-627da2bef9dd.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1413278%2F8176bf8e-9b46-92eb-7d9b-627da2bef9dd.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=44de3076f91511bd92d3b5e21d8337f5)  
$$
図27
$$
  
  
通常はバッチノーマライズを使用しますが、以下のモデルではそれぞれ以下のノーマライズが使われています。

- ＲＮＮ系やTransformerモデル　：　Layerノーマライズが多く使われている
- ＧＡＮ系モデル　　　　　　　 ：　Instanceノーマライズが多く使われている
- 画像認識系モデル　　　　　　 ：　グループノーマライズが使われている

### ⅲ) Dropout

ニューラルネットワークモデルが複雑になると、Weight decayだけでは過学習防止は困難になってきます。　そのため、Dropout（文献XXXX）を用います。　Dropoutは、学習時にニューロンをランダムに選び出し、その選び出したニューロンを消去して学習します。　また、テスト時にはすべてのニューロンを使用しますが、各ニューロンの出力に対して、訓練時に消去した割合を乗算して出力します。  

[![34.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1413278/5082e313-882e-6eba-d93b-11435aa8af6b.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1413278%2F5082e313-882e-6eba-d93b-11435aa8af6b.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=30868887b0bd17717e06aae7be9e6a19)  
$$
図28
$$

## ７．おわりに

CNNをマスタすることは、今後様々なモデルを学習・理解する基礎になると思います。記載誤り・不足等がありましたら追記して、できる限り正確かつ分かりやすくしていきたいと思います。

[2](https://qiita.com/DeepTama/items/#comments)

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

[125](https://qiita.com/DeepTama/items/379cac9a73c2aed7a082/likers)

いいねしたユーザー一覧へ移動

113