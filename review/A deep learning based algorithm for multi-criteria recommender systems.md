---
title: 多基準レコメンダーシステムのためのディープラーニングベースのアルゴリズム
tags:
  - thesis
  - memo
  - paper
  - deep-learning
  - deep-autoencoders
  - collaborative-filtering
  - multi-criteria-recommender-systems
created: 2025-05-11
source: "[[A deep learning based algorithm for multi-criteria recommender systems.pdf]]"
---
## Abstract
近年、レコメンダーシステムは、パーソナライズされた推薦を提供することにより、情報過多の問題に対処するために非常に普及しています。 <font color="#c0504d">多基準レコメンダーシステムは、単一基準レコメンダーシステムと比較して、より正確な推薦を行うことが証明されています。</font>なぜなら、多基準評価は、多くの側面から見たアイテムに対するユーザーの評価を反映しているからです。 一方で、深層学習技術は、画像処理、コンピュータービジョン、パターン認識、自然言語処理など、多くの研究分野で有望な性能を達成しています。 
最近、レコメンダーシステムにおける深層学習の応用が、有望な結果とともに頻繁に研究されています。 したがって、<font color="#c0504d">本論文では、多基準レコメンダーシステムのための深層学習ベースアルゴリズムを提案します</font>。このアルゴリズムでは、ディープオートエンコーダーを用いて、多基準の好みに関するユーザー間の自明でない、非線形かつ隠れた関係を抽出し、より正確な推薦を生成します。 Yahoo! MoviesおよびTripAdvisorの多基準データセットを用いた実験は、提案アルゴリズムが、最先端の推薦アルゴリズムと比較して、より正確な予測を行うという点で非常に効果的であることを証明しています。
## Gemini Summary

## Memo

