---
title:
AI:
published: 2025-1006
description:
tags:
  - permanent-note
  - transformer
---

https://qiita.com/omiita/items/07e69aef6c156d23c538
---
自然言語処理の世界では2018年10月にBERTが出てきてとうとうNLPにもブレイクスルーが来たと話題になった。2019年にはそのBERTを超えるXLNetというモデルが出て来たり、危険すぎて公開禁止になったGPT-2が出て来たりと進歩が止まらない。直近(2019年11月)の[GLUE Benchmark](https://gluebenchmark.com/leaderboard)のリーダーボードを見ると、**いつのまにかNLPでは人間能力を越してしまったT5という強過ぎるモデル** も出て来ていて盛り上がっている。

**どのモデルも「Transformer」をベースにしている** 。

**Transformerを知らずしてNLPは語れない**、ということでこの記事ではこのTransformerが提案された2017年12月の論文「[Attention Is All You Need](https://arxiv.org/abs/1706.03762)」を徹底解説。ちなみにTransformerの技術はNLPだけでなく他分野にも使われ高性能を叩き出しているためこれからのディープラーニングにTransformerは必要不可欠。Transformerのイメージは下図。

[![超単純なTransformer](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimgur.com%2FAEMmTcE.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=56b4d743ddf9259180da9ed29f61c48e)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimgur.com%2FAEMmTcE.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=56b4d743ddf9259180da9ed29f61c48e)

> スペイン語-英語の翻訳タスクで英語の"I"を予測するTransformer.  
> Encoderに原文のEmbeddingを一気に入れて、その出力を一気にDecoderに入れる。  
> あとは推論時はDecoderが通常のSeq2seqのように逐次的に単語を出力していく。

![[Pasted image 20251006212133.png]]

## Abstract 

翻訳などの入力文章を別の文章で出力するというモデル(=Sequence Transduction Model)は[Attention](https://www.slideshare.net/yutakikuchi927/deep-learning-nlp-attention)を用いたエンコーダー-デコーダ形式のRNNやCNNが主流であった。  
本論文ではRNNやCNNを用いず**Attentionのみを用いたモデル、Transformer**を提案。  
Transformerの特徴は以下。

- **再帰も畳み込みも一切使わない。**
- 翻訳で今までのアンサンブルモデルも含めたSoTAを超えるBLEUスコア(28.4)を叩き出した。
- なのに**並列化がかなりしやすく訓練時間が圧倒的に削減**できる。
- それでいて**Transformerは他のタスクにも汎用性が高い**という優れもの。

---
### Attentionとは

Attentionとは簡単に言うと、**文中のある単語の意味を理解する時に、文中の単語のどれに注目すれば良いかを表すスコアのこと** である。例えば英語でitが出て来たら、その単語だけでは翻訳できない。itを含む文章中のどの単語にどれだけ注目すべきかというスコアを表してくれるのがAttention。  
AttentionとはQuery $Q$ とKey $K$  とValue $V$ の3つのベクトルで計算される。各単語がそれぞれのQueryとKeyとValueのベクトルを持っている。より具体的には、Attentionとは $V$ の加重和であり、その加重は $Q$ と $K$ を使って計算される。

**QueryとKeyでAttentionスコアを計算し、そのAttentionスコアを使ってValueを加重和すると、Attentionを適用した単語の潜在表現が手に入る。**


#### 縮小付き内積Attention
$Q, K, V$ の正体はEncoderの最初の段であれば、**入力単語Embedding $X$ にそれぞれ重み $W^Q, W^K, W^V$ を掛け合わせたもの(つまり、 $XW^Q, XQ^K, XW^V$ )。** これがEncoderの2段目以降であれば、前段の出力にその段特有の別の重み $W^Q, W^K, W^V$  を掛けることで手に入れられる。

Attentionは $Q, K, V$ によって計算されると前述したように、縮小付き内積Attentionも同様である。 $Q$ と $K$  は次元 $d_k$ を持ち、 $V$ は次元 $d_v$  を持つ。この縮小付き内積Attentionを文章で表すと、 **クエリと全キーの内積によりその関連度を計算し、 $\sqrt{d_k}$ で割ったのちに、ソフトマックス関数を適用させる** こと。数式は以下。

$$
\text{Attention}(Q, K, V) = \text{softmax}(\frac{QK^T}{\sqrt{d_k}})V 
$$

上式で $V$ を除いた、 $\text{softmax}\frac{QK^T}{\sqrt{d_k}}$ の部分が縮小付き内積Attentionのスコアとなる。ここで $\sqrt{d_k}$ がなければ、普通のAttentionのこと。  

今回のようにクエリとキーによる掛け算でAttentionを算出する方法を **内積Attention** と言い、クエリとキーを入力とした1層の全結合層でAttentionを算出する方法を **加法Attention** という。内積Attentionの方が速いため今回は内積Attentionを使った。また、 **$\sqrt{d_k}$でスケール(縮小)しているのは内積Attentionは  が大きいと加法Attentionよりも性能が劣ってしまうため**。

#### 複数ヘッドAttention

**各単語に対して1組のQuery,Key,Valueを持たせるのではなく、比較的小さいQuery, Key, Valueをヘッドの数分つくり、それぞれのヘッドで潜在表現を計算する。** 最終的にそれらを1つのベクトルに落とすことによって獲得された潜在表現をその単語の潜在表現とする。  
より具体的には、それぞれのheadの潜在表現たちをConcatし、それを重み  で掛け算することで元の次元に戻してその層のOutputとする。この論文ではヘッドの数が8つなので、QueryとKeyとValue用の小さいサイズの重みをそれぞれ8つずつ用意する。計算式で見るとわかりやすい。]

[![複数ヘッドAttention数式](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimgur.com%2FFBEzxBt.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=7a94e5ff620bf8decce99ca5ca025be4)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimgur.com%2FFBEzxBt.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=7a94e5ff620bf8decce99ca5ca025be4)

> Vaswani, A. et al.(2017) Attention Is All You Need

**ヘッドを複数個用意することで、それぞれが異なる潜在表現の空間から有益な情報を取ってきてくれる。**