> [!PDF|yellow] [[A deep learning based algorithm for multi-criteria recommender systems.pdf#page=1&selection=39,21,40,101&color=yellow|A deep learning based algorithm for multi-criteria recommender systems, p.1]]
> > deep learning techniques achieve promising performance in many research areas such as image processing, computer vision, pattern recognition and natural language processing.
> 
> ディープラーニングが得意な領域(成功を収めている領域)は、画像認識、コンピュータービジョン、パターン認識、自然言語処理である。これから、研究内容を考えるとっかかりを得れるかも。

> [!PDF|yellow] [[A deep learning based algorithm for multi-criteria recommender systems.pdf#page=1&selection=60,55,64,66&color=yellow|A deep learning based algorithm for multi-criteria recommender systems, p.1]]
> >  In order to resolve the information overload problem, recommender system emerges to assist the users in making the proper decision by analysing the users’ preference information and recommend the items of interest to them according to their preferences [1–4]
> 
> 推薦アルゴリズムはユーザーが直面する情報過多問題を解決する手段として有効であり、そこに推薦アルゴリズムの価値が存在する。

> [!PDF|yellow] [[A deep learning based algorithm for multi-criteria recommender systems.pdf#page=1&selection=70,0,73,7&color=yellow|A deep learning based algorithm for multi-criteria recommender systems, p.1]]
> > CF techniques suffer from known problems such as cold start, accuracy of predictions and incapability of capturing complex user–item interactions when the rating matrix is very sparse [3,5]. 
> 
> 協調フィルタリングは、
> **「コールドスタート、予測の精度、及び評価行列のスパース性が非常に高い場合の複雑なユーザーとアイテムの相互作用を捉える能力の欠如など」**
> の既知の問題に悩まされているらしい。これが課題。

> [!PDF|red] [[A deep learning based algorithm for multi-criteria recommender systems.pdf#page=1&selection=99,11,126,1&color=red|A deep learning based algorithm for multi-criteria recommender systems, p.1]]
> >  DL aims to automatically mine multi-level feature representations from data by merging low-level features to form denser high-level semantic abstractions, therefore enable the codification of more versatile abstractions as data representations in the higher layers [9,10]. 
> 
> ディープラーニングは低レベルの特徴を結合してより密な高レベルのセマンティック抽象を形成することにより、データから多レベルの特徴表現を自動的にマイニングすることを目的としている。それによって、より高い層でデータ表現としてより汎用性の高い抽象をコード化することを可能にしている。
> 
> ディープラーニングについては今後しっかりと調べていく必要があるな。

> [!PDF|yellow] [[A deep learning based algorithm for multi-criteria recommender systems.pdf#page=1&selection=126,1,138,35&color=yellow|A deep learning based algorithm for multi-criteria recommender systems, p.1]]
> > Lately, DL techniques have been utilized and began to gain huge attention in recommender systems. The reason is that DL techniques can exploit the complex and nonlinear user–item relationships among users and items, which can led to enhancements of the performance in generating accurate recommendations
> 
> 近年ディープラーニングはレコメンダーシステムにおいて大きな注目を集めているそう。その理由は、DL技術がユーザーとアイテムの間の複雑で非線形な関係を抽出でき、正確な推薦を生成する性能を向上させることができるから。

> [!PDF|yellow] [[A deep learning based algorithm for multi-criteria recommender systems.pdf#page=2&selection=12,0,18,1&color=yellow|A deep learning based algorithm for multi-criteria recommender systems, p.2]]
> > In this paper, we propose a deep learning based algorithm for multi-criteria recommender systems. 
> 
> この論文では、多基準レコメンダーシステムのための深層学習ベースアルゴリズムを提案する。これがこの論文の新規性。

> [!PDF|red] [[A deep learning based algorithm for multi-criteria recommender systems.pdf#page=2&selection=72,0,74,20&color=red|A deep learning based algorithm for multi-criteria recommender systems, p.2]]
> > The deep autoencoder is a special type of the deep neural network, whose input vectors and the output vectors have the same dimensionality. It is a nonlinear feature extraction method used for learning a representation of the original data at hidden layers.
> 
> ディープオートエンコーダーは深層ニューラルネットワークの特殊タイプ。入力ベクトルと出力ベクトルの次元が同じ特徴を持つ。隠れ層で元のデータの表現を学習するために使われる**非線形特徴抽出法**。

> [!PDF|red] [[A deep learning based algorithm for multi-criteria recommender systems.pdf#page=2&selection=110,35,282,1&color=red|A deep learning based algorithm for multi-criteria recommender systems, p.2]]
> > A simple autoencoder is called as a deep autoencoder when there are more than one hidden layer. An autoencoder is trained using one of the back-propagation functions, usually the stochastic gradient descent method. An autoencoder consists of two main parts: an encoder part that maps the input into the code, and a decoder part that maps the code to a reconstruction of the original input as shown in Eqs. (1) and (2), respectively [14,15]. h = g(Wx + b) (1) x′ = f (W1h + b1) (2) where g(.) and f (.) are nonlinear activation functions, x is the input data, h is the encoded data, W ∈ RH×D is the weight matrix Fig. 1. A simple autoencoder [10]. between the input layer and hidden layer, W1 ∈ RD×H is the weight matrix between the hidden layer and outer layer, b is biased vector for the hidden layer, and b1 is biased vector for the outer layer. AutoEncoder is trained to minimize the reconstruction error between x and x′. For this purpose, this study uses the L1 loss function (Least absolute deviations), which is formulated as follows: L (x, x′) =  x − x′  =  x − f (W1g (Wx + b) + b1) 
> 
> ここはディープラーニングの説明。隠れ層が複数ある場合、シンプルなオートエンコーダーはディープオートエンコーダーと呼ばれる。オートエンコーダーは、**通常確率的勾配降下法**などの誤差逆伝播法の一つを使用して学習される。今回の研究では、L1損失関数(最小絶対偏差)を使用。

> [!PDF|red] [[A deep learning based algorithm for multi-criteria recommender systems.pdf#page=2&selection=288,0,377,3&color=red|A deep learning based algorithm for multi-criteria recommender systems, p.2]]
> > The main concept of the recommender system is to build a utility function that can evaluate the fitness of recommending a set of items i ∈ I to a set of users u ∈ U. The utility function is described as U × I → R, where R is a positive integer or real number within a certain range. Accordingly, each user u has to estimate the utility function R(u, i) for item i for which R(u, i) is not yet known, and then select one or a set of items i that will maximize R(u, i) as shown by Eq. (4) [16–18]. ∀u ∈ U, i = arg max i∈I R(u, i) (4)
> 
> レコメンダーシステムの定式化方法。ユーザーの集合にアイテムの集合を推薦。そこで、その推薦の適合性を評価できる効用関数を構築することを目的とする。つまり、効用関数はU×I → Rと記述され、Rは特定の範囲内の正の整数または実数。

> [!PDF|yellow] [[A deep learning based algorithm for multi-criteria recommender systems.pdf#page=3&selection=8,0,12,45&color=yellow|A deep learning based algorithm for multi-criteria recommender systems, p.3]]
> > The strength of DL models in discovering hidden features open the doors to produce effective solutions to the limitations of conventional recommender systems such as accuracy, sparsity and scalability. Thus, the adoption of DL models in recommender systems became a favourite and trending topic
> 
>精度、スパース性、スケーラビリティなどが従来のレコメンダーシステムの限界として存在していた。しかし、ディープラーニングの特徴である隠れ層はその限界に対する効果的な解決策になり得るようだ。

> [!PDF|yellow] [[A deep learning based algorithm for multi-criteria recommender systems.pdf#page=4&selection=11,0,17,62&color=yellow|A deep learning based algorithm for multi-criteria recommender systems, p.4]]
> > To summarize, current literature reveals that DL techniques can afford more accurate recommendations than conventional recommendation algorithms. However, there has still been little research work on exploring DL techniques to model MCRSsto better improve their performance. This paper examines the significance of deep autoencoders as a DL technique in improving the prediction accuracy of multi-criteria recommender systems.
> 
> これまでの文章で現在までの研究を説明している。まとめると、DL技術が従来の推薦アルゴリズムよりも比較的正確な推薦を提供できることを明らかにした。
> しかし、MCRSのパフォーマンスをより向上させるためにDL技術を探求する研究は未だ少ない。
> この論文では、多基準レコメンダーシステムの予測精度を向上させるうえで、DL技術としてのディープオートエンコーダーの重要性を検証する。

> [!PDF|yellow] [[A deep learning based algorithm for multi-criteria recommender systems.pdf#page=4&selection=99,0,126,0&color=yellow|A deep learning based algorithm for multi-criteria recommender systems, p.4]]
> > argmin U ∑ u=1 ∥ru − f (ru)∥ + λ. regularization
> 
> 深層オートエンコーダーモデルの目的損失関数はこのように定義される。
> 観測された評価への過学習を防ぐためにL1正則化が採用されている。
> r_uは観測された評価で構成されるユーザーuの評価ベクトルで、f(r_u)は再構築された評価ベクトル、λは正則化項である。

 > [!PDF|yellow] [[A deep learning based algorithm for multi-criteria recommender systems.pdf#page=4&selection=147,0,149,46&color=yellow|A deep learning based algorithm for multi-criteria recommender systems, p.4]]
> > The AEMC algorithm has four main phases to produce recommendations. In the first phase, the dataset is prepared in order to train and test the deep autoencoder model. 
> 
> AEMCアルゴリズムには、推薦を生成するための4つの主要フェーズがある。一つに、深層オートエンコーダーモデルをトレーニング、テストするためにデータセットが準備される。

> [!PDF|yellow] [[A deep learning based algorithm for multi-criteria recommender systems.pdf#page=4&selection=149,58,152,63&color=yellow|A deep learning based algorithm for multi-criteria recommender systems, p.4]]
> >  the numbers of users and items are extracted from the dataset and arranged in a two dimensional array for each rating criteria, where rows represent the users and columns represent the items.
> 
> ユーザーとアイテムの数をデータセットから抽出し、各評価基準について2次元配列を作る。行はユーザー数を表し、列はアイテムを表す。

> [!PDF|red] [[A deep learning based algorithm for multi-criteria recommender systems.pdf#page=4&selection=153,0,153,65&color=red|A deep learning based algorithm for multi-criteria recommender systems, p.4]]
> > The dataset is split into 80% training data and 20% testing data.
> 
> データセットは80%の訓練データと20%のテストデータに分割される。

> [!PDF|yellow] [[A deep learning based algorithm for multi-criteria recommender systems.pdf#page=4&selection=156,0,162,56&color=yellow|A deep learning based algorithm for multi-criteria recommender systems, p.4]]
> > The second phase is the deep autoencoder construction for each criterion-based user–item matrix with all essential hyperparameters. Architecturally, the form of the autoencoder is a feedforward neural network with an input layer, more than one hidden layer and an output layer. The input and output size is same and is the number of unique items in the dataset. The number of hidden layer is fixed arbitrary to 3, 5 and 7.
>
> 2番目のフェーズは、すべての必須ハイパーパラメータを使用して、各基準ベースのユーザーアイテムマトリックスの深層オートエンコーダー構築です 。アーキテクチャ的には、オートエンコーダーの形式は、入力層、複数の隠れ層、および出力層を備えた順伝播ニューラルネットワークです 。入力および出力サイズは同じであり、データセット内の一意のアイテムの数です 。隠れ層の数は任意に3、5、7に固定されています 。

 > [!PDF|yellow] [[A deep learning based algorithm for multi-criteria recommender systems.pdf#page=4&selection=164,20,166,44&color=yellow|A deep learning based algorithm for multi-criteria recommender systems, p.4]]
> > The inner layers use sigmoid as an activation function, as shown by Eq. (6). The outmost layers use ELU as an activation function, as shown by Eq. (7).
> 
> 入力層は活性化関数としてシグモイド関数を使用。出力層は活性化関数としてELUを使用する。

> [!PDF|yellow] [[A deep learning based algorithm for multi-criteria recommender systems.pdf#page=4&selection=175,0,183,11&color=yellow|A deep learning based algorithm for multi-criteria recommender systems, p.4]]
> > network parameter optimizer is RMSprop, which is based on the gradient descent optimizer. The optimizer object (RMSprop) is constructed with specific parameters such as the L1 loss function, the learning rate and weight decay are set to 0.001 and 0.1 respectively.
> 
> ネットワークパラメータオプティマイザはRMSpropであり、これは勾配降下オプティマイザに基づいています。 オプティマイザオブジェクト（RMSprop）は、L1損失関数、学習率、重み減衰などの特定のパラメータで構築され、それぞれ0.001と0.1に設定されます。
> ↑ なにをいってるん？

> [!PDF|yellow] [[A deep learning based algorithm for multi-criteria recommender systems.pdf#page=4&selection=237,0,238,35&color=yellow|A deep learning based algorithm for multi-criteria recommender systems, p.4]]
> > In the third phase the deep autoencoder is trained and tested using the prepared dataset in phase 1
> 
> 第三フェーズはフェーズ1で整えたデータセットを使って学習し、テストさせる。

実験フェーズは一旦スキップ。

## 結論と今後の課題

> [!PDF|red] [[A deep learning based algorithm for multi-criteria recommender systems.pdf#page=7&selection=15,0,60,66&color=red|A deep learning based algorithm for multi-criteria recommender systems, p.7]]
> > This paper proposed a deep learning based algorithm for multi-criteria recommender systems, which captures the nonlinear and non-trivial user–item relationships to better learn the user preferences and then improves the accuracy of recommendations. Specifically, a deep autoencoder-based multi-criteria recommendation algorithm is developed to enable the codification of more composite abstractions as data representations in the higher layers, in addition to the multi-criteria preferences of users, to better model and learn the fine-grained user–item interactions. Experimental results have revealed the effectiveness of the proposed AEMC recommendation algorithm in terms of recommendation accuracy with significant enhancement with respect to the state-of-the-art baseline algorithms. Future directions will be to extend the proposed algorithm by adopting other DL-based techniques and incorporating additional information source, and study their effects on the quality of recommendations.
> 
> 本稿では、多基準推薦システムのための深層学習ベースのアルゴリズムを提案した。このアルゴリズムは、ユーザーの嗜好をよりよく学習し、推薦の精度を向上させるために、非線形かつ自明ではないユーザーとアイテムの関係を捉えるものである 。具体的には、深層オートエンコーダベースの多基準推薦アルゴリズムを開発し、ユーザーの多基準の嗜好に加えて、より複合的な抽象化を上位層のデータ表現として符号化できるようにすることで、きめ細かいユーザーとアイテムの相互作用をよりよくモデル化し学習することを可能にした 。実験結果は、提案されたAEMC推薦アルゴリズムが、最先端のベースラインアルゴリズムと比較して、推薦精度において大幅な向上を伴う有効性を示した 。今後の方向性としては、他の深層学習ベースの手法を採用したり、追加の情報源を組み込んだりして提案アルゴリズムを拡張し、それらが推薦の質に与える影響を研究することである 。

## 現状の課題と今後の展望

### 現状の課題

- **情報過多問題**: 製品、書籍、ホテル、映画、レストランなどのサービスに関する様々なリソースが急速に発展するにつれて、個人が直面する情報フローの爆発的な増加が顕著になっています 。
    
- **協調フィルタリング（CF）技術の限界**: CFは推薦システムで最も一般的に使用される技術ですが、コールドスタート問題、予測の精度、評価行列が非常にスパースな場合の複雑なユーザーとアイテムの相互作用を捉える能力の欠如といった既知の問題に悩まされています 。
    
- **単一基準評価の制約**: ほとんどのCF技術は、全体的な評価（単一基準評価）を利用して推薦を生成しますが、これはユーザーの好みに関する詳細な分析を表現できないため、制限と見なされ得ます。これにより、ユーザーは正確な推薦を得られない可能性があります 。
    
- **深層学習と多基準推薦システムの統合における研究不足**: 深層学習（DL）技術は従来の推薦アルゴリズムよりも正確な推薦を提供できることが示されていますが、その性能をさらに向上させるために、多基準推薦システム（MCRS）をモデリングするためにDL技術を探求する研究はまだ少ないと指摘されています 。
    

### 今後の展望

- **他の深層学習（DL）ベースの技術の採用**: 提案されたアルゴリズムを、他のDLベースの技術を導入することで拡張する 。
    
- **追加の情報源の組み込み**: 推薦の質を向上させるために、追加の情報源をアルゴリズムに組み込む 。
    
- **推薦の質への影響の研究**: 上記の拡張と情報源の追加が推薦の質に与える影響について研究する 。


## The first words
- pertain to： ~に関連する
- information overload probrem：情報過多問題
- **collaborative filtering：協調フィルタリング = CF**
- sparse array：疎行列 [[研究用語]]
- alleviate：～を軽減する
- multi-criteria recommender systems(MCRSs)：多基準レコメンダーシステム
- codification：コード化
- versatile：汎用性の高い
- methodology：方法論、方法
- recurrent：再発の
- regularization：正則化